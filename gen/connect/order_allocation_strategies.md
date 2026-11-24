# Order_allocation_strategies (`order_allocation_strategies`)
<!-- id: connect:order_allocation_strategies -->

| Fact | Value |
|---|---|
| Table | `order_allocation_strategies` |
| Primary key | `order_allocation_strategy_id` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `allocation_quantities` | VARCHAR2 | 18 |  |  |  |  | `purpose`: `allocation_quantities` |
| `allocation_type` | VARCHAR2 | 19 |  |  |  |  | `purpose`: `allocation_type` |
| `date_created` | TIMESTAMP |  |  |  |  |  | `purpose`: `date_created` |
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `date_last_modified` |
| `is_future_inventory` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `is_future_inventory` |
| `is_inactive` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `is_inactive` |
| `name` | VARCHAR2 | 100 |  |  |  |  | `purpose`: `name` |
| `order_allocation_strategy_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `order_allocation_strategy_id` |

## Referenced by

| Key | Table | Columns | Columns here |
|---|---|---|---|
| `Transaction_lines_order_allocation_strategies_fk` | `transaction_lines` | `order_allocation_strategy_id` | `order_allocation_strategy_id` |
