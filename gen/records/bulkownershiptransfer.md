# Take Ownership (`bulkownershiptransfer`)
<!-- id: record:bulkownershiptransfer -->

| Fact | Value |
|---|---|
| Internal ID | `bulkownershiptransfer` |
| Tags | `custom_fields_supported` |

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `vendor` | select | Vendor | no |  |  | `purpose`: `vendor_filter_for_po_items` |
| `inboundshipmentnumber` | text | Inbound Shipment | no |  |  |  |
| `incoterm` | select | Incoterm | no |  |  |  |
| `postingperiod` | select | Posting Period | no |  |  |  |
| `receivinglocation` | select | Receiving Location | no |  |  |  |
| `trandate` | date | Date | no |  |  |  |

## Sublist `items` (Ownership Transfer Item)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `amount` | currency | Amount | no |  |  |
| `currency` | select | Currency | no |  |  |
| `description` | textarea | Description | no |  |  |
| `exchangerate` | currency2 | Exchange Rate | no |  |  |
| `incoterm` | select | Incoterm | no |  |  |
| `item` | select | Item | no |  |  |
| `ownershiptransfer` | select | Receive Inbound Shipment | no |  |  |
| `porate` | currency2 | PO Rate | no |  |  |
| `process` | checkbox | Process | no |  |  |
| `purchaseorder` | select | PO | no |  |  |
| `quantity` | float | Quantity | no |  |  |
| `rate` | currency2 | Rate | no |  |  |
| `receivinglocation` | select | Receiving Location | no |  |  |
| `unit` | text | Unit | no |  |  |
| `vendor` | text | Vendor | no |  |  |
