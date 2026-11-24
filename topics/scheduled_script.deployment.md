# Scheduled script deployments
<!-- id: topic:scheduled_script.deployment -->

See also: [ScheduledScript](../script-types/ScheduledScript.md),
[`task.ScheduledScriptTask`](../modules/N_task.md),
[Scheduled script status page](scheduled_script.status_page.md), [SuiteCloud Processors](suitecloud_processors.md),
[Script Deployment](script_deployment.md), [scriptdeployment record](../gen/records/scriptdeployment.md).

A [scheduled script](../script-types/ScheduledScript.md) cannot run until it has at least one
deployment record. The **Deploy Script** button on the script record opens a new deployment record
for it. Every instance of a SuiteScript 2.1 scheduled script is handled as a single processor job
(one job per instance).

## Identity

- Once the deployment exists, its link to the script record cannot be edited.
- Deployment IDs always begin with `customdeploy`. If the deployment will go into a bundle, give it
  your own ID in lowercase with underscores instead of accepting a generated one. The ID can be
  changed afterwards through Change ID.

## Deployed and Status

Clearing **Deployed** switches the deployment off completely: it does not run even when it has a
schedule, the Save and Execute action is hidden, and scripts cannot submit it.

| Status | Effect |
|---|---|
| Testing | Only the owner can run it, and only in the debugger. |
| Scheduled | Runs on its schedule; on-demand runs are not allowed. |
| Not Scheduled | Can be run on demand, with Save and Execute or from code through [`task.ScheduledScriptTask`](../modules/N_task.md). |

- Pick Scheduled once testing is finished, when the script does not need to start immediately, or
  when it should run at regular intervals.
- A recurring deployment keeps the Scheduled status after each run; it does not flip to another
  status.
- At most one on-demand instance of a deployment runs at any time.
- The **See Instances** link opens the [status page](scheduled_script.status_page.md) already
  filtered to this deployment.

## Other fields

- **Log Level**: debug, audit, error or emergency. Debug is meant for testing; use audit, error or
  emergency in production.
- **Execute As Role**: fixed to Administrator; it cannot be changed.
- **Priority**: high, standard (the default) or low. High-priority jobs are sent to a processor first,
  standard ones after high, and low ones after both high and standard. See
  [SuiteCloud Processors](suitecloud_processors.md).
- **Queue**: a deprecated field. The **Remove Queue** button takes it off the deployment.

## Schedule subtab

The Schedule subtab appears whatever the status is, but a schedule only takes effect when the status
is Scheduled **and** Deployed is checked.

- Schedule kinds: one-time (single), daily, weekly, monthly and yearly.
- A start date is required for one-time and recurring schedules alike.
- The shortest interval you can schedule is 15 minutes.
- Without an end date the schedule continues indefinitely.
- The schedule sets when the script is **submitted** for processing, not when it actually starts
  executing.
- Repeats within a day stop at midnight; on the next scheduled day the sequence restarts at the start
  time. Some combinations:
  - daily at 3 a.m., every five hours: 3 a.m., 8 a.m., 1 p.m., 6 p.m. and 11 p.m., then over again
    the following day;
  - starting at 2 p.m. and repeating hourly: hourly runs from 2 p.m. until midnight each day;
  - hourly around the clock: a daily schedule starting at midnight, repeating every hour, with no end
    date.
- When a run is submitted from the UI (Save and Execute) or by the schedule, the script runs as a
  system user that has Administrator permissions.

### Time zones

- The start date and time are saved in the time zone of whoever saves the deployment, and each viewer
  sees them converted to their own time zone. For instance, a start of 8 a.m. on May 5 saved by a user
  in Manila appears as 8 p.m. on May 4 to a viewer on US Eastern time.
- If someone else saves the deployment again, the start values are stored anew in that person's time
  zone, which can shift the schedule.
- Daylight saving rules are applied according to the start date.

## When scripts start late

Possible reasons for a delayed start:

- the deployment still uses a legacy queue;
- it depends on a queue;
- it depends on an asynchronous search;
- database access problems;
- network problems;
- a system component being restarted.

Move jobs off legacy queues onto the dynamic queue. Signs that a legacy queue is still in use: a
number in the queue column, a **Remove Queue** button, or the deprecated Queue field on the
deployment. In the queue column, a number means a legacy queue; None or an empty cell means dynamic
processing.

To investigate, use the SuiteCloud Processors Monitor, the script queue monitor and the
[scheduled script status page](scheduled_script.status_page.md). If the APM SuiteApp is installed, the
processors monitor is under Customization > Performance > SuiteCloud Processors Monitor; sort its
overview by average wait time before you contact support. The source does not state where the script
queue monitor is found in the UI.

## Example

Submit a particular Not Scheduled deployment from server code with a one-off parameter value. The
script, deployment and parameter IDs are invented.

```js
/**
 * @NApiVersion 2.1
 */
define(['N/task'], (task) => {
  const queueStockSnapshot = (locationId) => {
    const snapshotTask = task.create({
      taskType: task.TaskType.SCHEDULED_SCRIPT,
      scriptId: 'customscript_stock_snapshot_ss',
      deploymentId: 'customdeploy_stock_snapshot_ondemand',
      params: { custscript_snapshot_location: locationId }
    });
    return snapshotTask.submit();
  };

  return { queueStockSnapshot };
});
```
