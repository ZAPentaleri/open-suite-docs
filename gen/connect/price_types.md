# Price_types (`price_types`)
<!-- id: connect:price_types -->

| Fact | Value |
|---|---|
| Table | `price_types` |
| Primary key | `price_type_id` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `date_last_modified`; `time_zone`: `gmt` |
| `discount_percentage` | NUMBER |  | 5 | 2 |  |  | `purpose`: `markup_discount` |
| `isinactive` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `inactive` |
| `isonline` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `is_online` |
| `name` | VARCHAR2 | 60 |  |  |  |  | `purpose`: `name` |
| `price_type_extid` | VARCHAR2 | 255 |  |  |  |  | `purpose`: `price_type_external_id` |
| `price_type_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `price_type_id` |

## Referenced by

| Key | Table | Columns | Columns here |
|---|---|---|---|
| `Employee_time_price_types_fk` | `employee_time` | `price_type_id` | `price_type_id` |
| `Item_billing_rates_price_types_fk` | `item_billing_rates` | `item_price_id` | `price_type_id` |
| `Item_price_history_price_types_fk` | `item_price_history` | `price_type_id` | `price_type_id` |
| `Opportunity_lines_price_types_fk` | `opportunity_lines` | `price_type_id` | `price_type_id` |
