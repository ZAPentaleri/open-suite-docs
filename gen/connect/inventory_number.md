# Inventory_number (`inventory_number`)
<!-- id: connect:inventory_number -->

| Fact | Value |
|---|---|
| Table | `inventory_number` |
| Record | `inventorynumber` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `available_count` | NUMBER |  | 39 | 0 |  |  | `purpose`: `available_count` |
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `date_last_modified` |
| `expiration_date` | TIMESTAMP |  |  |  |  |  | `purpose`: `expiration_date` |
| `in_transit_count` | NUMBER |  | 39 | 0 |  |  | `purpose`: `in_transit_count` |
| `inventory_number` | VARCHAR2 | 100 |  |  |  |  | `purpose`: `inventory_or_serial_number` |
| `inventory_number_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `inventory_number_id` |
| `item_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `item_id` |
| `location_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `location_id` |
| `memo` | VARCHAR2 | 4000 |  |  |  |  | `purpose`: `memo` |
| `number_type` | VARCHAR2 | 6 |  |  |  |  | `purpose`: `number_type` |
| `on_hand_count` | NUMBER |  | 39 | 0 |  |  | `purpose`: `on_hand_count` |
| `on_order_count` | NUMBER |  | 39 | 0 |  |  | `purpose`: `on_order_count` |
