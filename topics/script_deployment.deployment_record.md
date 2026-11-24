# Deployment Record
<!-- id: topic:script_deployment.deployment_record -->

Part of: [Script Deployment](script_deployment.md)

See also: [scriptdeployment record](../gen/records/scriptdeployment.md),
[Deployments Sublist](script_deployment.deployments_sublist.md), [Script records](script_record.md).

The script deployment record ([scriptdeployment](../gen/records/scriptdeployment.md)) exposes more
settings than the Deployments sublist on the script record. You create one with **Save and Deploy**
on the script record. To change an existing deployment, go to Customization > Scripting > Script
Deployments and click Edit. Deployments can be edited in three ways: on the Deployments sublist, on the
deployment record form, or in code with the [N/record](../modules/N_record.md) module.

The sublist covers Applies To, the custom ID, Deployed, Status, Event Type and Log Level; the record
adds further fields such as Execute As Role.

## Key fields

- **Title** is required for [Suitelet](../script-types/Suitelet.md),
  [scheduled](../script-types/ScheduledScript.md) and [portlet](../script-types/Portlet.md)
  deployments.
- **Applies To** is required for [user event](../script-types/UserEventScript.md) and
  [client](../script-types/ClientScript.md) deployments. Its **All Records** choice covers the records
  that officially support SuiteScript.
- **ID** begins with `customdeploy`; leave it empty and an ID is generated.
- **Deployed** is checked by default, and the script runs only while it stays checked.
- **Available Without Login** on a Suitelet produces an external URL when you save. External URLs use
  account-specific domains, so do not hard-code them; resolve the Suitelet's URL at run time with
  [`url.resolveScript`](../modules/N_url.md).
- A [portlet](../script-types/Portlet.md) deployment also has to be enabled on the dashboard before
  it shows.

## Status values

On the Deployments sublist, Status offers **Testing** and **Released**. The full list of status values
a deployment can hold is the SDF value list
[catalog:sdf_list_script_status](../gen/catalogs/sdf_list_script_status.md): `COMPLETED`,
`INPROGRESS`, `INQUEUE`, `NOTSCHEDULED`, `RELEASED`, `SCHEDULED` and `TESTING`. The catalog itself
does not say what each value means or which script types accept it; for scheduled scripts see
[Scheduled script deployments](scheduled_script.deployment.md), and for Testing and Released see
[Setting Runtime Options](runtime_options.md#deployment-status).

## Subtabs

| Subtab | Purpose | Shown for |
|---|---|---|
| Audience | The roles, groups or individual users the script affects; a deployed script runs only for its audience. | [ClientScript](../script-types/ClientScript.md), [Portlet](../script-types/Portlet.md), [Restlet](../script-types/Restlet.md), [Suitelet](../script-types/Suitelet.md), [UserEventScript](../script-types/UserEventScript.md) |
| Scripts | Summarizes the client or user event scripts involved. | [ClientScript](../script-types/ClientScript.md), [UserEventScript](../script-types/UserEventScript.md) |
| Links | Adds a menu link that opens the Suitelet. | [Suitelet](../script-types/Suitelet.md) |
| Schedule | When and how often the script runs. Times are in the time zone set in the preferences of the user who last edited the deployment. | [MapReduceScript](../script-types/MapReduceScript.md), [ScheduledScript](../script-types/ScheduledScript.md) |
| Context Filtering | Execution and localization contexts. | Not stated |
| Execution Log | Custom views of the script's log entries. | Not stated |
| System Notes | The contexts applied on each execution (see [Script system notes](script_deployment.system_notes.md)). | Not stated |

## Editing while the script runs

A deployment cannot be edited while its script is running. Wait until the script has stopped, then
make the change.
