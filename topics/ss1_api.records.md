# SuiteScript 1.0 record modes
<!-- id: topic:ss1_api.records -->

> **Legacy material.** This page exists so that 1.0 scripts can be read, converted and minimally
> repaired. Do not write new 1.0 code; for 2.1 see [N/record](../modules/N_record.md) and the
> [Record Modes](record_modes.md) topic.

Part of: [SuiteScript 1.0 API conventions](ss1_api.md)

A 1.0 record object works in one of two modes. The mode decides when NetSuite runs sourcing,
validation and other business logic.

## Standard and dynamic mode compared

| | Standard mode | Dynamic mode |
|---|---|---|
| When business logic runs | Once, when the record is submitted | Each time a value is set, as in the UI |
| Order of setting fields | Any order | The order a user would follow on the form |
| Calculated fields | Not refreshed until submit; reload the record to read them | Refreshed as you go |
| Sublist writes | `setLineItemValue` allowed | `setLineItemValue` not allowed; select a line, set current values, commit |
| How you get it | Default for the record functions | Pass `recordmode: 'dynamic'` in the [initialization defaults](ss1_api.initialization_defaults.md) |

## Fixed cases

- The records returned by `nlapiGetNewRecord` and `nlapiGetOldRecord` in user event scripts are
  always in standard mode.
- In a client script, the current record (the one on the page) always behaves as dynamic mode.
- A client script that creates, loads, copies or transforms another record (a remote object) and
  asks for dynamic mode gets an error. Remote objects are standard mode only.

See [`nlapiLoadRecord`](../ss1/records.md), [`nlapiCreateRecord`](../ss1/records.md) and the
[`nlobjRecord`](../ss1/objects-record-search.md) methods.

## Example (1.0, for reading legacy code)

Adding a line to an opportunity in dynamic mode on the server. Because sourcing runs as each value
is set, the item goes in before the quantity and rate.

```js
function addTrainingLine(opportunityId, itemId) {
  var opp = nlapiLoadRecord('opportunity', opportunityId, { recordmode: 'dynamic' });
  opp.selectNewLineItem('item');
  opp.setCurrentLineItemValue('item', 'item', itemId);
  opp.setCurrentLineItemValue('item', 'quantity', 2);
  opp.setCurrentLineItemValue('item', 'rate', 450);
  opp.commitLineItem('item');
  return nlapiSubmitRecord(opp);
}
```
