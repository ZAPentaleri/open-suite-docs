# ScheduledScript
<!-- id: script:ScheduledScript -->

A scheduled script is server-side code that runs as a background job, either at times set on its
deployment or when something asks for it on demand. Use it for tasks that should happen on a timetable
or be pushed off the user's request: periodic logging, deferred maintenance, creating and later
purging temporary records, or asynchronous work handed over by another server script.

## At a glance

| Item | Value |
|---|---|
| `@NScriptType` | `ScheduledScript` |
| Usage-unit limit per execution | 10,000 |
| Time limit | 3,600 seconds |
| Runs on | Server, on SuiteCloud Processors |
| Jobs per script instance | 1 |
| Needs | A scheduled script deployment record |
| SuiteScript 1.0 counterpart | Scheduled Scripts ([1.0 concepts](../topics/ss1_api.scheduled_scripts.md)) |
| Entry points | [`execute`](#executecontext) |

Related pages: [deployment](../topics/scheduled_script.deployment.md),
[status page](../topics/scheduled_script.status_page.md), [SDF object](../topics/sdf.scheduledscript.md),
[script deployment](../topics/script_deployment.md), [N/task](../modules/N_task.md),
[N/runtime](../modules/N_runtime.md), [MapReduceScript](MapReduceScript.md).

## How it runs

**When to choose it.** Pick a scheduled script for work that runs on a schedule or on demand. Avoid it
for big-data jobs and long-running operations: for large data sets or many records, a
[map/reduce script](MapReduceScript.md) is the better tool because it processes in parallel and
handles governance better. Many jobs that were written as scheduled scripts in SuiteScript 1.0 now
fit map/reduce. The structural difference is that a scheduled script instance is one job, whereas
map/reduce splits work across many.

**No yielding, no recovery points.** A scheduled script cannot set recovery points or yield, and
there is no replacement for the 1.0 `nlapiYieldScript` and `nlapiSetRecoverPoint`. It has to finish
its work inside one execution's 10,000 units.

**Restarts.** If the server restarts mid-run, the script starts again from the beginning once
resources are available; there is no manual yield or recovery point to resume from. An update that is
not idempotent can then be applied twice. Make each operation idempotent, or detect and fix
duplicates, and in general write the script so it handles interruptions and recovers. A robust shape
is:

1. a custom checkbox field marks a record as processed (the source's example uses a transaction body
   field, `custbody_...`, that you create in the UI first);
2. the search that selects work excludes records with the box ticked;
3. the script ticks the box as soon as a record is done.

A rerun then simply picks up what is left. You can tell that a run is a restart because
`context.type` equals `ABORTED` (see [InvocationType](#invocationtype)).

**How it gets submitted.** Every scheduled script needs a script deployment record, and how it
behaves depends on the deployment type. There are four ways to start one:

- *One-time, in the future*, or *recurring*: set a schedule on the deployment record.
- *On demand from the UI*: Save and Execute on the deployment (the option appears in the Save
  drop-down). This works when the deployment status is Not Scheduled or Testing. A script submitted or
  scheduled from the UI runs as a system user with administrator permissions.
- *On demand from a script*: with [`task.create`](../modules/N_task.md) and a
  [`ScheduledScriptTask`](../modules/N_task.md). Deploy the script with status Not Scheduled (and
  Deployed checked), create a scheduled-script task, set its script ID and deployment ID, and submit
  it. A script-submitted run uses the calling script's user and role.

For any on-demand submission the deployment must have Deployed checked, and you cannot submit it on
demand while an earlier instance of it is still unfinished. The source does not state which error
code, if any, that attempt raises; the only error it documents for `ScheduledScriptTask.submit` is
`FAILED_TO_SUBMIT_JOB_REQUEST_1`, for a failed operation in general. Submission is not immediate: the
script may start after a delay and waits behind scripts already queued.

**Dispatch order.** Jobs go to processors in order of priority, then submission time. SuiteCloud
Processors' priority elevation can also change the order.

**Queues (older deployments).** Deployments created before SuiteCloud Processors still show a Queue
field. A queue processes its jobs first-in, first-out from that specific queue, but queued and
unqueued jobs share one pool of processors. Where queues are still used, a high-priority queue can
keep a processor ready, and a low-priority queue can make jobs run one at a time; keep the same
priority for all jobs in one queue. Removing the queue from a deployment makes it stop using the queue
and cannot be undone; update and test any script that relies on FIFO order before you do it. If
strict ordering is needed, have the first script submit the second through
[`ScheduledScriptTask`](../modules/N_task.md).

**Debugging and monitoring.** Debug with the SuiteScript Debugger, or with deployed debugging when the
deployment status is Testing. Progress is shown on the
[Scheduled Script Status page](../topics/scheduled_script.status_page.md).

**Managing scripts.** Scheduled scripts can be managed with SDF, copied between accounts with Copy to
Account (the control is at the top right of the script page), and reviewed with SuiteScript analysis
tooling.

## Entry points

### execute(context)
<!-- id: script:ScheduledScript.execute -->

Runs when the scheduled script is triggered. NetSuite passes the context object automatically.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `context` | Object | Yes | |
| `context.type` | string | - | |

`context.type` exists since 2015.2.

- `context.type` says how this run was started; its values come from
  [`InvocationType`](#invocationtype). `ABORTED` signals a rerun after an interruption.

**Returns:** `void`

**Governance:** the whole execution may use up to 10,000 usage units.

## Enums

### InvocationType
<!-- id: script:ScheduledScript.InvocationType -->

Lists the execution contexts a scheduled script run can come from; `context.type` holds one of these.
In the entry point it is reached as `context.InvocationType`, so compare
`context.type === context.InvocationType.ABORTED` rather than hard-coding text. The source gives the
value names only; it does not state the literal strings behind them.

| Value | Notes |
|---|---|
| `SCHEDULED` | A normal run triggered by the deployment's schedule |
| `ON_DEMAND` | Submitted by another script through [`ScheduledScriptTask.submit`](../modules/N_task.md); requires the deployment status Not Scheduled |
| `USER_INTERFACE` | Started by clicking Save and Execute |
| `ABORTED` | Rerun because the system went down while the script was executing |
| `SKIPPED` | Run after downtime caused the scheduled time to be missed |

## Examples

Nightly collections follow-up. For each invoice that still has an amount due and has not been handled
yet, create a task for the sales rep and tick a custom checkbox on the invoice
(`custbody_collections_task_made`, created in the UI). Because the search skips ticked invoices, a
restart or an early stop for low governance just leaves the remainder for the next run. A restart
between creating the task and ticking the box could still create one duplicate task, so the task
title carries the invoice number to make duplicates easy to spot, and restarts are logged.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType ScheduledScript
 */
define(['N/search', 'N/record', 'N/runtime', 'N/log'], (search, record, runtime, log) => {
  const DONE_FLAG = 'custbody_collections_task_made';
  const USAGE_RESERVE = 200;
  const DAY_MS = 24 * 60 * 60 * 1000;

  const execute = (context) => {
    if (context.type === context.InvocationType.ABORTED) {
      log.audit({ title: 'Collections run restarted', details: 'Check for a duplicate task on the last invoice.' });
    }
    const script = runtime.getCurrentScript();
    let created = 0;
    let stoppedEarly = false;

    const openInvoices = search.create({
      type: search.Type.INVOICE,
      filters: [
        [DONE_FLAG, search.Operator.IS, 'F'],
        'AND',
        ['amountremaining', search.Operator.GREATERTHAN, '0']
      ],
      columns: ['tranid', 'entity', 'salesrep']
    });

    openInvoices.run().each((result) => {
      if (script.getRemainingUsage() < USAGE_RESERVE) {
        stoppedEarly = true;
        return false;
      }
      const invoiceNo = result.getValue({ name: 'tranid' });
      const followUp = record.create({ type: record.Type.TASK });
      followUp.setValue({ fieldId: 'title', value: `Collect balance on invoice ${invoiceNo}` });
      followUp.setValue({ fieldId: 'company', value: result.getValue({ name: 'entity' }) });
      followUp.setValue({ fieldId: 'transaction', value: result.id });
      followUp.setValue({ fieldId: 'duedate', value: new Date(Date.now() + 3 * DAY_MS) });
      const rep = result.getValue({ name: 'salesrep' });
      if (rep) {
        followUp.setValue({ fieldId: 'assigned', value: rep });
      }
      followUp.save();

      record.submitFields({
        type: record.Type.INVOICE,
        id: result.id,
        values: { [DONE_FLAG]: true }
      });
      created += 1;
      return true;
    });

    log.audit({
      title: `Collections run (${context.type})`,
      details: `${created} tasks created${stoppedEarly ? '; stopped early to stay under the usage limit' : ''}`
    });
  };

  return { execute };
});
```

Queue the job on demand from another server script. Here a user event script hands a freshly saved
estimate's ID to a scheduled script deployment whose status is Not Scheduled. Submission is refused
while a previous instance of that deployment is still unfinished (the source does not name the error
for that case), so the call is guarded. The script, deployment and parameter IDs are examples.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType UserEventScript
 */
define(['N/task', 'N/log'], (task, log) => {
  const afterSubmit = (context) => {
    if (context.type !== context.UserEventType.CREATE) {
      return;
    }
    const job = task.create({
      taskType: task.TaskType.SCHEDULED_SCRIPT,
      scriptId: 'customscript_estimate_pricing_check',
      deploymentId: 'customdeploy_estimate_pricing_ondemand',
      params: { custscript_estimate_to_check: String(context.newRecord.id) }
    });
    try {
      const taskId = job.submit();
      log.audit({ title: 'Pricing check queued', details: taskId });
    } catch (e) {
      log.error({ title: 'Could not queue pricing check', details: `${e.name}: ${e.message}` });
    }
  };

  return { afterSubmit };
});
```
