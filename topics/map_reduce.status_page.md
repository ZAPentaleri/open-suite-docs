# Map/Reduce Status Page
<!-- id: topic:map_reduce.status_page -->

Part of: [Map/reduce processing model](map_reduce.md)

See also: [Scheduled Script Status Page](scheduled_script.status_page.md),
[Map/Reduce Deployment Record](map_reduce.deployment.md), [Map/Reduce Submission](map_reduce.submission.md).

The Map/Reduce Script Status page lives at Customization > Scripting > Map/Reduce Script Status and
tracks how far submitted map/reduce work has got. On this page a **script instance** means one
submitted task.

## What it shows

- Instances that are pending, in progress, or unable to complete.
- Entries stay available for 30 days.
- The page reports progress. That is not the same as the Status field on the deployment, which
  describes the kind of deployment (scheduled, not scheduled, testing).
- While every task of an instance is still pending, you can cancel the deployment from here.

## Drilling into an instance

- **View Details** lists the jobs that make up the instance. Get input data, shuffle and summarize
  have one job each; the number of map and reduce jobs depends on the concurrency limit and on how
  many times the jobs yielded.
- From there you can drill down into the map stages, processor utilization and timing.
- You can open the deployment from the page, for example to adjust its concurrency limit.

## Reading the same data from a script

The spec names these building blocks for getting instance progress in code:

- [`search.create`](../modules/N_search.md) run against the
  [scheduled script instance record](../gen/records/scheduledscriptinstance.md) (search type
  `SCHEDULED_SCRIPT_INSTANCE`), which lists instances much as the page does;
- the [`task.MapReduceScriptTask`](../modules/N_task.md#mapreducescripttask-object) object you
  submitted, whose task ID you pass to
  [`task.checkStatus`](../modules/N_task.md#taskcheckstatusoptions); for a map/reduce task that call
  returns a [`task.MapReduceScriptTaskStatus`](../modules/N_task.md#mapreducescripttaskstatus-object).

### What MapReduceScriptTaskStatus gives you

[`MapReduceScriptTaskStatus`](../modules/N_task.md#mapreducescripttaskstatus-object) is the
programmatic counterpart of one row on this page. It is server-side only and exists since 2015.2.
Its properties are read-only; assigning `taskId`, `deploymentId`, `scriptId` or `status` raises
`READ_ONLY_PROPERTY`.

| Member | What it tells you | Governance |
|---|---|---|
| `taskId` | ID of the submitted task | |
| `deploymentId` | script ID of the deployment | |
| `scriptId` | internal ID of the script (a number) | |
| `status` | a [`task.TaskStatus`](../modules/N_task.md#tasktaskstatus) value: `PENDING`, `PROCESSING`, `COMPLETE` or `FAILED` | |
| `stage` | the current stage, a [`task.MapReduceStage`](../modules/N_task.md#taskmapreducestage) value: `GET_INPUT`, `MAP`, `SHUFFLE`, `REDUCE` or `SUMMARIZE` | |
| `getPercentageCompleted()` | percent complete of the current stage; for get input data and summarize it is only ever 0 or 100 | 10 units |
| `getTotalMapCount()` / `getPendingMapCount()` | rows fed into map / rows map has not processed yet | 10 units each |
| `getTotalReduceCount()` / `getPendingReduceCount()` | rows fed into reduce / rows reduce has not processed yet | 10 units each |
| `getTotalOutputCount()` / `getPendingOutputCount()` | pairs passed to summarize / rows not processed yet | 10 units each |
| `getPendingMapSize()` / `getPendingReduceSize()` | bytes map / reduce still has to process | 25 units each |
| `getPendingOutputSize()` | bytes of pairs written as output (see the N/task page for a caveat on this description) | 25 units |
| `getCurrentTotalSize()` | bytes of work in progress currently stored | 25 units |

The page's pending / in progress / cannot complete states and the `status` property describe similar
things, but the source does not map one set onto the other.

## Example

A custom module that reports on a map/reduce task you submitted earlier, given the ID that
`submit()` returned. It reads the size figures only while the task is processing, since those calls
cost more.

```js
/**
 * @NApiVersion 2.1
 */
define(['N/task', 'N/log'], (task, log) => {
  const reportProgress = (taskId) => {
    const st = task.checkStatus({ taskId });
    if (st.status === task.TaskStatus.PROCESSING) {
      const pendingKb = Math.round(st.getPendingMapSize() / 1024);
      log.audit({
        title: `Task ${st.taskId} in ${st.stage}`,
        details: `${st.getPercentageCompleted()}% of stage done; ${st.getPendingMapCount()} map rows (${pendingKb} KB) left`
      });
    } else {
      log.audit({ title: `Task ${st.taskId}`, details: `Status ${st.status}` });
    }
    return st.status;
  };

  return { reportProgress };
});
```

To list several instances at once instead, search the scheduled script instance record. The field
IDs below come from that record's page.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType ScheduledScript
 */
define(['N/search', 'N/log'], (search, log) => {
  const execute = () => {
    const results = search.create({
      type: search.Type.SCHEDULED_SCRIPT_INSTANCE,
      columns: [
        search.createColumn({ name: 'datecreated', sort: search.Sort.DESC }),
        'status',
        'percentcomplete',
        'startdate',
        'enddate'
      ]
    }).run().getRange({ start: 0, end: 20 });

    results.forEach((row) => {
      log.debug({
        title: `Instance created ${row.getValue({ name: 'datecreated' })}`,
        details: `${row.getValue({ name: 'status' })}, ${row.getValue({ name: 'percentcomplete' })}`
      });
    });
  };

  return { execute };
});
```
