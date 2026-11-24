# SuiteScript 1.0 record initialization defaults
<!-- id: topic:ss1_api.initialization_defaults -->

> **Legacy material.** This page exists so that 1.0 scripts can be read, converted and minimally
> repaired. Do not write new 1.0 code; in 2.1 the equivalent is the `defaultValues` option of the
> [N/record](../modules/N_record.md) create, copy, load and transform functions.

Part of: [SuiteScript 1.0 API conventions](ss1_api.md)

## What they are

Initialization defaults are an object of name/value pairs passed as the last argument of the 1.0
record functions. They set up the record before your script sees it (for example the entity of a
new transaction, the custom form, or dynamic mode).

| Function | Argument that takes the defaults |
|---|---|
| [`nlapiCreateRecord`](../ss1/records.md) | `initializeValues` |
| [`nlapiCopyRecord`](../ss1/records.md) | `initializeValues` |
| [`nlapiLoadRecord`](../ss1/records.md) | `initializeValues` |
| [`nlapiDeleteRecord`](../ss1/records.md) | `initializeValues` |
| [`nlapiTransformRecord`](../ss1/records.md) | `transformValues` |

The full list of supported names, per record type, is in
[catalog:record_initialization_defaults](../gen/catalogs/record_initialization_defaults.md).

## Rules

- Quote the values, not the property names, unless the value is a variable, a number or a boolean.
- The deletion reason settings are only valid with `nlapiDeleteRecord`.

> [!SOURCE-ERROR] The source's table of defaults names the deletion reason property
> `deletionreason`, but a note next to it calls it `deletionreasoncode`. The generated catalog uses
> `deletionreason`; the spec does not say which name the API accepts, so test before relying on
> either.
<!-- source-error: topic:ss1_api.initialization_defaults | - | table_names_deletionreason_note_says_deletionreasoncode -->

- Country codes, used by the tax group nexus country default and the tax type country default, are
  listed in [catalog:countries](../gen/catalogs/countries.md).

## Example (1.0, for reading legacy code)

Copying a purchase order onto a different custom form, in dynamic mode, to place a reorder. The
property names are plain, the string value is quoted, and the form ID is passed as a variable.

```js
function copyPurchaseOrderForReorder(poId, formId) {
  var original = nlapiLoadRecord('purchaseorder', poId);
  var copy = nlapiCopyRecord('purchaseorder', poId, { customform: formId, recordmode: 'dynamic' });
  copy.setFieldValue('memo', 'Reorder of ' + original.getFieldValue('tranid'));
  return nlapiSubmitRecord(copy, true);
}
```
