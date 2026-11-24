# Units_type (`units_type`)
<!-- id: connect:units_type -->

| Fact | Value |
|---|---|
| Table | `units_type` |
| Record | `unitstype` |
| Primary key | `units_type_id` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `date_last_modified`; `time_zone`: `gmt` |
| `isinactive` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `units_type_inactive` |
| `name` | VARCHAR2 | 40 |  |  |  |  | `purpose`: `user_defined_name` |
| `units_type_extid` | VARCHAR2 | 255 |  |  |  |  | `purpose`: `units_type_external_id` |
| `units_type_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `units_type_id` |

## Referenced by

| Key | Table | Columns | Columns here |
|---|---|---|---|
| `Billing_rate_cards_prices_units_type_fk` | `billing_rate_cards_prices` | `units_type_id` | `units_type_id` |
| `Inventory_items_units_type_fk` | `inventory_items` | `secondary_units_type_id` | `units_type_id` |
