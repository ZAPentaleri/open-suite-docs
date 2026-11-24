# Transaction_cost_components (`transaction_cost_components`)
<!-- id: connect:transaction_cost_components -->

| Fact | Value |
|---|---|
| Table | `transaction_cost_components` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `amount` | NUMBER |  | 20 | 2 |  |  | `purpose`: `amount` |
| `cost_category` | VARCHAR2 | 50 |  |  |  |  | `purpose`: `cost_category` |
| `item_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `item_id` |
| `quantity` | NUMBER |  | 39 | 0 |  |  | `purpose`: `quantity` |
| `standard_cost` | NUMBER |  | 30 | 15 |  |  | `purpose`: `standard_cost` |
| `transaction_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `transaction_id` |
| `transaction_line` | NUMBER |  | 39 | 0 |  |  | `purpose`: `transaction_line` |
