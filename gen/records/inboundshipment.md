# Inbound Shipment (`inboundshipment`)
<!-- id: record:inboundshipment -->

| Fact | Value |
|---|---|
| Internal ID | `inboundshipment` |
| Category | list |
| Client scripts | yes |
| Server scripts | yes |
| Tags | `no_copy`, `no_transform` |

## Record facts

- `script_types`: `client`, `server`
- `not_supported`: `copy`, `transform`
- `script_functions`: `create`, `read`, `update`, `delete`, `search`
- `requires_feature`: `Inbound Shipment Management` [`real_name`]
- `script_types`: `client`, `server`
- `script_functions`: `create`, `read`, `update`, `delete`, `search`
- `not_supported`: `copy`, `transform`
- `line_requires`: `purchase_order_id`, `purchase_order_line_number`
- `purpose`: `in_transit_inventory_and_shipment_status`
- `can`: `bulk_receive_and_bill_lines_from_many_pos`
- `can`: `ownership_transfer_before_receipt`
- `workflow`: `create`, `add_po_lines`, `set_primary_fields`, `update_status`, `update_expected_shipping_date`, `partial_ownership_transfer`, `receive`

## Body fields

| Field ID | Type | Label | Required | submitField | Read-only | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `actualdeliverydate` | date | Actual Delivery Date | no |  |  |  | `purpose`: `actual_delivery_date`; `use_when`: `variance_reporting_and_metrics` |
| `actualshippingdate` | date | Actual Shipping Date | no |  |  |  | `purpose`: `actual_shipping_date`; `use_when`: `variance_reporting_and_metrics` |
| `billoflading` | text | Bill Of Lading | no |  |  |  | `purpose`: `bill_of_lading_number` |
| `expecteddeliverydate` | date | Expected Delivery Date | no |  |  |  | `purpose`: `expected_delivery_date` |
| `expectedshippingdate` | date | Expected Shipping Date | no |  |  |  | `purpose`: `expected_shipping_date`; `can`: `edit_when_circumstances_change` |
| `externaldocumentnumber` | text | External Document Number | no |  |  |  | `purpose`: `carrier_shipment_number`; `supported_in`: `list_customization` |
| `internalid` | integer | Internal ID | no |  | yes |  | `purpose`: `internal_id`; `value_source`: `system_generated` |
| `shipmentcreateddate` | date | Date Created | no |  | yes |  | `purpose`: `shipment_created_date` |
| `shipmentmemo` | textarea | Memo | no |  |  |  | `purpose`: `shipment_memo`; `recommendation`: `log_communication_as_user_note` |
| `shipmentnumber` | text | Shipment Number | no |  |  |  | `purpose`: `shipment_number`; `value_source`: `system_generated`; `configured_by`: `auto_generated_transaction_numbers`; `caveat`: `format_customizable` |
| `shipmentstatus` | select | Status | no |  |  |  | `purpose`: `shipment_status`; `allowed_values`: `To be Shipped`, `In Transit`, `Partially Received`, `Received`, `Closed`; `value_kind`: `shipment_progress` |
| `vesselnumber` | text | Vessel Number | no |  |  |  | `purpose`: `vessel_number` |
| `customform` | select | Custom Form | no |  |  |  |  |
| `externalid` | text | ExternalId | no |  |  |  |  |
| `id` | integer | Internal ID | no |  |  |  |  |
| `shipmentbasecurrency` | select | Base Currency | no |  |  |  |  |
| `shipmentbillingstatus` | text | Billing Status | no |  |  |  |  |

## Sublist `items` (Inbound Shipment Item)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `currency` | select | Currency | no |  |  |
| `currencyprecision` | posinteger | Currency Precision | no |  |  |
| `expectedrate` | currency | Expected Rate | no |  |  |
| `id` | integer | ID | no |  |  |
| `inboundshipment` | select | Inbound Shipment | no |  |  |
| `incoterm` | select | Incoterm | no |  |  |
| `inventorydetail` | summary | Inventory Detail | no |  |  |
| `pocurrency` | select | Currency | no |  |  |
| `porate` | currency | PO Rate | no |  |  |
| `povendor` | text | Vendor | no |  |  |
| `purchaseorder` | select | PO | yes |  |  |
| `quantitybilled` | currency | Quantity Billed | no |  |  |
| `quantityexpected` | currency | Quantity Expected | no |  |  |
| `quantityreceived` | currency | Quantity Received | no |  |  |
| `quantityremaining` | currency | Quantity Remaining | no |  |  |
| `receivinglocation` | select | Receiving Location | no |  |  |
| `shipmentitem` | select | Item | yes |  |  |
| `shipmentitemamount` | currency | Amount | no |  |  |
| `shipmentitemdescription` | textarea | Description | no |  |  |
| `shipmentitemeffectivedate` | date | Effective Date | yes |  |  |
| `shipmentitemexchangerate` | currency2 | Exchange Rate | no |  |  |
| `totalunitcost` | currency2 | Total Unit Cost | no |  |  |
| `unit` | select | Unit | no |  |  |
| `unitlandedcost` | currency2 | Unit Landed Cost | no |  |  |
| `vendor` | text | Vendor | no |  |  |
| `vendorid` | integer | Vendor | no |  |  |

