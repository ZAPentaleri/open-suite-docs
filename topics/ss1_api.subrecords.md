# SuiteScript 1.0 subrecords
<!-- id: topic:ss1_api.subrecords -->

> **Legacy material.** This page exists so that 1.0 scripts can be read, converted and minimally
> repaired. Do not write new 1.0 code; in 2.1 subrecords are handled by the subrecord methods of
> [N/record](../modules/N_record.md).

Part of: [SuiteScript 1.0 API conventions](ss1_api.md)

A subrecord is a record that only exists inside a parent record, attached either to a body field
or to a field on a sublist line. The two subrecords 1.0 scripts meet most are addresses and
inventory detail.

## Where scripts can work with subrecords

- Server scripts reach a subrecord only through its parent; there is no way to load one on its own.
- Before touching a subrecord, create or load the parent, in standard or dynamic mode.
- A subrecord can hang off a body field or a sublist field.
- Subrecords cannot be created or edited in client scripts. A client script on the parent can only
  view or delete them.
- Subrecords cannot be created or edited in a user event `beforeLoad`; the source points to
  `pageInit` instead.

> [!SOURCE-ERROR] Recommending `pageInit` (a client event) for creating or editing subrecords
> contradicts the source's own rule that client scripts cannot create or edit subrecords. Do not
> rely on `pageInit` for this; do subrecord writes in server-side code.
<!-- source-error: topic:ss1_api.subrecords | - | pageinit_advised_for_subrecord_edit_but_client_cannot_edit -->

- Like everything else on the page, subrecord UI must not be reached through the DOM.

## Body-field subrecords

| Action | Current record (function) | Loaded record (method) |
|---|---|---|
| Create | [`nlapiCreateSubrecord`](../ss1/records.md) | [`createSubrecord`](../ss1/objects-record-search.md) |
| Edit | [`nlapiEditSubrecord`](../ss1/records.md) | [`editSubrecord`](../ss1/objects-record-search.md) |
| Remove | [`nlapiRemoveSubrecord`](../ss1/records.md) | [`removeSubrecord`](../ss1/objects-record-search.md) |
| View | [`nlapiViewSubrecord`](../ss1/records.md) | [`viewSubrecord`](../ss1/objects-record-search.md) |

A typical body subrecord field is `shippingaddress` on a transaction. Save the subrecord, then
save the parent.

## Sublist-field subrecords

| Action | Current record (function) | Loaded record (method) |
|---|---|---|
| Create on the current line | [`nlapiCreateCurrentLineItemSubrecord`](../ss1/records.md) | `createCurrentLineItemSubrecord` |
| Edit on the current line | [`nlapiEditCurrentLineItemSubrecord`](../ss1/records.md) | `editCurrentLineItemSubrecord` |
| Remove from the current line | [`nlapiRemoveCurrentLineItemSubrecord`](../ss1/records.md) | `removeCurrentLineItemSubrecord` |
| View on the current line | [`nlapiViewCurrentLineItemSubrecord`](../ss1/records.md) | `viewCurrentLineItemSubrecord` |
| View on any line | [`nlapiViewLineItemSubrecord`](../ss1/records.md) | `viewLineItemSubrecord` |

A typical sublist subrecord field is `inventorydetail` on the `item` sublist.

## Sublists inside a subrecord

- Work on a subrecord's own sublist with `selectNewLineItem`, `selectLineItem`,
  `setCurrentLineItemValue` and `commitLineItem`. `setLineItemValue` does not work there.
- Save in this order: commit the subrecord's sublist line, call
  [`nlobjSubrecord.commit`](../ss1/objects-record-search.md), commit the parent's line (for a
  sublist subrecord), then submit the parent with `nlapiSubmitRecord`. Unless both the subrecord
  and the parent are committed, the changes are lost.
- NetSuite does not check a subrecord against its parent line (for example, that inventory detail
  quantities match the line). Do that check yourself in `validateLine`.

## Errors and unsupported calls

- Editing or viewing a subrecord that does not exist returns `null`.
- Calling a subrecord API on a field that is not a subrecord field raises an error.
- The field text APIs do not work on subrecords; use the value APIs.
- These functions are not supported on subrecords: `nlapiGetLineItemValue`, `nlapiGetLineItemText`,
  `nlapiFindLineItemValue`, `nlapiGetCurrentLineItemText`, `nlapiGetCurrentLineItemValue`,
  `nlapiGetFieldValue` and `nlapiGetFieldText`.

## Addresses

- On entities, addresses live on the `addressbook` sublist, and each line's address subrecord is in
  the `addressbookaddress` field.
- Two state fields exist: `state` is free text and is not validated; `dropdownstate` is a select
  list used for the United States, Canada and Australia.
- In dynamic mode, set the country first: it decides which address form (and so which fields)
  applies.
- In dynamic mode, address fields cannot be handled with `nlapiGetLineItemValue` or
  `nlapiSetLineItemValue`; use `nlapiGetCurrentLineItemValue` and `nlapiSetCurrentLineItemValue`
  instead.

> [!SOURCE-ERROR] The source writes the current-line setter as `nlapSetCurrentLineItemValue` [sic],
> missing an "i". No function by that name is documented; the documented function is
> `nlapiSetCurrentLineItemValue`.
<!-- source-error: topic:ss1_api.subrecords | - | api_name_written_nlapsetcurrentlineitemvalue -->

- Address field metadata can only be read through the subrecord APIs.
- The company's own address (Company Information) is read through
  [`nlapiLoadConfiguration`](../ss1/utilities.md).
- State names coming from other systems must be converted to NetSuite state codes.
- On a transaction, choose one of the entity's addresses by its internal ID.
- In client scripts deployed to entity or fulfillment records, `validateField`, `fieldChanged` and
  `postSourcing` do not fire for address fields; put such logic in custom code on the address form.

What each deployment can do with addresses:

| Script and deployment | Address access |
|---|---|
| Server script on the parent record | Read, create, edit, delete |
| Client script on the parent record | Read, delete |
| Client script on the address form | Read and edit, with the standard field APIs |

## Inventory detail

- The subrecord field is `inventorydetail`; its line sublist is `inventoryassignment`.
- On an assembly build, the body subrecord field is `inventorydetail` and the component lines use
  `componentinventorydetail`.

| Part | Fields |
|---|---|
| Inventory detail body | `item`, `location`, `tolocation`, `itemdescription`, `quantity`, `baseunitquantity`, `unit`, `totalquantity` |
| `inventoryassignment` lines | `receiptinventorynumber`, `issueinventorynumber`, `binnumber`, `tobinnumber`, `expirationdate`, `quantity`, `quantityavailable` |

`receiptinventorynumber` takes the text of a new serial or lot number; `issueinventorynumber` is a
select of an existing number.

## Example (1.0, for reading legacy code)

Recording the lot produced by an assembly build. Note the full save sequence: commit the
assignment line, commit the subrecord, submit the parent.

```js
function recordBuildLot(buildId, lotText, qty) {
  var build = nlapiLoadRecord('assemblybuild', buildId);
  var detail = build.createSubrecord('inventorydetail');
  detail.selectNewLineItem('inventoryassignment');
  detail.setCurrentLineItemValue('inventoryassignment', 'receiptinventorynumber', lotText);
  detail.setCurrentLineItemValue('inventoryassignment', 'quantity', qty);
  detail.commitLineItem('inventoryassignment');
  detail.commit();
  return nlapiSubmitRecord(build);
}
```
