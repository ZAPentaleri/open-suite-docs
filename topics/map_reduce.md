# Map/reduce processing model
<!-- id: topic:map_reduce -->

Sub-topics:

- [Map/Reduce Deployment Record](map_reduce.deployment.md)
- [Map/reduce governance](map_reduce.governance.md)
- [Map/reduce interruption handling](map_reduce.interruptions.md)
- [Map/Reduce Status Page](map_reduce.status_page.md)
- [Map/Reduce Submission](map_reduce.submission.md)
- [Map/Reduce Testing](map_reduce.testing.md)
- [Map/reduce yielding](map_reduce.yielding.md)

See also: [MapReduceScript](../script-types/MapReduceScript.md), [SuiteCloud Processors](suitecloud_processors.md),
[ScheduledScript](../script-types/ScheduledScript.md).

A [map/reduce script](../script-types/MapReduceScript.md) breaks a big job into many small pieces of
work that do not depend on each other. You supply the logic for one piece; the platform carries the
data from stage to stage and calls your functions as many times as needed. Write each map or reduce
call the way you would write the body of a loop: short, and about one item.

## The five stages

An execution always passes through the stages in this order, and no stage starts until the previous
one is completely done:

| # | Stage | Entry point | How it runs |
|---|---|---|---|
| 1 | Get input data | [`getInputData`](../script-types/MapReduceScript.md#getinputdatacontext) | serial: one job, one invocation |
| 2 | Map | [`map`](../script-types/MapReduceScript.md#mapcontext) | parallel; one invocation per key/value pair |
| 3 | Shuffle | none (system only) | serial: one job, one invocation |
| 4 | Reduce | [`reduce`](../script-types/MapReduceScript.md#reducecontext) | parallel; one invocation per key left after shuffle |
| 5 | Summarize | [`summarize`](../script-types/MapReduceScript.md#summarizecontext) | serial: one job, one invocation |

- **Get input data** is the one stage you must always provide.
- You also need **map, reduce, or both**. One of the two may be left out, never both, so every
  execution uses at least two of your stages.
- **Summarize** is optional and sits outside the main processing. Typical uses: summarizing what the
  run produced, writing a file, or sending an email.
- **Shuffle** belongs to the system alone. It has no entry point, and your script cannot reach it.

### What each stage receives

- Map runs once per key/value pair produced by get input data; each invocation sees exactly one
  pair. Twenty input pairs therefore give twenty map invocations.
- Shuffle takes the pairs written by map (or, when there is no map function, the pairs from get
  input data), groups them by key automatically, and turns the values belonging to each key into an
  array.
- Reduce runs once per key that comes out of shuffle; each invocation gets one key together with all
  of its values.
- Map output travels on to shuffle and then reduce when a reduce function is defined; otherwise it
  goes on to summarize.
- When get input data hands back a search, each pair's key is the record's internal ID and its value
  is a JSON string of the result's field IDs and their values.

Worked illustrations:

- One hundred pairs that belong to two employees, ninety for one and ten for the other, leave shuffle
  as just two keys.
- Five unpaid invoices spread over three customers: map runs five times and reduce three times. A
  design for that job loads the unpaid invoices in get input data, maps each invoice to its customer,
  creates one payment per customer in reduce, and emails run metrics from summarize.

The context object handed to each entry point carries that invocation's data, plus information about
errors and usage statistics.

## Keys, values and serialization

- Between stages, a key or value that is not already a string is converted with `JSON.stringify`.
- Nothing is converted back automatically. Call `JSON.parse` on a value yourself when you need the
  object again.
- Data that is not JSON, for example CSV or XML text, can cause errors when it passes through this
  handling.
- A key may be at most 3,000 characters
  ([`mapContext.key`](../script-types/MapReduceScript.md#mapcontextkey),
  [`reduceContext.key`](../script-types/MapReduceScript.md#reducecontextkey)); a longer key raises
  `KEY_LENGTH_IS_OVER_3000_BYTES`.

> [!SOURCE-ERROR] The limit is stated in characters, yet the error code that enforces it speaks of
> bytes. The source does not settle which unit applies; with multi-byte text, stay well below 3,000
> to be safe on either reading.
<!-- source-error: topic:map_reduce | - | key_limit_in_characters_vs_error_code_says_bytes -->

- A value may be at most 10 MB; a larger one raises `VALUE_LENGTH_IS_OVER_10_MB`.
- When keys or values are built dynamically before
  [`mapContext.write`](../script-types/MapReduceScript.md#mapcontextwriteoptions) or
  [`reduceContext.write`](../script-types/MapReduceScript.md#reducecontextwriteoptions), check their
  length first.
- Carry the payload in the value, not in the key.

## Jobs and processors

Map/reduce scripts are executed as jobs on [SuiteCloud Processors](suitecloud_processors.md). In this
context a processor is the same thing as a single processing thread.

- Every stage gets at least one job, so one submitted task needs at least five jobs, one per stage.
- Get input data, shuffle and summarize are serial: each is a single job doing a single invocation.
- Map and reduce are parallel: their jobs run independently of each other at the same time.
- That is the key difference from a [scheduled script](../script-types/ScheduledScript.md), which
  uses only one job.
- Job priority is set either on the script deployment record or on the SuiteCloud Processors
  priority settings page.
- The size of the processor pool depends on the account's licensing. How many of those processors a
  deployment may use is set by its concurrency limit field (see
  [Map/Reduce Deployment Record](map_reduce.deployment.md)).
- A single deployment can have no more than one unfinished instance at a time.

## Yielding, limits and interruptions

One benefit of the model is that the script manages its own resource use by yielding (see
[Map/reduce yielding](map_reduce.yielding.md)). When a job has been running long, it ends after its
current invocation and a fresh job picks up the rest. A yield never interrupts an invocation that is
under way, and no API exists to force one. When the follow-up job really starts depends on its
priority and its submission time.

Yielding does not manage usage limits; those are enforced separately (see
[Map/reduce governance](map_reduce.governance.md)).

An execution can also be interrupted by server problems or by uncaught errors; see
[Map/reduce interruption handling](map_reduce.interruptions.md).

## Example

Add up the open balance per sales rep. Get input data returns plain objects, so map must parse its
value, and reduce parses each grouped value once more before summing.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType MapReduceScript
 */
define(['N/search', 'N/log'], (search, log) => {
  const getInputData = () => {
    const rows = [];
    search.create({
      type: search.Type.INVOICE,
      filters: [['amountremaining', search.Operator.GREATERTHAN, 0]],
      columns: ['salesrep', 'amountremaining']
    }).run().each((result) => {
      rows.push({
        invoiceId: result.id,
        repId: result.getValue({ name: 'salesrep' }) || 'none',
        open: Number(result.getValue({ name: 'amountremaining' }))
      });
      return true;
    });
    return rows;
  };

  const map = (context) => {
    const invoice = JSON.parse(context.value);
    // Short key; the payload rides in the value.
    context.write({ key: String(invoice.repId), value: { id: invoice.invoiceId, open: invoice.open } });
  };

  const reduce = (context) => {
    const total = context.values
      .map((raw) => JSON.parse(raw))
      .reduce((sum, inv) => sum + inv.open, 0);
    context.write({ key: context.key, value: total.toFixed(2) });
  };

  const summarize = (summary) => {
    summary.output.iterator().each((repId, total) => {
      log.audit({ title: `Sales rep ${repId}`, details: `Open balance ${total}` });
      return true;
    });
  };

  return { getInputData, map, reduce, summarize };
});
```
