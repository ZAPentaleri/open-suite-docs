# Form-level deployment
<!-- id: topic:script_deployment.form_level -->

Part of: [Script deployment](script_deployment.md)

Sub-topics:

- [Custom actions](script_deployment.custom_actions.md)

See also: [ClientScript](../script-types/ClientScript.md), [Deployment levels](script_deployment.levels.md),
[Entry point script validation](entry_point_validation.md).

Form-level deployment attaches a [client script](../script-types/ClientScript.md) directly to one
custom form. It is available for client scripts only. The script runs only when that form is in use,
not on the record type's other forms.

## Which forms

- Custom entry forms
- Custom transaction forms
- Custom address forms

On custom entry and transaction forms you can also define a custom button or menu item backed by the
script (see [Custom actions](script_deployment.custom_actions.md)). Custom address forms do not
support custom actions.

## Attaching the script

1. Open the form with its **Edit** link and go to the **Custom Code** subtab.
2. Pick the file in the **Script File** field. To upload a new file from this page, click the plus
   icon to the right of the Script File list; an upload window opens.
3. The script must be valid (see [entry point validation](entry_point_validation.md)).

You need the SuiteScript permission at Edit level to work on the Custom Code subtab; with View level
you can only look at it.

## Entry point checkboxes

Once a file is chosen, the API version and the entry point checkboxes are filled in from the file:
the list shows a check for each entry point function found in it. You cannot tick or clear these
boxes yourself. To add or remove an entry point, change the functions in the script file.
