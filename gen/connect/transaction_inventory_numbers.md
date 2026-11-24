# Transaction_inventory_numbers (`transaction_inventory_numbers`)
<!-- id: connect:transaction_inventory_numbers -->

| Fact | Value |
|---|---|
| Table | `transaction_inventory_numbers` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `inventory_number` | VARCHAR2 | 100 |  |  |  |  | `purpose`: `inventory_number` |
| `quantity` | NUMBER |  | 39 | 0 |  |  | `purpose`: `inventory_number_item_count`; `option_meanings`: `serial_number:always_1`, `lot_number:lot_quantity` |
| `transaction_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `transaction_id` |
| `transaction_line` | NUMBER |  | 39 | 0 |  |  | `purpose`: `transaction_line_id` |
