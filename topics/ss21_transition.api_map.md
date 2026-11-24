# SuiteScript 1.0 to 2.1 API map
<!-- id: topic:ss21_transition.api_map -->

Part of: [Transitioning to SuiteScript 2.1](ss21_transition.md)

The full symbol-by-symbol table (every `nlapi*` function and `nlobj*` object with its 2.x target,
relation and argument mapping) is in [Converting SuiteScript 1.0 to 2.1](../conversion/ss1-to-ss21.md).
This page gathers the rules and caveats that sit around that table.

## Scope of the map

- It covers the `nlapi*` functions and the `nlobj*` objects.
- It leaves out 1.0 APIs that were already deprecated, and 2.1 features that have no 1.0
  predecessor.
- The mapping is often not one-to-one: one 1.0 call may become several 2.x calls, a property, or
  nothing at all.
- 1.0 APIs cannot be called from a 2.1 script; mixing the two is not supported.

## Line indexes shift by one

2.1 numbers sublist lines from 0; 1.0 numbers them from 1. Adjust every line argument when
converting these functions: `nlapiGetLineItemField`, `nlapiGetLineItemMatrixField`,
`nlapiGetLineItemMatrixValue`, `nlapiGetLineItemText`, `nlapiGetLineItemValue`,
`nlapiGetLineItemValues`, `nlapiSelectLineItem`, `nlapiSetLineItemValue` and
`nlapiViewLineItemSubrecord`.

## Functions without a direct target

| 1.0 function | What to do in 2.x |
|---|---|
| `nlapiAddDays` | Plain JavaScript dates: `setDate(getDate() + n)` (or minus) |
| `nlapiAddMonths` | Plain JavaScript dates: `setMonth(getMonth() + n)` (or minus) |
| `nlapiGetNewRecord` | `context.newRecord` in user event entry points |
| `nlapiGetOldRecord` | `context.oldRecord` in the submit entry points |
| `nlapiGetRecordType` | The `type` of the current record ([N/currentRecord](../modules/N_currentRecord.md)) or of a [N/record](../modules/N_record.md) record |
| `nlapiSelectValue` | [`XPath.select`](../modules/N_xml.md), then read the node's `textContent` |
| `nlapiSelectValues` | `XPath.select`, then loop over the nodes reading `textContent` |
| `nlapiEncrypt` | [N/crypto](../modules/N_crypto.md) together with [N/encode](../modules/N_encode.md) |
| Date-time value functions | [N/format](../modules/N_format.md) |
| `nlapiYieldScript`, `nlapiSetRecoveryPoint` | Nothing to call: a [map/reduce script](../script-types/MapReduceScript.md) yields automatically |

## Getters that became properties, and other surprises

- `nlapiDisableField` maps to the field's `isDisabled` **property**.
- `nlapiGetLocation` maps to the user's `location` **property** ([N/runtime](../modules/N_runtime.md)).
- `nlapiIsLineItemChanged` maps to the sublist's `isChanged` **property**.
- `nlapiFormatCurrency`: currency formatting lives in [N/format](../modules/N_format.md), not in
  [N/currency](../modules/N_currency.md).
- `nlapiGetLineItemValues` returns an array, for multi-select fields.
- `nlapiRequestURLWithCredentials`: its 2.x replacement works in server scripts only.
- Rendering a PDF: [`TemplateRenderer.renderAsPdf`](../modules/N_render.md) produces what 1.0 got
  by passing an email merger's output string to `nlapiXMLToPDF`.

## Objects

- **`nlobjContext.getSetting`**: the first argument decides the target.

| `getSetting` type | 2.x target |
|---|---|
| `SCRIPT` | [`Script.getParameter`](../modules/N_runtime.md) |
| `SESSION` | [`Session.get`](../modules/N_runtime.md) |
| `FEATURE` | [`runtime.isFeatureInEffect`](../modules/N_runtime.md) |
| `PERMISSION` | [`User.getPermission`](../modules/N_runtime.md) |

- **`nlobjConfiguration`**: use [`config.load`](../modules/N_config.md), which returns a record.
- **`nlobjField`**: in Suitelets the target is [`serverWidget.Field`](../modules/N_ui_serverWidget.md);
  for reading field metadata on a record it is [`record.Field`](../modules/N_record.md).
- **Server responses**: [`https.ServerResponse`](../modules/N_https.md) text is UTF-8 by default.
- **`nlobjEmailMerger`**: its setters turn into parameters of
  [`render.mergeEmail`](../modules/N_render.md).
- **`getHeaders`**: in 2.x, a header that has several values comes back as an array.

### Search IDs

`nlobjSearch.getId` maps to the search's `searchId` property in
[N/search](../modules/N_search.md); `getScriptId` maps to its `id` property.

> [!SOURCE-ERROR] The source's note for `nlobjSearch.getId` names the `id` property, while the
> mapping itself points to `searchId`. Since `id` is the 2.x counterpart of the script ID, rely on
> the mapping (`searchId`) and verify against a saved search.
<!-- source-error: topic:ss21_transition.api_map | - | nlobjsearch_getid_note_names_id_for_searchid -->

### Subrecord cancel and commit

`nlobjSubrecord.cancel` and `nlobjSubrecord.commit` both map only approximately to the 2.x
[`record.Record`](../modules/N_record.md) object; neither has a same-named 2.x method.

> [!SOURCE-ERROR] The source's notes for `cancel` and `commit` describe the conversion differently
> although both map to the same 2.x target. The spec does not say which note is right; check the
> subrecord methods of [N/record](../modules/N_record.md) when converting either call.
<!-- source-error: topic:ss21_transition.api_map | - | nlobjsubrecord_cancel_and_commit_notes_differ -->

## Example: before and after

A 1.0 helper that totals a purchase order's line amounts, reading a script parameter for the line
limit:

```js
function sumLineAmounts(poId) {
  var limit = parseInt(nlapiGetContext().getSetting('SCRIPT', 'custscript_max_lines'), 10) || 100;
  var po = nlapiLoadRecord('purchaseorder', poId);
  var total = 0;
  for (var i = 1; i <= po.getLineItemCount('item') && i <= limit; i++) {
    total += parseFloat(po.getLineItemValue('item', 'amount', i)) || 0;
  }
  return total;
}
```

The 2.1 version: the setting becomes `Script.getParameter`, the loop starts at 0, and every call
takes an options object.

```js
/**
 * @NApiVersion 2.1
 */
define(['N/record', 'N/runtime'], (record, runtime) => {
  const sumLineAmounts = (poId) => {
    const limit = Number(runtime.getCurrentScript().getParameter({ name: 'custscript_max_lines' })) || 100;
    const po = record.load({ type: record.Type.PURCHASE_ORDER, id: poId });
    const count = Math.min(po.getLineCount({ sublistId: 'item' }), limit);
    let total = 0;
    for (let line = 0; line < count; line++) {
      total += Number(po.getSublistValue({ sublistId: 'item', fieldId: 'amount', line })) || 0;
    }
    return total;
  };

  return { sumLineAmounts };
});
```
