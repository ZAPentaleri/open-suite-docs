# Deployments Sublist
<!-- id: topic:script_deployment.deployments_sublist -->

Part of: [Script Deployment](script_deployment.md)

See also: [Deployment Record](script_deployment.deployment_record.md),
[scriptdeployment record](../gen/records/scriptdeployment.md), [Script records](script_record.md).

The quickest way to deploy a script is the Deployments subtab of the script record: open the script
record, click Edit and go to the Deployments subtab. How deployment works there depends on the script
type.

## Columns

Depending on the script type, the sublist shows some of these columns: Title, Applies To, ID,
Deployed, Status, Event Type, Log Level and Execute As Role.

- **Applies To**: add several lines to deploy the same script on several record types.
- **ID**: `customdeploy` is put in front of whatever you enter. If you leave the ID empty, one is
  generated.
- **Deployed**: defaults to Yes.
- **Status**: which values are offered depends on the script type.
- **Event Type**: a deployment can carry only one event type. When you need the script on several
  event types, create one deployment per event type.
- **Log Level**: decides which messages show up on the Execution Log subtab.
- **Execute As Role**: lets the script run with Administrator privileges.

For [map/reduce](../script-types/MapReduceScript.md) and [scheduled](../script-types/ScheduledScript.md)
scripts, the source gives the Deployed default as Not Scheduled.

> [!SOURCE-ERROR] The source states the default of the Deployed checkbox as "Not Scheduled", which is a
> status value, not a checkbox setting; elsewhere Deployed defaults to Yes. Most likely Not Scheduled is
> the default status for these two script types, but verify on a new deployment in your account.
<!-- source-error: topic:script_deployment.deployments_sublist | - | deployed_checkbox_default_given_as_not_scheduled -->

> [!SOURCE-ERROR] The source lists Execute As Role as a column of this sublist, while its description
> of the deployment record treats Execute As Role as a field found only on the record. Check whether the
> column appears for your script type; if not, set the role on the
> [deployment record](script_deployment.deployment_record.md).
<!-- source-error: topic:script_deployment.deployments_sublist | - | execute_as_role_sublist_column_vs_record_only_field -->

## Adding lines

Click **Add** to commit each deployment line. After the script record is saved, every line becomes a
link to its own deployment record, where you can fill in the fields the sublist does not offer.

## Limits

- Context filtering cannot be set from the sublist; set it on the
  [scriptdeployment](../gen/records/scriptdeployment.md) record instead.
- For some script types the sublist is read-only while you edit the script record.
