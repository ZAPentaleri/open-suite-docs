# Transaction_bin_numbers (`transaction_bin_numbers`)
<!-- id: connect:transaction_bin_numbers -->

| Fact | Value |
|---|---|
| Table | `transaction_bin_numbers` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `bin_number` | VARCHAR2 | 100 |  |  |  |  | `purpose`: `transaction_bin_number` |
| `quantity` | NUMBER |  | 39 | 0 |  |  | `purpose`: `items_in_bin` |
| `transaction_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `transaction_id` |
| `transaction_line` | NUMBER |  | 39 | 0 |  |  | `purpose`: `transaction_line_id` |
