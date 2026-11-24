# SuiteCloud Processors
<!-- id: topic:suitecloud_processors -->

Part of: [SuiteScript Developer Guide](developer_guide.md)

See also: [ScheduledScript](../script-types/ScheduledScript.md), [MapReduceScript](../script-types/MapReduceScript.md),
[Map/reduce processing model](map_reduce.md), [Scheduled script deployments](scheduled_script.deployment.md),
[N/task](../modules/N_task.md).

SuiteCloud Processors is the system that executes [scheduled scripts](../script-types/ScheduledScript.md)
and [map/reduce scripts](../script-types/MapReduceScript.md). It took over from the older
first-in, first-out scheduling queues. Instead of tying work to a fixed queue, a scheduler orders
jobs by priority, submission time and your preferences. The payoff is automatic load balancing,
fewer bottlenecks and less need for manual intervention.

## Vocabulary

| Term | Meaning |
|---|---|
| Task | One submitted script instance. It is handled by one or more jobs. |
| Job | A unit of work that runs on one processor. |
| Processor | A single processing thread. |
| Processor pool | All the processors an account has. |
| Scheduler | The component that decides the order in which jobs go to processors. |

- A scheduled script task is a single job. A map/reduce task has one job per stage (get input,
  shuffle, summarize, map, reduce), with at least one map or reduce job.
- The processing environment a job lands in does not change how long the job takes.

## Submitting work

A task reaches the processors in one of three ways: from the deployment's schedule, with Save and
Execute, or through the API with [`task.ScheduledScriptTask`](../modules/N_task.md#scheduledscripttask-object)
or [`task.MapReduceScriptTask`](../modules/N_task.md#mapreducescripttask-object). The permissions the
source lists for this are Documents and Files, SuiteScript, and SuiteScript Scheduling at the Full
level.

## Pool size and licensing

- Without SuiteCloud Plus an account has 2 processors (the default); with SuiteCloud Plus it has
  between 2 and 60. Some processor features also require SuiteCloud Plus.
- The allotments by licence are in
  [catalog:suitecloud_processor_allotment](../gen/catalogs/suitecloud_processor_allotment.md). The
  allotment does not apply to deployments that still use queues.
- A second processor doubles the bandwidth, but it also means jobs no longer run strictly one after
  the other. Scripts that depend on running in a certain order can break once two processors are in
  play.

## Priority

- Levels: high, standard (the default) and low. They are configured on the deployment record or on
  the Priority Settings page.
- High-priority jobs are assigned first, in submission order. Low-priority jobs run after the
  available standard jobs. Jobs with the same priority are sent in submission order. For instance,
  if jobs 1 to 5 are submitted with priorities high, standard, low, high, standard, they go out as 1
  and 4, then 2 and 5, then 3.
- A low-priority job can still run ahead of standard jobs when those standard jobs are blocked.
- Changing priorities reorders jobs that are waiting.
- With one processor and equal priorities, jobs run first in, first out.
- Use low for jobs that can tolerate a longer wait.
- Job order is also influenced by priority elevation and processor reservation, below.

### Priority elevation

Elevation raises the priority of low and standard jobs that have been waiting. Configure it at
Setup > Preferences > SuiteCloud Processors.

| Option | Behaviour |
|---|---|
| None | The default; no elevation. |
| Moderate | Priority goes up one step every 4 hours, so a low job reaches high after 8 hours. |
| Intensive | Priority goes up one step every hour, so a low job reaches high after 2 hours. |
| Custom | You choose the time interval. |

- The interval is counted from the job's submission.
- A job sent to a processor before its interval has passed keeps its original priority.
- The time interval can be edited only with the custom option; the **Advanced** link reveals the
  custom option and its interval.
- Turn elevation on when lower-priority jobs wait too long.

### Processor reservation

Reservation keeps processors free for high-priority jobs. It needs SuiteCloud Plus, is disabled by
default, and is found at Setup > Preferences > SuiteCloud Processors > Advanced.

- You can reserve all processors but one.
- Low and standard jobs need an unreserved processor. Worked figure from the source: with 10 of 25
  processors reserved, lower-priority jobs need 11 free processors.
- Reserving processors reduces the throughput of standard and low work.
- The number of reserved processors grows by the number of high-priority jobs waiting, up to the
  limit you set.
- A change applies to jobs that have not started yet, including jobs created by yields.
- After a change, you cannot decrease the setting again for 24 hours.

**Reuse idle processors.** This option releases reservations that went unused for 24 hours. It is
driven by an hourly audit that works with five figures:

| Variable | Meaning |
|---|---|
| A | total processors available |
| B | the "number of processors reserved" setting |
| C | the most jobs running at the same time in the last 24 hours |
| D | the most high-priority jobs that waited more than 1 minute in the last hour |
| E | processors currently reserved |

- When peak use stays below the total (C below A), lower-priority jobs gain A minus C processors.
- When E is below B and D is positive, the reservation grows by D, but never past B.

## Moving off legacy queues

- New scheduled script deployments have no queue. The Queue field is kept for first-in, first-out
  behaviour, but every deployment shares the same processor pool.
- Removing a queue cannot be undone. Test scripts that rely on first-in, first-out order before you
  remove queues, and give jobs that shared a queue the same priority.
- In a legacy setup, a dedicated high-priority queue kept a processor available for urgent work,
  and a shared low-priority queue ran one job at a time.
- On SuiteCloud Plus, a map/reduce deployment's concurrency limit takes over the role of queues (the
  Concurrency Limit field replaced the Queues field). For legacy map/reduce deployments the limit was
  derived from the old queue count: queues 1, 3, 7 and 9 become a concurrency of 4, and "select all"
  becomes an empty limit, which means all processors.
- With queues gone, the serial stages no longer need duplicate jobs. When duplicates exist, the
  first get-input job cancels the others, and one map job may finish the stage and cancel the
  remaining ones. The source illustrates this with three processors; real accounts have more.
- If one script must follow another, have the first script submit the second through
  `ScheduledScriptTask`.

### Priority Settings page

Found at Customization > Scripting > Priority Settings, it is used for bulk priority changes and for
removing queues. Deployment priority can be set here or on the deployment record.

- **Submit** saves your changes; **Reset Priorities** sets every deployment to standard.
- Removed queues appear as removed only after you submit.
- The Remove Queue and Queue columns apply to scheduled script deployments; the Concurrency Limit
  column applies to map/reduce deployments on SuiteCloud Plus.
- The page shows a deployment's internal ID (for example `345`) and its ID (for example
  `customdeploy_testscript1`) in separate columns.

## Keeping order-dependent scripts in order

If one script has to finish before another starts, do not count on submission order. Find the
scripts that depend on order, and have the first one submit the next through the task API when it is
done: from a map/reduce script, submit a `MapReduceScriptTask` at the end of summarize; from a
scheduled script, submit a `ScheduledScriptTask` at the end of the script.

## Example

A nightly scheduled script that, as its last step, starts the map/reduce script that depends on its
output. The script IDs are invented.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType ScheduledScript
 */
define(['N/task', 'N/log'], (task, log) => {
  const stageExchangeRates = () => {
    // ... prepare the data the follow-up job needs ...
    log.debug({ title: 'Rates staged', details: new Date().toISOString() });
  };

  const execute = () => {
    stageExchangeRates();
    const followUp = task.create({
      taskType: task.TaskType.MAP_REDUCE,
      scriptId: 'customscript_revalue_open_orders_mr'
    });
    const taskId = followUp.submit();
    log.audit({ title: 'Follow-up queued', details: taskId });
  };

  return { execute };
});
```
