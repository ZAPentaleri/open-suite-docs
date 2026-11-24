# N/ui/dialog
<!-- id: N/ui/dialog -->

`N/ui/dialog` shows modal dialogs in the browser: the page waits until the user presses one of the
dialog's buttons. There are three shapes: a plain notice with an OK button (`alert`), an OK/Cancel
question (`confirm`), and a dialog with buttons you define (`create`). Each returns a Promise that
resolves with the value of the button the user pressed.

Use these APIs rather than reaching into the NetSuite page through the DOM; scripts are not meant to
manipulate the NetSuite UI that way.

- **Availability:** client scripts only.
- **Load it as:** `define(['N/ui/dialog'], (dialog) => { ... })`
- **Since:** 2016.1
- Related: [N/ui/message](N_ui_message.md) for non-blocking banners.

## Members

| Member | Kind | Summary |
|---|---|---|
| [`dialog.alert(options)`](#dialogalertoptions) | function | Notice with an OK button |
| [`dialog.confirm(options)`](#dialogconfirmoptions) | function | Question with OK and Cancel |
| [`dialog.create(options)`](#dialogcreateoptions) | function | Dialog with custom buttons |

## Functions

### dialog.alert(options)
<!-- id: N/ui/dialog.alert -->

Shows a message with a single OK button. The returned Promise resolves with `true` when the user
presses OK. You only need to handle the Promise if something should happen after the user has
dismissed the dialog.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | No | |
| `options.title` | string | No | `""` |
| `options.message` | string | No | `""` |

- `options.title` and `options.message` default to an empty string. Since 2016.1.

**Returns:** `Promise`, resolving with the pressed button's value (`true` for OK).

**Governance:** 0 units.

**Availability:** client scripts.

**Since:** 2016.1

### dialog.confirm(options)
<!-- id: N/ui/dialog.confirm -->

Asks a yes/no question with OK and Cancel buttons. The Promise resolves with `true` if the user
chose OK and `false` if they chose Cancel.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | No | |
| `options.title` | string | No | `""` |
| `options.message` | string | No | `""` |

- `options.title` and `options.message` default to an empty string. Since 2016.1.

**Returns:** `Promise`, resolving with `true` (OK) or `false` (Cancel).

**Governance:** 0 units.

**Availability:** client scripts.

**Since:** 2016.1

### dialog.create(options)
<!-- id: N/ui/dialog.create -->

Shows a dialog with your own set of buttons. Each button has a label (what the user sees) and a
value (what the Promise resolves with when that button is pressed). If you give no buttons, the
dialog has a single OK button whose value is `true`.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | No | |
| `options.buttons` | string[] | No | |
| `options.title` | string | No | `""` |
| `options.message` | string | No | `""` |

- `options.buttons`: an array of button objects, each with a required `label` and a required
  `value`. Without it, one OK button with value `true` is shown. Since 2016.1.
- `options.title` and `options.message` default to an empty string. Since 2016.1.

> [!SOURCE-ERROR] The source types `options.buttons` as an array of strings, yet describes each entry
> as an object with `label` and `value`. Pass objects with both properties, as the description and
> the error rules require.
<!-- source-error: N/ui/dialog.create | options.buttons | typed_string_array_but_items_are_objects -->

**Returns:** `Promise`, resolving with the `value` of the button the user pressed.

**Errors:**
- `WRONG_PARAMETER_TYPE`: `options.buttons` is not an array.
- `BUTTONS_MUST_INCLUDE_BOTH_A_LABEL_AND_VALUE`: a button in `options.buttons` lacks a label or a
  value.

**Governance:** 0 units.

**Availability:** client scripts.

**Since:** 2016.1

## Examples

A client script on purchase orders that double-checks a change of vendor. If the user cancels, the
previous vendor is put back; the Promise is needed here because the follow-up depends on the answer.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType ClientScript
 */
define(['N/ui/dialog'], (dialog) => {
  let lastVendor = null;

  const pageInit = (context) => {
    lastVendor = context.currentRecord.getValue({ fieldId: 'entity' });
  };

  const fieldChanged = (context) => {
    if (context.fieldId !== 'entity') {
      return;
    }
    const po = context.currentRecord;
    const newVendor = po.getValue({ fieldId: 'entity' });
    if (!lastVendor || newVendor === lastVendor) {
      lastVendor = newVendor;
      return;
    }
    dialog.confirm({
      title: 'Change vendor?',
      message: 'Switching vendors can reset item prices on this order. Keep the new vendor?'
    }).then((keep) => {
      if (keep) {
        lastVendor = newVendor;
      } else {
        po.setValue({ fieldId: 'entity', value: lastVendor, ignoreFieldChange: true });
      }
    });
  };

  return { pageInit, fieldChanged };
});
```

A three-way choice with custom buttons, used from a button on a support case form:

```js
/**
 * @NApiVersion 2.1
 * @NScriptType ClientScript
 */
define(['N/ui/dialog', 'N/currentRecord'], (dialog, currentRecord) => {
  const pageInit = () => {};

  const chooseEscalation = () => {
    dialog.create({
      title: 'Escalate case',
      message: 'Where should this case go next?',
      buttons: [
        { label: 'Tier 2', value: 'tier2' },
        { label: 'Engineering', value: 'eng' },
        { label: 'Not now', value: '' }
      ]
    }).then((target) => {
      if (target) {
        currentRecord.get().setValue({ fieldId: 'custevent_escalation_target', value: target });
      }
    });
  };

  return { pageInit, chooseEscalation };
});
```
