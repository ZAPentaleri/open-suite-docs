# Custom actions
<!-- id: topic:script_deployment.custom_actions -->

Part of: [Form-level deployment](script_deployment.form_level.md)

See also: [ClientScript](../script-types/ClientScript.md), [Script deployment](script_deployment.md).

A custom action adds your own button or menu item to a custom form. What it does comes from the
[client script](../script-types/ClientScript.md) attached to that form.

## Where you can use them

- Custom entry forms and custom transaction forms.
- Not on custom address forms.

## Setting one up

Open the form for editing (for example Customization > Forms > Transaction Forms, then Edit) and go to
the **Custom Actions** list on the **Actions** subtab. Each line has three columns:

| Column | What to enter |
|---|---|
| Label | The text the user sees. |
| Function | The name of the entry point function in the attached client script that should run. |
| Display As | **Button** or **Menu**. |

- **Button** puts the action at the top of the page.
- **Menu** puts it in the Actions menu.
