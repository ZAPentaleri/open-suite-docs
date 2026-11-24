# Script records
<!-- id: topic:script_record -->

See also: [Script deployment](script_deployment.md), [Script parameters](script_parameters.md),
[Entry point script validation](entry_point_validation.md), [Script system notes](script_deployment.system_notes.md).

Every script needs a script record before it can be deployed. The record ties a script file in the
File Cabinet to a script type and carries the settings shared by all of its deployments.

## Creating the record

- Go to Customization > Scripting > Scripts > New.
- The script file must already be in the File Cabinet, or you upload it from the same page: click
  the plus icon to the right of the Script File list to open an upload window. The list offers every
  script file version you have uploaded.
- The file must pass [entry point validation](entry_point_validation.md).
- A client-side script file cannot have **Hide in SuiteBundle** set; saving fails with a validation
  error.
- If you pick a SuiteScript 1.0 file, you are asked to choose a script type. To pick a different
  file instead, use the browser's Back button. Converting 1.0 scripts to 2.1 is recommended.

## Fields filled from the file

| Field | Taken from |
|---|---|
| API Version | the `@NApiVersion` tag |
| Script File | the file name |
| Type | the `@NScriptType` tag |
| Functions (entry points) | the entry point functions in the file |

These fields are read-only. You cannot edit the entry point list in the UI; when the record is saved
the list is synced from the file again, and the saved record shows a check next to each entry point
in use. A replacement file must keep the same script type; that includes upgrading a 1.0 script
record to 2.1 by swapping in a 2.1 file.

## Other settings

- **ID**: generated if you leave it empty.
- **Portlet Type**: shown and required only for portlet scripts.
- **Owner**: defaults to the current user. After saving, only the owner or an administrator can edit
  the record.
- **Inactive**: stops the script and all of its deployments from running.
- **Buttons** sublist (client scripts only): button labels can be translated when multiple languages
  are enabled.
- **Custom Plug-in Types** sublist: shown only for custom plug-in type scripts.
- **Parameters** subtab: defines [script parameters](script_parameters.md), which are custom fields
  that configure the script per deployment or per user. They are most useful when a script is
  deployed more than once.

## Error notifications

Who is emailed when the script throws errors:

- the script owner (the default),
- all administrators,
- groups chosen from the groups list,
- other addresses, entered as a comma-separated list.

Emails go out for the first error in an hour, as an hourly summary, and after 100 errors since the
last reset. For example, 130 errors in one hour produce the initial email, an email at the 100th
error, and a summary; 50 errors in the next hour produce a single summary.

To monitor execution and errors, look at the script's Execution Log subtab, the script deployment
page, and the SuiteScript Debugger.

## Saving

- **Save** stores the record without a deployment line; you can come back later and use **Save and
  Deploy**.
- **Save and Deploy** saves and opens a new deployment (see [Script deployment](script_deployment.md)).
