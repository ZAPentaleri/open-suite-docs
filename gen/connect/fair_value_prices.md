# Fair_value_prices (`fair_value_prices`)
<!-- id: connect:fair_value_prices -->

| Fact | Value |
|---|---|
| Table | `fair_value_prices` |
| Record | `fairvalueprice` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `currency_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `currency_id` |
| `date_created` | TIMESTAMP |  |  |  |  |  | `purpose`: `date_created` |
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `date_last_modified` |
| `end_date` | TIMESTAMP |  |  |  |  |  | `purpose`: `end_date` |
| `fair_value_formula` | VARCHAR2 | 200 |  |  |  |  | `purpose`: `fair_value_formula` |
| `fair_value_price_extid` | VARCHAR2 | 255 |  |  |  |  | `purpose`: `fair_value_price_external_id` |
| `fair_value_price_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `fair_value_price_id` |
| `fair_value_range_policy` | VARCHAR2 | 128 |  |  |  |  | `purpose`: `fair_value_range_policy` |
| `high_value` | VARCHAR2 | 42 |  |  |  |  | `purpose`: `high_value` |
| `is_vsoe` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `whether_is_vsoe` |
| `item_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `item_id` |
| `item_revenue_category` | VARCHAR2 | 250 |  |  |  |  | `purpose`: `item_revenue_category` |
| `low_value` | VARCHAR2 | 42 |  |  |  |  | `purpose`: `low_value` |
| `start_date` | TIMESTAMP |  |  |  |  |  | `purpose`: `start_date` |
| `unit_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `unit_id` |
| `unit_price` | NUMBER |  | 30 | 15 |  |  | `purpose`: `unit_price` |
| `units_type_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `units_type_id` |