## Sublist `landedcost` (Inbound Shipment Landed Cost)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `landedcostallocationmethod` | select | Allocation Method | yes |  |  |
| `landedcostamount` | currency | Amount | yes |  |  |
| `landedcostcostcategory` | select | Cost Category | yes |  |  |
| `landedcostcurrency` | select | Currency | yes |  |  |
| `landedcosteffectivedate` | date | Effective Date | yes |  |  |
| `landedcostexchangerate` | currency2 | Exchange Rate | yes |  |  |
| `landedcostshipmentitems` | multiselect | Shipment Items | yes |  |  |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `actualdeliverydate` | date | Actual Delivery Date |  |
| `actualshippingdate` | date | Actual Shipping Date |  |
| `billoflading` | text | Bill Of Lading |  |
| `createddate` | datetime | Date Created |  |
| `currency` | select | Currency |  |
| `description` | text | Description |  |
| `expecteddeliverydate` | date | Expected Delivery Date |  |
| `expectedrate` | currency | Expected Rate |  |
| `expectedshippingdate` | date | Expected Shipping Date |  |
| `externaldocumentnumber` | select | External Document Number |  |
| `externalid` | select | External ID |  |
| `externalidstring` | text | External ID (Text) |  |
| `incoterm` | select | Incoterm |  |
| `internalid` | select | Internal Id |  |
| `internalidnumber` | integer | Internal Id Number |  |
| `item` | select | Item |  |
| `landedcostallocationmethod` | select | Landed Cost - Allocation Method |  |
| `landedcostamount` | currency | Landed Cost - Amount |  |
| `landedcostcostcategory` | select | Landed Cost - Cost Category |  |
| `landedcostcurrency` | select | Landed Cost - Currency |  |
| `landedcosteffectivedate` | date | Landed Cost - Effective Date |  |
| `landedcostexchangerate` | float | Landed Cost - Exchange Rate |  |
| `memo` | text | Memo |  |
| `poamount` | currency | Amount |  |
| `porate` | currency | PO Rate |  |
| `purchaseorder` | select | PO |  |
| `quantityexpected` | float | Quantity Expected |  |
| `quantityreceived` | float | Quantity Received |  |
| `quantityremaining` | float | Quantity Remaining |  |
| `receivinglocation` | select | Receiving Location |  |
| `shipmentbasecurrency` | select | Base Currency |  |
| `shipmentnumber` | select | Shipment Number |  |
| `status` | select | Status |  |
| `unit` | select | Unit |  |
| `vendor` | select | Vendor |  |
| `vesselnumber` | text | Vessel Number |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `actualdeliverydate` | date | Actual Delivery Date |  |
| `actualshippingdate` | date | Actual Shipping Date |  |
| `billoflading` | text | Bill Of Lading |  |
| `createddate` | datetime | Date Created |  |
| `currency` | text | Currency |  |
| `description` | text | Description |  |
| `expecteddeliverydate` | date | Expected Delivery Date |  |
| `expectedrate` | currency | Expected Rate |  |
| `expectedshippingdate` | date | Expected Shipping Date |  |
| `externaldocumentnumber` | text | External Document Number |  |
| `externalid` | select | External ID |  |
| `incoterm` | text | Incoterm |  |
| `internalid` | integer | Internal Id |  |
| `internalidnumber` | integer | Internal Id Number |  |
| `item` | text | Item |  |
| `landedcostallocationmethod` | select | Landed Cost - Allocation Method |  |
| `landedcostamount` | currency | Landed Cost - Amount |  |
| `landedcostcostcategory` | select | Landed Cost - Cost Category |  |
| `landedcostcurrency` | select | Landed Cost - Currency |  |
| `landedcosteffectivedate` | date | Landed Cost - Effective Date |  |
| `landedcostexchangerate` | float | Landed Cost - Exchange Rate |  |
| `memo` | text | Memo |  |
| `poamount` | currency | Amount |  |
| `porate` | currency | PO Rate |  |
| `purchaseorder` | text | PO |  |
| `quantitybilled` | float | Quantity Billed |  |
| `quantityexpected` | float | Quantity Expected |  |
| `quantityreceived` | float | Quantity Received |  |
| `quantityremaining` | float | Quantity Remaining |  |
| `receivinglocation` | text | Receiving Location |  |
| `shipmentbasecurrency` | select | Base Currency |  |
| `shipmentnumber` | text | Shipment Number |  |
| `status` | text | Status |  |
| `unit` | text | Unit |  |
| `vendor` | text | Vendor |  |
| `vesselnumber` | text | Vessel Number |  |

## Search joins

| Join ID | Label | Joined record |
|---|---|---|
| `item` | Item | Item |
| `itemreceipt` | Item Receipt | Transaction |
| `purchaseorder` | Purchase Order | Transaction |
| `vendor` | Vendor | Vendor |
