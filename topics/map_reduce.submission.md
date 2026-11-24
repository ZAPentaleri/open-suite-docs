# Map/Reduce Submission
<!-- id: topic:map_reduce.submission -->

Part of: [Map/reduce processing model](map_reduce.md)

See also: [MapReduceScript](../script-types/MapReduceScript.md), [Map/Reduce Deployment Record](map_reduce.deployment.md),
[SuiteCloud Processors](suitecloud_processors.md), [N/task](../modules/N_task.md).

There are three ways to start a [map/reduce script](../script-types/MapReduceScript.md): on a
schedule, on demand from the UI, or on demand from another script. Whichever route you take, the work
is carried out by [SuiteCloud Processors](suitecloud_processors.md), and every process needs a
deployment record of its own. A script can have several deployments.

## One unfinished instance per deployment

- A deployment can have at most one unfinished instance. It cannot be submitted again until the
  previous instance has finished.
- Submitting from code while an instance is still unfinished raises `MAP_REDUCE_ALREADY_RUNNING`.
  In the UI, Save and Execute is hidden while the deployment has an unfinished instance.
- This rule is independent of the deployment's concurrency limit; raising the limit does not let a
  second instance start.
- Your options: wait until the instance finishes and submit again, or, when the same script must run
  concurrently, create several deployments. Multiple deployments let instances run side by side and
  avoid the failed second submission.
- If a script triggered by transactions does the submitting, restrict that script to the execution
  contexts it actually needs, so that transactions do not cause duplicate submissions.

## On demand from the UI

- The deployment needs status **Not Scheduled** and **Deployed** checked.
- Choose **Save and Execute** from the deployment's Save drop-down.
- On the script record in view mode, the **Deploy Script** button opens a new deployment.
- The run may start after a delay.
- To stop it, clear the **Deployed** box.

## On demand from a script

- Steps: create a deployment with status Not Scheduled and Deployed checked; call
  [`task.create`](../modules/N_task.md#taskcreateoptions) for a map/reduce task; set the script ID;
  optionally set the deployment ID; then call
  [`MapReduceScriptTask.submit()`](../modules/N_task.md#mapreducescripttasksubmit) on the resulting
  [`MapReduceScriptTask`](../modules/N_task.md#mapreducescripttask-object). The deployment's status
  must be Not Scheduled.
- This is available to server-side scripts only.
- Work submitted this way runs as the calling script's user and role. The caller needs the
  Administrator role, or the SuiteScript and SuiteScript Scheduling permissions.
- [`MapReduceScriptTask.deploymentId`](../modules/N_task.md#mapreducescripttaskdeploymentid) is
  optional. Without it, the system chooses an available deployment of the script, meaning one whose
  status is Not Scheduled, whose Deployed box is checked, and which has no unfinished instance. When
  you keep spare deployments for this purpose (each Not Scheduled and Deployed), leave the deployment
  ID out.
- The submission may start after a delay or wait in a queue.

## On a schedule

- Set the status to **Scheduled** and enter times on the Schedule subtab; Deployed must be checked.
- Schedules can be one-time, daily, weekly, monthly or yearly. The shortest interval is 15 minutes.
- The schedule controls when the work is **submitted**, not when it executes.
- Repeats within a day stop at midnight and resume the next day at the start time. To run every hour
  around the clock, for example, use a daily event starting at midnight, repeating every hour, with no
  end date.
- Schedule times are shown in the current user's session time zone; for map/reduce deployments the
  schedule is presented in the viewer's time zone.

> [!SOURCE-ERROR] The submission material ties the displayed schedule time to the current user's
> session, while the deployment-record material says the time zone comes from the user who last
> edited the deployment. The source does not resolve this; check the times shown in your account
> before relying on either.
<!-- source-error: topic:map_reduce.submission | - | schedule_time_zone_current_user_vs_last_editor -->

## Who the work runs as

| Started from | Runs as |
|---|---|
| UI (Save and Execute, or a schedule) | a system user with Administrator permissions |
| A script | the calling script's user and role |

## Example

A custom module that queues a map/reduce run and reports when every deployment is busy. No
deployment ID is passed, so any available deployment of the script is used. The script ID and
parameter name are invented.

```js
/**
 * @NApiVersion 2.1
 */
define(['N/task', 'N/log'], (task, log) => {
  const queueRebateRun = (periodEnd) => {
    const mrTask = task.create({
      taskType: task.TaskType.MAP_REDUCE,
      scriptId: 'customscript_vendor_rebate_mr',
      params: { custscript_rebate_period_end: periodEnd }
    });
    try {
      return mrTask.submit();
    } catch (e) {
      if (e.name === 'MAP_REDUCE_ALREADY_RUNNING') {
        log.audit({ title: 'Rebate run skipped', details: 'Every deployment still has an unfinished instance.' });
        return null;
      }
      throw e;
    }
  };

  return { queueRebateRun };
});
```
