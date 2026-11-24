# SuiteScript 1.0 scheduled scripts
<!-- id: topic:ss1_api.scheduled_scripts -->

> **Legacy material.** This page exists so that 1.0 scripts can be read, converted and minimally
> repaired. Do not write new 1.0 code; the 2.1 counterparts are
> [ScheduledScript](../script-types/ScheduledScript.md) and, for work that must yield,
> [MapReduceScript](../script-types/MapReduceScript.md).

Part of: [SuiteScript 1.0 API conventions](ss1_api.md)

## Queues

- Scheduled scripts wait in queues that run first in, first out for the whole company.
- Each queue runs one script at a time.
- An account has 1 queue by default; SuiteCloud Plus raises that to 5.
- The times on a deployment's schedule are the times the script is **queued**, not the times it
  starts running.

## Usage limits and requeueing

**Governance:** 10,000 usage units per execution.

- Every queued run gets its own 10,000-unit allowance.
- A script that is close to its limit should requeue itself with
  [`nlapiScheduleScript`](../ss1/search-scheduling-context.md) and stop.
- `nlapiScheduleScript` only works for a deployment whose status is not Scheduled.
- It can be called from scheduled scripts, user event scripts and Suitelets.

## The `type` argument

NetSuite passes the entry function one argument that says why the run started:

| `type` value | Why the script is running |
|---|---|
| `scheduled` | The deployment's schedule |
| `ondemand` | A call to `nlapiScheduleScript` |
| `userinterface` | A user clicked Save and Execute on the deployment |
| `aborted` | A rerun of a run that was aborted |
| `skipped` | A run that was missed during downtime and runs afterwards |

## Recovery points and yielding

- [`nlapiSetRecoveryPoint`](../ss1/search-scheduling-context.md) saves the script's state;
  [`nlapiYieldScript`](../ss1/search-scheduling-context.md) saves it and gives up the queue. After an
  interruption or a yield, the script restarts from its last recovery point.
- On resume the usage allowance starts again, but memory does not: whatever the script holds in
  memory comes back with it.
- The memory limit is 50 MB. If the script is over it when it yields or sets a recovery point, it
  fails with `SS_EXCESSIVE_MEMORY_FOOTPRINT`.
- To stay small, copy the values you need from records and search results into plain objects and
  drop the originals.

Rough memory costs for sizing a script:

| Item | Approximate size |
|---|---|
| Empty function | 111 bytes |
| One instruction | 32 bytes |
| Local variable | 32 bytes |
| Number | 32 bytes |
| String | 32 bytes plus 4 per character |
| Empty object | 32 bytes |
| Customer record, standard mode | 40 KB |
| Customer record, dynamic mode | 460 KB |

## Converting

2.x has no recovery points or yielding for scheduled scripts; a restarted 2.x scheduled script
begins again from the start. Work that needs to yield belongs in a map/reduce script. See
[Converting SuiteScript 1.0 scripts](ss21_transition.from_ss1.md).

## Example (1.0, for reading legacy code)

A scheduled function that flags overdue tasks. It keeps only IDs in memory, skips tasks already
flagged so a requeued run picks up where the last one stopped, and requeues itself when the
allowance runs low. The custom field ID is illustrative.

```js
function flagOverdueTasks(type) {
  var ctx = nlapiGetContext();
  var filters = [
    new nlobjSearchFilter('duedate', null, 'before', 'today'),
    new nlobjSearchFilter('custevent_overdue_flag', null, 'is', 'F')
  ];
  var rows = nlapiSearchRecord('task', null, filters, null) || [];
  var ids = [];
  for (var i = 0; i < rows.length; i++) {
    ids.push(rows[i].getId());
  }
  for (var j = 0; j < ids.length; j++) {
    if (ctx.getRemainingUsage() < 200) {
      nlapiScheduleScript(ctx.getScriptId(), ctx.getDeploymentId());
      return;
    }
    nlapiSubmitField('task', ids[j], 'custevent_overdue_flag', 'T');
  }
  nlapiLogExecution('AUDIT', 'Overdue run finished', 'type=' + type + ', tasks=' + ids.length);
}
```
