# Item_supply_plan_lines (`item_supply_plan_lines`)
<!-- id: connect:item_supply_plan_lines -->

| Fact | Value |
|---|---|
| Table | `item_supply_plan_lines` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `is_order_created` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `order_created` |
| `item_supply_plan_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `item_supply_plan_id` |
| `line_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `line_id` |
| `order_date` | TIMESTAMP |  |  |  |  |  | `purpose`: `created_order_transaction_date`; `applies_to`: `purchase_order`, `work_order` |
| `order_type` | VARCHAR2 | 192 |  |  |  |  | `purpose`: `planned_order_type`; `options`: `purchase_order`, `work_order` |
| `quantity` | NUMBER |  | 39 | 0 |  |  | `purpose`: `recommended_quantity_to_replenish` |
| `receipt_date` | TIMESTAMP |  |  |  |  |  | `purpose`: `expected_receipt_date` |
| `source_location_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `source_location_id` |
