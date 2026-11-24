# Receive Inbound Shipment (`receiveinboundshipment`)
<!-- id: record:receiveinboundshipment -->

| Fact | Value |
|---|---|
| Internal ID | `receiveinboundshipment` |
| Tags | `custom_fields_supported` |

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `inboundshipmentnumber` | text | Inbound Shipment | no |  |  | `purpose`: `inbound_shipment_number`; `value_source`: `related_shipment` |
| `incoterm` | select | Incoterm | no |  |  | `purpose`: `incoterm`; `effect`: `defines_where_customer_takes_ownership` |
| `postingperiod` | select | Posting Period | no |  |  | `purpose`: `posting_period`; `cannot`: `post_to_closed_period` |
| `receivinglocation` | select | Receiving Location | no |  |  | `purpose`: `receiving_location_filter` |
| `trandate` | date | Date | no |  |  | `purpose`: `posting_date`; `default_behavior`: `current_date`; `displayed_on`: `registers` |
| `vendor` | select | Vendor | no |  |  | `purpose`: `preferred_vendor_filter`; `special_values`: `All`; `effect`: `shows_items_at_or_below_reorder_point` |

## Sublist `receiveitems` (Receive Item)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `amount` | currency | Amount | no |  |  |
| `currency` | select | Currency | no |  |  |
| `description` | textarea | Description | no |  |  |
| `incoterm` | select | Incoterm | no |  |  |
| `item` | select | Item | no |  |  |
| `ownershiptransfer` | select | Ownership Transfer | no |  |  |
| `porate` | currency2 | PO Rate | no |  |  |
| `purchaseorder` | select | PO | no |  |  |
| `quantity` | float | Quantity | no |  |  |
| `quantityreceived` | float | Quantity Received | no |  |  |
| `quantitytobereceived` | posfloat | Quantity to be Received | no |  |  |
| `rate` | currency2 | Rate | no |  |  |
| `receiveinboundshipment` | select | Receive Inbound Shipment | no |  |  |
| `receiveitem` | checkbox | Receive | no |  |  |
| `receivinglocation` | select | Receiving Location | no |  |  |
| `unit` | text | Unit | no |  |  |
| `vendor` | text | Vendor | no |  |  |
