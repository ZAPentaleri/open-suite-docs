# SuiteScript 1.0 inline editing
<!-- id: topic:ss1_api.inline_editing -->

> **Legacy material.** This page exists so that 1.0 scripts can be read, converted and minimally
> repaired. Do not write new 1.0 code; the 2.1 counterpart of `nlapiSubmitField` is
> `record.submitFields` in [N/record](../modules/N_record.md).

Part of: [SuiteScript 1.0 API conventions](ss1_api.md)

## nlapiSubmitField is the scripted inline edit

[`nlapiSubmitField`](../ss1/fields-sublists.md) does in a script what inline editing does in the UI:
it changes one or more fields on a record without loading it.

- It already saves the record. Do not follow it with `nlapiSubmitRecord`.
- It is cheaper than the full cycle when the fields are inline-editable:

| Approach | Usage units |
|---|---|
| `nlapiSubmitField` on inline-editable fields | 10 |
| `nlapiLoadRecord`, set values, `nlapiSubmitRecord` | 30 |

- A field counts as inline-editable when no other field is slaved (sourced) from it.
- If even one field in the call is not inline-editable, NetSuite falls back to loading, setting and
  submitting the whole record for the whole set of fields. On a transaction that costs 30 units (10
  for the load plus 20 for the submit).
- A scheduled script started from an inline-edit user event that calls `nlapiSubmitField` does not
  get its change saved.

## The xedit event in user event scripts

When a user event script runs because of an inline edit or a mass update, its `type` argument is
`xedit`. The `edit` type does not cover these two cases, so a script that only checks for `edit`
misses them.

During `xedit`:

- The new record returned by `nlapiGetNewRecord` holds only the fields that were changed.
- Use [`nlobjRecord.getAllFields`](../ss1/objects-record-search.md) to get the list of fields that
  changed.
- Reading an unchanged field with `getFieldValue` returns `null`, which is easy to mistake for an
  empty value.
- Reading the old record with `nlapiGetOldRecord` is clearer for such comparisons, though slower.

## Example (1.0, for reading legacy code)

A `beforeSubmit` function that reacts to inline edits of a support case's priority only when that
field was actually part of the edit.

```js
function caseBeforeSubmit(type) {
  if (type !== 'xedit') {
    return;
  }
  var changed = nlapiGetNewRecord().getAllFields();
  if (changed.indexOf('priority') === -1) {
    return;
  }
  var before = nlapiGetOldRecord().getFieldValue('priority');
  var after = nlapiGetNewRecord().getFieldValue('priority');
  nlapiLogExecution('AUDIT', 'Case priority edited inline', before + ' -> ' + after);
}
```
