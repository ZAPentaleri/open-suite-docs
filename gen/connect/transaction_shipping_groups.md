# Transaction_shipping_groups (`transaction_shipping_groups`)
<!-- id: connect:transaction_shipping_groups -->

| Fact | Value |
|---|---|
| Table | `transaction_shipping_groups` |
| Primary key | `transaction_id`, `shipping_group_id` |
| Tags | `composite_primary_key` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `handling_line_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `handling_line_id` |
| `shipping_group_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `shipping_group_id` |
| `shipping_item_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `shipping_item_id` |
| `shipping_line_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `shipping_line_id` |
| `transaction_address_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `transaction_address_id` |
| `transaction_id` | NUMBER |  | 39 | 0 | `transactions`.`transaction_id` | primary_key | `purpose`: `transaction_id` |

## Foreign keys

| Key | Columns | References table | Referenced columns |
|---|---|---|---|
| `transaction_shipping_groups_transactions_fk` | `transaction_id` | `transactions` | `transaction_id` |

## Referenced by

| Key | Table | Columns | Columns here |
|---|---|---|---|
| `Transaction_lines_transaction_shipping_groups_fk` | `transaction_lines` | `transaction_id`, `shipping_group_id` | `transaction_id`, `shipping_group_id` |
