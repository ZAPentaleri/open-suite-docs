# The Scripted Records Page
<!-- id: topic:monitoring.scripted_records -->

Part of: [Script monitoring and logs](monitoring.md)

See also: [UserEventScript](../script-types/UserEventScript.md), [ClientScript](../script-types/ClientScript.md),
[Setting Runtime Options](runtime_options.md), [SuiteFlow](suiteflow.md).

The Scripted Records page, found under Customization > Scripting > Scripted Records, shows for each
record type which [user event](../script-types/UserEventScript.md) scripts,
[client](../script-types/ClientScript.md) scripts and workflows run on it. It is where you:

- decide the order in which scripts run,
- change a deployment's status, deploy a script or make it inactive,
- change a workflow's status, and
- set a workflow's initiation details.

## Execution order of user event scripts

Scripts are first grouped by entry point. Inside each entry point they run in the order shown in
the list on this page.

- Priority belongs to the script as a whole, not to each of its entry points.
- The afterSubmit run starts with the scripts that also have a beforeSubmit function.
- User event afterSubmit functions run before workflow actions that fire after submit.
- Scripts that are not localized run before localized ones.

Illustration: four user event scripts, A to D, appear in the list in that order. The resulting
runs are:

| Entry point | Runs |
|---|---|
| beforeLoad | A, then B |
| beforeSubmit | B, then D |
| afterSubmit | D, then A, then C |

## Using the page

By default the list includes only record types that have at least one script or workflow. You can
show undeployed records too (the list then contains every record) and filter the list by script.
**View** shows the scripts for one record type; **Edit** lets you reorder them and deploy or
undeploy scripts. Clicking a script or a workflow opens its record, and clicking a bundle ID or
bundle name opens the bundle details.

### Subtabs

- User event scripts
- Localized user event scripts
- Client scripts
- Localized client scripts
- Custom forms
- Workflows (shown only when SuiteFlow is enabled)

### User event script subtabs

Columns: script, owner, API version, bundle, status, countries, the entry point functions (before
load, before submit, after submit) and options. The countries column appears only on the localized
subtab.

Drag scripts to change the order in which they run. You can also change a deployment's status or its
deployed check box here. Aim for about ten scripts or fewer per entry point.

### Client script subtabs

Columns include the page init, save record, field changed and validate line functions. Drag scripts
to change their run order.

A record takes at most 10 client scripts, localized and non-localized counted together. To deploy
more, turn on the preference that removes the per-record client script deployment limit; be aware
that many scripts on one record slow it down.

### Custom forms subtab

Columns: form, bundle, script and entry point functions. Clicking a script name opens its deployment
record.

### Workflows subtab

This subtab needs the `SuiteFlow` feature. Columns: workflow, internal ID, description, from bundle,
bundle name, company name of the bundle, owner, status, trigger type, on create, and on view or
update. Clicking a workflow name or ID opens the workflow.

| Setting | Values |
|---|---|
| Status | `Suspended`, `Not Initiating`, `Testing`, `Released` |
| Trigger type | `All`, `Before Record Load`, `Before Record Submit`, `After Record Submit` |
| Event triggers | On create; on view or update |
