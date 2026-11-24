# N/ui Modules
<!-- id: topic:ui_modules -->

`N/ui` is a family of three modules for working with the NetSuite user interface:

| Module | Use it for |
|---|---|
| [N/ui/dialog](../modules/N_ui_dialog.md) | Modal dialogs, whose outcome is handled through promises |
| [N/ui/message](../modules/N_ui_message.md) | Messages shown at the top of the page |
| [N/ui/serverWidget](../modules/N_ui_serverWidget.md) | Custom pages, forms and lists that look like standard NetSuite pages |

## Loading the whole family

Instead of naming each module, a script can list `N/ui` itself as a `define` dependency. What that
loads depends on where the script runs:

- in a **server** script, all three modules;
- in a **client** script, only `N/ui/dialog` and `N/ui/message`.

The source does not state what the `N/ui` dependency returns or how the three modules are reached
on it. The example below therefore names the individual modules.

## Example

A client script on purchase orders that asks the user, when the form opens, whether to show a
reminder about the receiving cut-off, and shows a page message if they agree.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType ClientScript
 */
define(['N/ui/dialog', 'N/ui/message'], (dialog, message) => {
  const pageInit = () => {
    dialog.confirm({
      title: 'Receiving cut-off',
      message: 'Show today\'s receiving cut-off reminder?'
    }).then((wantsReminder) => {
      if (wantsReminder) {
        message.create({
          type: message.Type.INFORMATION,
          title: 'Reminder',
          message: 'Goods received after 4 pm are booked on the next business day.'
        }).show();
      }
    }).catch(() => {});
  };

  return { pageInit };
});
```
