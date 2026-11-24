# Map/Reduce Deployment Record
<!-- id: topic:map_reduce.deployment -->

Part of: [Map/reduce processing model](map_reduce.md)

See also: [scriptdeployment record](../gen/records/scriptdeployment.md),
[MapReduceScript](../script-types/MapReduceScript.md), [Script Deployment](script_deployment.md),
[Map/Reduce Submission](map_reduce.submission.md).

A [map/reduce script](../script-types/MapReduceScript.md) cannot run until a deployment record
([scriptdeployment](../gen/records/scriptdeployment.md)) exists for it. On top of the fields every
deployment has, a map/reduce deployment carries five of its own: **Priority**, **Concurrency
Limit**, **Submit All Stages at Once**, **Yield After Minutes** and **Buffer Size**.

## Identity fields

- **Script** points to the script record. Once set, it cannot be changed.
- **ID** has to be lowercase, without spaces, with underscores separating words, and the system puts
  `customdeploy` in front of it. Set your own ID when you intend to bundle the deployment. The Change
  ID button exists for renaming afterwards, but it is not the preferred route.

## Deployed and Status

- The script runs only while **Deployed** is checked, whatever the status says.
- With Deployed cleared and a status other than Scheduled, you can neither use Save and Execute nor
  submit the deployment. With Deployed cleared and status Scheduled, the schedule is simply ignored.
- **Status** takes one of three values: Scheduled, Not Scheduled (the default) or Testing.

**Not Scheduled** is for on-demand runs: submit from the UI with Save and Execute, or from code with
[`task.MapReduceScriptTask`](../modules/N_task.md#mapreducescripttask-object). A submission is
accepted only when no instance of that deployment is still running.

> [!SOURCE-ERROR] The source's description of on-demand submission from code names the scheduled
> script task object rather than the map/reduce one. Use `MapReduceScriptTask` for a map/reduce
> deployment; `ScheduledScriptTask` is the object for scheduled scripts.
<!-- source-error: topic:map_reduce.deployment | - | on_demand_submit_names_ScheduledScriptTask -->

- **Scheduled** means the deployment runs only from its schedule and cannot be run on demand. The
  status remains Scheduled even after a one-time schedule, or one with an end date, has finished.
- **Testing** lets only the script owner test and debug the deployment, without submitting it.
- After you submit or schedule a run, the actual execution may start with a delay.
- **See Instances** opens the [status page](map_reduce.status_page.md), filtered to this deployment
  and to today.

## Log level and role

- **Log Level** is Debug (the default), Audit, Error or Emergency, ordered from the most messages to
  the fewest. Debug suits testing; use Audit, Error or Emergency in production.
- **Execute As Role** is always Administrator and cannot be changed.

## Priority

High, Standard (the default) or Low. The value applies to every job the deployment produces, and the
same setting exists for both map/reduce and scheduled script deployments. See
[SuiteCloud Processors](suitecloud_processors.md) for how priority orders jobs.

## Concurrency Limit

- It applies to the map and reduce stages: a value of N allows up to N map jobs and up to N reduce
  jobs at once.
- The default is 2. When the field is left empty, the deployment may use the account's maximum
  number of processors.
- It does not allow a deployment to have more than one unfinished submission.
- Deploying through SDF to an account with a lower processor limit lowers the value to that
  account's limit, while the XML definition keeps the original number.
- The field exists since 2017.2. Deployments older than that got their value from the number of
  queues they had.

## Submit All Stages at Once

Each instance needs four or five stage jobs: get input data, shuffle and summarize, plus map, reduce
or both.

- **Checked** (the default): the jobs for all stages are submitted together.
- **Cleared**: each stage's job is submitted only after the stage it depends on is done.
- Checking it does not guarantee there are no gaps between stages in which other jobs run. If scripts
  must run in a strict order, have summarize schedule the next script with
  [`task.create`](../modules/N_task.md#taskcreateoptions).
- Clear the box only for low-priority deployments.

## Yield After Minutes

- Accepts 3 to 60 minutes; the default is 60.
- Once the time is exceeded the job yields, but only when the current function call ends, so the real
  yield can come later. With a 3-minute setting and a map function that takes 15 minutes, the yield
  happens at minute 15.
- Separately, a job yields after it has consumed 10,000 usage units; this is checked after each
  call. See [Map/reduce yielding](map_reduce.yielding.md).

## Buffer Size

- Allowed values: 1, 2, 4, 8, 16, 32 and 64; the default is 1.
- It is the number of key/value pairs a job flags before its progress is saved.
- A larger buffer means more pairs may be processed again after a server restart (see
  [Map/reduce interruption handling](map_reduce.interruptions.md)). Keep 1, especially when the
  script works with records; a higher value fits fast, purely algorithmic work.

## Schedule subtab

- The schedule is honored only when Status is Scheduled **and** Deployed is checked.
- Options: Single Event, Daily Event, Weekly Event, Monthly Event and Yearly Event, with fields for
  start date, start time, repeat, end by and no end date.
- A start date is required as soon as any schedule is set.
- A repeat interval begins again on each scheduled day.
- The start time is stored in the time zone of the user who saves the record and displayed in each
  viewer's preferred time zone. Daylight saving rules follow the start date.
