# Map/reduce interruption handling
<!-- id: topic:map_reduce.interruptions -->

Part of: [Map/reduce processing model](map_reduce.md)

See also: [MapReduceScript](../script-types/MapReduceScript.md), [Map/reduce governance](map_reduce.governance.md),
[Map/Reduce deployment record](map_reduce.deployment.md), [N/search](../modules/N_search.md).

A map/reduce execution can be cut off part-way. Your code has to cope with being run again for work
that may already have been done.

## What interrupts a script

There are two causes:

1. **Application server disruption**, for example a NetSuite update, maintenance, or a failure of
   the execution environment. The script is restarted automatically, whatever stage it was in, and
   carries on with the stage that was interrupted.
2. **An uncaught error** thrown by your code. In map and reduce this ends only the current
   invocation, not the whole script.

## How the work is split into jobs

- Get input data, shuffle and summarize each run as one job; map and reduce run as many jobs.
- At the start, the number of map jobs equals the deployment's concurrency limit.
- A map job claims a batch of unprocessed pairs and flags them as its own; no other job may take
  them. The batch size is the deployment's buffer size.
- A job may yield any number of times, and every yield adds a new job. Yields happen only between
  function calls, never during one.

## Behaviour after a server disruption

| Stage | What happens |
|---|---|
| getInputData | The function is started again. On a rerun it should return the same data; each result is still sent to map only once. |
| shuffle | The work is simply done again; it only costs time. |
| map / reduce | Whatever the interrupted invocation wrote is discarded. The restarted job first reprocesses the unfinished pairs it had flagged (how many depends on the buffer size). `SSS_APP_SERVER_RESTART` is recorded in the errors of [`mapContext`](../script-types/MapReduceScript.md), [`mapSummary`](../script-types/MapReduceScript.md), [`reduceContext`](../script-types/MapReduceScript.md) and [`reduceSummary`](../script-types/MapReduceScript.md). |
| summarize | The function is started again. |

Flagged pairs are retried after a disruption unless the retry count is set to 0. Without a retry
count, all uncertain pairs are reprocessed.

## Behaviour after an uncaught error

| Stage | What happens |
|---|---|
| getInputData | The invocation ends and the script goes to summarize without restarting. This cannot be configured; the error is in [`inputSummary.error`](../script-types/MapReduceScript.md). |
| map / reduce | The failed invocation's writes are discarded. Then, in order: retry the pair if retries remain; otherwise exit to summarize if `exitOnError` is true; otherwise carry on with the other pairs. |
| summarize | The script stops. This cannot be configured. |

By default (no retry count) the failing pair is not processed again; the invocation ends and the job
moves on to its other pairs.

One exception: when `SSS_USAGE_LIMIT_EXCEEDED` is raised, jobs already processing finish and pending
jobs are cancelled (see [Map/reduce governance](map_reduce.governance.md)).

## Configuration: retryCount, exitOnError and buffer size

`retryCount` and `exitOnError` are set in a `config` block inside the object your script returns.
Buffer size is a field on the deployment record.

- **retryCount** applies to map and reduce and accepts 0 to 3. It is the number of retries per
  uncertain pair and starts again for each new pair. After a server restart it covers every flagged,
  unfinished pair; after an uncaught error it covers only the exact pair that failed. A value of 2
  gives each pair up to three attempts. If you want a pair retried after an uncaught error, you must
  set it.
- **exitOnError** applies only to uncaught errors in map and reduce, and has no effect on server
  restarts. When true, the stage is abandoned and the script goes to summarize once the retries are
  used up. The default is false.
- retryCount is evaluated before exitOnError.
- **Buffer size** sets how many pairs are processed before progress is saved. Keep it at 1.

## What the framework guarantees, and what it does not

- When a piece of work restarts, the map/reduce data it had written internally is deleted
  automatically. When a map invocation is restarted, its writes from the earlier attempt are gone;
  after a reduce restart only the most recent writes for each pair remain. The framework also removes
  the writes of a partially executed batch, so internal data is never written twice and you need not
  check whether a `write` already happened.
- Changes outside the framework, such as records you created or updated, are **never** rolled back.
  A restarted map could, for example, update the same sales order twice. Make those operations
  idempotent.
- [`isRestarted`](../script-types/MapReduceScript.md) on the context is true exactly when the current
  call is part of a restart. It does **not** prove that this particular pair ran before.

## A restart-safe pattern

1. Filter records that were already processed out of the input, so the whole task can be rerun
   safely.
2. In map or reduce, check a "processed" marker only when the context says it is restarted, and use
   [`search.lookupFields`](../modules/N_search.md) for that instead of loading the record.
3. Call `write` outside the restart check, because the earlier attempt's writes were removed.
4. Log restarts in summarize. The summary log entry can show up twice, since summarize itself may be
   restarted.

Check for errors from the input, map and reduce stages in summarize. Error data is available here:

- get input data: [`inputSummary.error`](../script-types/MapReduceScript.md)
- map: [`mapContext.errors`](../script-types/MapReduceScript.md) and [`mapSummary.errors`](../script-types/MapReduceScript.md)
- reduce: [`reduceContext.errors`](../script-types/MapReduceScript.md) and [`reduceSummary.errors`](../script-types/MapReduceScript.md)

> [!SOURCE-ERROR] The source's logging table names the reduce context's errors for the map stage,
> which looks like a copy slip. Use the map context and map summary errors for map-stage failures, as
> listed above, and verify in a test run.
<!-- source-error: topic:map_reduce.interruptions | - | logging_table_lists_reducecontext_errors_for_map_stage -->

## Example

A map-only script marks overdue invoices with a custom checkbox (`custbody_ar_reminder_sent`, a field
you would create yourself). The input skips invoices already marked; on a restart, map checks the
marker cheaply before touching the record, and always writes its output.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType MapReduceScript
 */
define(['N/search', 'N/record', 'N/log'], (search, record, log) => {
  const FLAG = 'custbody_ar_reminder_sent';
  const isChecked = (v) => v === true || v === 'T';

  const getInputData = () => search.create({
    type: search.Type.INVOICE,
    filters: [[FLAG, 'is', 'F'], 'AND', ['amountremaining', search.Operator.GREATERTHAN, 0]],
    columns: ['entity']
  });

  const map = (context) => {
    const invoiceId = context.key;
    let alreadyDone = false;
    if (context.isRestarted) {
      const current = search.lookupFields({ type: search.Type.INVOICE, id: invoiceId, columns: [FLAG] });
      alreadyDone = isChecked(current[FLAG]);
    }
    if (!alreadyDone) {
      record.submitFields({ type: record.Type.INVOICE, id: invoiceId, values: { [FLAG]: true } });
    }
    // Outside the restart check: earlier writes for this pair were discarded.
    context.write({ key: invoiceId, value: 'marked' });
  };

  const summarize = (summary) => {
    if (summary.isRestarted) {
      log.audit({ title: 'Summarize restarted', details: 'This entry may appear twice.' });
    }
    if (summary.inputSummary.error) {
      log.error({ title: 'Input stage failed', details: summary.inputSummary.error });
    }
    summary.mapSummary.errors.iterator().each((key, error, executionNo) => {
      log.error({ title: `Invoice ${key}, attempt ${executionNo}`, details: error });
      return true;
    });
  };

  return {
    config: { retryCount: 2, exitOnError: false },
    getInputData,
    map,
    summarize
  };
});
```
