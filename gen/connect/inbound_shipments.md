# Inbound_shipments (`inbound_shipments`)
<!-- id: connect:inbound_shipments -->

| Fact | Value |
|---|---|
| Table | `inbound_shipments` |
| Record | `inboundshipment` |
| Primary key | `inboundshipment_id` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `bill_of_landing` | VARCHAR2 | 200 |  |  |  |  | `purpose`: `bill_of_lading_number`; `doc_inconsistency`: `lading_written_as_landing` |
| `bill_status` | VARCHAR2 | 2 |  |  |  |  | `purpose`: `bill_status` |
| `currency_id` | NUMBER |  | 39 | 0 | `currencies`.`currency_id` | foreign_key | `purpose`: `currency_id` |
| `date_actual_delivery` | TIMESTAMP |  |  |  |  |  | `purpose`: `actual_delivery_date` |
| `date_actual_shipping` | TIMESTAMP |  |  |  |  |  | `purpose`: `actual_shipping_date` |
| `date_create` | TIMESTAMP |  |  |  |  |  | `purpose`: `date_created` |
| `date_expected_delivery` | TIMESTAMP |  |  |  |  |  | `purpose`: `expected_delivery_date` |
| `date_expected_shipping` | TIMESTAMP |  |  |  |  |  | `purpose`: `expected_shipping_date` |
| `inboundshipment_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `inbound_shipment_id` |
| `memo` | VARCHAR2 | 3000 |  |  |  |  | `purpose`: `memo` |
| `shipment_number` | NUMBER |  | 39 | 0 |  |  | `purpose`: `shipment_number` |
| `status` | VARCHAR2 | 64 |  |  |  |  | `purpose`: `inbound_shipment_status` |
| `vessel_number` | VARCHAR2 | 200 |  |  |  |  | `purpose`: `vessel_number` |

## Foreign keys

| Key | Columns | References table | Referenced columns |
|---|---|---|---|
| `inbound_shipments_currencies_fk` | `currency_id` | `currencies` | `currency_id` |

## Referenced by

| Key | Table | Columns | Columns here |
|---|---|---|---|
| `Inbound_shipment_items_inbound_shipments_fk` | `inbound_shipment_items` | `inboundshipment_id` | `inboundshipment_id` |

## Suspected source errors

> [!SOURCE-ERROR] `lading_written_as_landing` (on `bill_of_landing`)
<!-- source-error: connect:inbound_shipments | bill_of_landing | lading_written_as_landing -->
