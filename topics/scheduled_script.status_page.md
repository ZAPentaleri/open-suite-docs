# Scheduled script status page
<!-- id: topic:scheduled_script.status_page -->

See also: [ScheduledScript](../script-types/ScheduledScript.md),
[Scheduled script deployments](scheduled_script.deployment.md), [Map/Reduce status page](map_reduce.status_page.md),
[scheduledscriptinstance record](../gen/records/scheduledscriptinstance.md).

The Scheduled Script Status page lists the instances of [scheduled scripts](../script-types/ScheduledScript.md)
and how far each has got. Open it from Customization > Scripting > Scheduled Script Status, or from
the status page link on a deployment record. Entries are kept for 30 days.

The page reports the progress of each run. That is not the same as the Status field on the
deployment, which says what kind of deployment it is (testing, scheduled, not scheduled).

## Filters

You can narrow the list by date, a from/to range, script, deployment ID, queue, status and API
version.

## Columns

| Column | Notes |
|---|---|
| API version | Shows the most recently submitted version, for every listing. |
| Deployment ID | |
| Date created | |
| Status | See the status table below. |
| Start / End | |
| Priority | |
| Queue | Only relevant to deployments created before SuiteCloud Processors. |
| % Complete | 0 while pending or processing, 100 once completed. A script cannot report its own percentage. |

The order of the columns depends on how you opened the page.

## Status values

| Status | Meaning |
|---|---|
| Pending | Waiting to be processed. |
| Processing | Running now. |
| Complete | Finished normally. |
| Failed | Was processed but did not complete. |
| Canceled | A server error canceled the script. |
| Deferred | Eligible to run but waiting on processing constraints. |
| Retry | Failed while processing and is being retried automatically. Check the script's execution log to find out why. |

## Reading instances from a script

The data behind this page is exposed as the
[scheduled script instance record](../gen/records/scheduledscriptinstance.md), which you can search
with [N/search](../modules/N_search.md).

## Example

Tally the retained instances of one script deployment by status, so an alert script can spot
failures and retries. The deployment internal ID is a script parameter you would define (see
[script parameters](script_parameters.md)); the status values are compared as text, which you
should confirm against your account's results.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType ScheduledScript
 */
define(['N/search', 'N/runtime', 'N/log'], (search, runtime, log) => {
  const execute = () => {
    const deploymentId = runtime.getCurrentScript().getParameter({ name: 'custscript_watch_deployment' });
    if (!deploymentId) {
      log.audit({ title: 'Nothing to watch', details: 'Parameter custscript_watch_deployment is empty.' });
      return;
    }
    const tally = {};
    search.create({
      type: search.Type.SCHEDULED_SCRIPT_INSTANCE,
      filters: [['scriptdeployment.internalid', search.Operator.ANYOF, [deploymentId]]],
      columns: ['status', 'percentcomplete', 'datecreated']
    }).run().each((row) => {
      const status = row.getValue({ name: 'status' });
      tally[status] = (tally[status] || 0) + 1;
      return true;
    });
    log.audit({ title: 'Instance status counts', details: JSON.stringify(tally) });
  };

  return { execute };
});
```
