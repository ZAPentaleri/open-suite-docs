# SuiteScript 1.0 sublists
<!-- id: topic:ss1_api.sublists -->

> **Legacy material.** This page exists so that 1.0 scripts can be read, converted and minimally
> repaired. Do not write new 1.0 code; in 2.1 use the sublist methods of
> [N/record](../modules/N_record.md) and [N/currentRecord](../modules/N_currentRecord.md).

Part of: [SuiteScript 1.0 API conventions](ss1_api.md)

## Sublist types

1.0 knows four kinds of sublist: editor, inline editor, list and static list.

| Type | Get and set line values | Add or remove lines |
|---|---|---|
| Editor | Yes | Yes |
| Inline editor | Yes | Yes |
| List | Yes | No |
| Static list | No | No |

Getting and setting line values works on editor, inline editor and list sublists. Lines cannot be
added to or removed from a list sublist. Static list sublists on standard records cannot be
scripted at all.

## Line numbers

Line numbers in 1.0 start at **1**. Keep that in mind when a converted script moves to 2.x
sublist methods.

## Adding a line

| Where | Steps |
|---|---|
| Client script (current record) | `nlapiSelectNewLineItem`, then `nlapiSetCurrentLineItemValue` for each field, then `nlapiCommitLineItem` |
| Server script | Load the record with `nlapiLoadRecord`, call its `selectNewLineItem`, `setCurrentLineItemValue` and `commitLineItem` methods, then save it with `nlapiSubmitRecord` |

On the server, the record methods take the sublist ID as their first (`group`) argument.

> [!SOURCE-ERROR] The source's server-side steps say the `group` argument takes a line number,
> which appears to mean the `linenum` argument. `group` names the sublist; check the method
> signatures in the 1.0 reference before relying on either reading.
<!-- source-error: topic:ss1_api.sublists | - | server_steps_say_group_argument_takes_line_number -->

- In a client script, line changes only take effect once the line is committed.
- If setting a field makes other fields on the line source their values, let that sourcing finish
  before committing. The postSourcing client event is the only way a script can tell that
  sourcing has finished.

## Editing an existing line

Select the line with `nlapiSelectLineItem`, change it with `nlapiSetCurrentLineItemValue`, and
commit it with `nlapiCommitLineItem`.

## setLineItemValue is restricted

[`nlapiSetLineItemValue`](../ss1/fields-sublists.md) and
[`nlobjRecord.setLineItemValue`](../ss1/objects-record-search.md) cannot be used in dynamic mode
or in client scripts. There, always use the select, set current, commit sequence.

See the [field and sublist functions](../ss1/fields-sublists.md) for signatures.

## Adding a line whose item sources other values

When a client script adds a line and sets an item that sources further fields, the source
describes this sequence:

1. Call `nlapiSelectNewLineItem`.
2. Set a custom flag column on the line to true.
3. Set the item field.
4. In the postSourcing function, check the flag when the field that finished sourcing is the item
   field.
5. Reset the flag.
6. Set the remaining line values.
7. Call `nlapiCommitLineItem`.

The flag lets postSourcing tell a line the script is adding apart from a user's own edits.

## Examples (1.0, for reading legacy code)

A client function, run from a button, that raises the quantity on every purchase order line below
a minimum. It edits each existing line with select, set, commit; lines are numbered from 1.

```js
function enforceMinimumQuantity() {
  var MIN_QTY = 12;
  var count = nlapiGetLineItemCount('item');
  for (var line = 1; line <= count; line++) {
    nlapiSelectLineItem('item', line);
    var qty = parseFloat(nlapiGetCurrentLineItemValue('item', 'quantity')) || 0;
    if (qty < MIN_QTY) {
      nlapiSetCurrentLineItemValue('item', 'quantity', MIN_QTY, true, true);
      nlapiCommitLineItem('item');
    }
  }
}
```

The flag sequence above, split across a helper and the postSourcing function. The column
`custcol_added_by_script` is an invented custom checkbox column.

```js
function addSampleKitLine(itemId) {
  nlapiSelectNewLineItem('item');
  nlapiSetCurrentLineItemValue('item', 'custcol_added_by_script', 'T');
  nlapiSetCurrentLineItemValue('item', 'item', itemId, true, true);
}

function postSourcing(type, name) {
  if (type !== 'item' || name !== 'item') {
    return;
  }
  if (nlapiGetCurrentLineItemValue('item', 'custcol_added_by_script') !== 'T') {
    return;
  }
  nlapiSetCurrentLineItemValue('item', 'custcol_added_by_script', 'F');
  nlapiSetCurrentLineItemValue('item', 'quantity', 1);
  nlapiCommitLineItem('item');
}
```
