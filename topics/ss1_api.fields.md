# SuiteScript 1.0 fields
<!-- id: topic:ss1_api.fields -->

> **Legacy material.** This page exists so that 1.0 scripts can be read, converted and minimally
> repaired. Do not write new 1.0 code; see [N/record](../modules/N_record.md) for the 2.1 field API.

Part of: [SuiteScript 1.0 API conventions](ss1_api.md)

Related functions: [`nlapiGetFieldValue`](../ss1/fields-sublists.md),
[`nlapiGetLineItemValue`](../ss1/fields-sublists.md),
[`nlobjRecord.getFieldValue`](../ss1/objects-record-search.md),
[`nlobjRecord.getLineItemValue`](../ss1/objects-record-search.md).

## Referencing fields and values

- Refer to records and list values by internal ID, not by display name.
- Custom field IDs start with a prefix that tells you what they belong to:

| Custom field kind | ID prefix |
|---|---|
| Entity field | `custentity` |
| Item field | `custitem` |
| CRM field | `custevent` |
| Transaction body field | `custbody` |
| Transaction column field | `custcol` |
| Item option | `custcol` |
| Item number field | `custitemnumber` |
| Other record field | `custrecord` |

- A custom field ID must not contain the suffix `send`.

## Empty values: null versus empty string

In standard mode, `nlapiGetFieldValue`, `nlapiGetLineItemValue`, `nlobjRecord.getFieldValue` and
`nlobjRecord.getLineItemValue` can report an empty field either as `null` or as an empty string,
so test for both. In dynamic mode these getters are consistent.

## Custom fields that are not stored

- A custom field whose value is not stored cannot be set by a script.
- Its value is computed when read and can change over time, so do not treat a value you read once
  as permanent.

## The currency name field

`currencyname` is read-only and cannot be submitted. Reading it with `nlapiGetFieldValue` or
`nlapiLookupField` returns an empty value or raises an error; it is only readable from a
transaction you have loaded. Read the `currency` field instead.

## Example (1.0, for reading legacy code)

A helper that treats both empty forms as "no value", which is what standard-mode getters require.

```js
function hasValue(fieldId) {
  var v = nlapiGetFieldValue(fieldId);
  return v !== null && v !== '';
}

function requireTaskDueDate() {
  if (!hasValue('duedate')) {
    alert('Set a due date before saving this task.');
    return false;
  }
  return true;
}
```
