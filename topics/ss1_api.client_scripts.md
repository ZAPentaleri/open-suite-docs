# SuiteScript 1.0 client scripts
<!-- id: topic:ss1_api.client_scripts -->

> **Legacy material.** This page exists so that 1.0 scripts can be read, converted and minimally
> repaired. Do not write new 1.0 code; the 2.1 counterpart is
> [ClientScript](../script-types/ClientScript.md).

Part of: [SuiteScript 1.0 API conventions](ss1_api.md)

A 1.0 client script runs in the user's browser while a record or Suitelet page is open.

## Form-level and record-level scripts

| | Form-level | Record-level |
|---|---|---|
| Attached to | One form | A record type, through a deployment set up like a server script's, with an audience |
| Runs on | Only the forms it is attached to | Built-in and custom forms of that record type, plus forms and lists built from UI objects in Suitelets |
| Usable in Suitelets | No | Yes |
| How many | The source does not state a limit | At most 10 record-level scripts per record type |

- Record-level scripts run regardless of whatever form-level scripts are on the form.
- On save, the order is: form-level client script, then record-level client script, then user event
  scripts.
- Since 2016.1, attaching a script to a form requires the SuiteScript permission at Edit level.

**Governance:** 1,000 usage units per script. Each script has its own allowance; scripts on the same
page do not share it.

## Event functions and their arguments

Function names are free; you bind each function to an event on the script record. The events, the
positional arguments NetSuite passes, and what the return value means:

| Event | Arguments | Return value |
|---|---|---|
| `pageInit` | `type` | None |
| `saveRecord` | none | Boolean: `false` stops the save |
| `validateField` | `type`, `name`, `linenum` | Boolean: `false` rejects the value |
| `fieldChanged` | `type`, `name`, `linenum` | None |
| `postSourcing` | `type`, `name` | None |
| `lineInit` | `type` | None |
| `validateLine` | `type` | Boolean: `false` rejects the line |
| `recalc` | `type` | None |
| `validateInsert` | `type` | Boolean: `false` rejects the insert |
| `validateDelete` | `type` | Boolean: `false` rejects the delete |

- For `pageInit`, `type` is the page mode: `create`, `copy` or `edit`.
- For `validateField` and `fieldChanged`, `type` is the sublist ID, and it is `null` when the field
  is a body field. `name` is the field ID.
- `linenum` is 1-based.
- `saveRecord` must always return `true` or `false`; falling off the end of the function is a bug.

## When each event fires

- **validateField** does not fire for select fields or checkboxes.
- **validateField** and **fieldChanged** do fire for fields that a user event script added in
  `beforeLoad`.
- **fieldChanged** does not fire for field values passed in the page URL; handle those in
  `pageInit`.
- **postSourcing** runs after the field's dependent (child) fields have finished sourcing.
- **lineInit** and **validateLine** apply to editor and inline editor sublists.
- **validateLine** reads the line through the current-line getters.
- **recalc**: do not use current-line calls in it (put that logic in `validateLine`), and do not call
  `nlapiCommitLineItem` in it, or the line is calculated twice. The Add Multiple button does not
  trigger it.
- **validateInsert** returning `false` blocks `nlapiInsertLineItem`.

## Remote object calls

Client calls that create, load, copy or transform a record (remote objects) cannot use dynamic
mode. See [SuiteScript 1.0 record modes](ss1_api.records.md).

## Converting

The 2.1 [ClientScript](../script-types/ClientScript.md) has its own entry points and context
objects; see [Converting SuiteScript 1.0 scripts](ss21_transition.from_ss1.md) and
[Converting SuiteScript 1.0 to 2.1](../conversion/ss1-to-ss21.md) for the mapping. Remember that
1.0 line numbers start at 1.

## Example (1.0, for reading legacy code)

A `validateLine` function on purchase order item lines that rejects a line whose expected receipt
date is missing when the quantity is large. It always returns a boolean.

```js
function checkPurchaseLine(type) {
  if (type !== 'item') {
    return true;
  }
  var qty = parseFloat(nlapiGetCurrentLineItemValue('item', 'quantity')) || 0;
  var eta = nlapiGetCurrentLineItemValue('item', 'expectedreceiptdate');
  if (qty >= 500 && !eta) {
    alert('Lines of 500 units or more need an expected receipt date.');
    return false;
  }
  return true;
}
```
