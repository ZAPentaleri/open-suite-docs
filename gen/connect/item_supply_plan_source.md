# Item_supply_plan_source (`item_supply_plan_source`)
<!-- id: connect:item_supply_plan_source -->

| Fact | Value |
|---|---|
| Table | `item_supply_plan_source` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `assembly_parent_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `assembly_parent_id` |
| `date_order` | TIMESTAMP |  |  |  |  |  | `purpose`: `date_order` |
| `is_detail_transaction_line` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `is_detail_transaction_line` |
| `item_supply_plan_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `item_supply_plan_id` |
| `quantity` | NUMBER |  | 39 | 0 |  |  | `purpose`: `quantity` |
| `ship_receipt_date` | TIMESTAMP |  |  |  |  |  | `purpose`: `ship_receipt_date` |
| `transaction_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `transaction_id` |
| `transaction_line_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `transaction_line_id` |
| `type_id` | VARCHAR2 | 40 |  |  |  |  | `purpose`: `type_id` |
