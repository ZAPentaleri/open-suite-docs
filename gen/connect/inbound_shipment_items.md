# Inbound_shipment_items (`inbound_shipment_items`)
<!-- id: connect:inbound_shipment_items -->

| Fact | Value |
|---|---|
| Table | `inbound_shipment_items` |
| Primary key | `inboundshipmentitem_id` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `amount` | NUMBER |  | 20 | 2 |  |  | `purpose`: `amount` |
| `currency_id` | NUMBER |  | 39 | 0 | `currencies`.`currency_id` | foreign_key | `purpose`: `currency_id` |
| `date_effective` | TIMESTAMP |  |  |  |  |  | `purpose`: `effective_date` |
| `exchange_rate` | NUMBER |  | 30 | 15 |  |  | `purpose`: `base_exchange_rate` |
| `fx_rate` | NUMBER |  | 30 | 15 |  |  | `purpose`: `exchange_rate` |
| `inboundshipment_id` | NUMBER |  | 39 | 0 | `inbound_shipments`.`inboundshipment_id` | foreign_key | `purpose`: `inbound_shipment_id` |
| `inboundshipmentitem_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `inbound_shipment_item_id` |
| `incoterm_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `incoterm_id` |
| `item_id` | NUMBER |  | 39 | 0 | `items`.`item_id` | foreign_key | `purpose`: `item_id` |
| `item_type` | VARCHAR2 | 11 |  |  |  |  | `purpose`: `item_type` |
| `line_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `line_id` |
| `po_num` | VARCHAR2 | 138 |  |  |  |  | `purpose`: `purchase_order_number` |
| `quantity_billed` | NUMBER |  | 39 | 0 |  |  | `purpose`: `quantity_billed` |
| `quantity_expected` | NUMBER |  | 18 | 8 |  |  | `purpose`: `quantity_expected` |
| `quantity_received` | NUMBER |  | 39 | 0 |  |  | `purpose`: `quantity_received` |
| `quantity_remaining` | NUMBER |  | 39 | 0 |  |  | `purpose`: `quantity_remaining` |
| `receiving_location_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `receiving_location_id` |
| `unit_id` | NUMBER |  | 39 | 0 | `uom`.`uom_id` | foreign_key | `purpose`: `unit_id` |
| `unit_rate` | NUMBER |  | 18 | 8 |  |  | `purpose`: `unit_rate` |
| `vendor_id` | NUMBER |  | 39 | 0 | `vendors`.`vendor_id` | foreign_key | `purpose`: `vendor_id` |
| `vendor_name` | VARCHAR2 | 200 |  |  |  |  | `purpose`: `vendor_name` |

## Foreign keys

| Key | Columns | References table | Referenced columns |
|---|---|---|---|
| `inbound_shipment_items_currencies_fk` | `currency_id` | `currencies` | `currency_id` |
| `inbound_shipment_items_inbound_shipments_fk` | `inboundshipment_id` | `inbound_shipments` | `inboundshipment_id` |
| `inbound_shipment_items_items_fk` | `item_id` | `items` | `item_id` |
| `inbound_shipment_items_uom_fk` | `unit_id` | `uom` | `uom_id` |
| `inbound_shipment_items_vendors_fk` | `vendor_id` | `vendors` | `vendor_id` |
