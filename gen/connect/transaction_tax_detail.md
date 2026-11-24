# Transaction_tax_detail (`transaction_tax_detail`)
<!-- id: connect:transaction_tax_detail -->

| Fact | Value |
|---|---|
| Table | `transaction_tax_detail` |
| Primary key | `transaction_id`, `transaction_line_id`, `tax_item_id` |
| Domains | General_accounting |
| Tags | `composite_primary_key` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `account_id` | NUMBER |  | 39 | 0 | `accounts`.`account_id` | foreign_key | `purpose`: `account_id` |
| `amount` | NUMBER |  | 20 | 2 |  |  | `purpose`: `amount` |
| `amount_foreign` | NUMBER |  | 20 | 2 |  |  | `purpose`: `amount_in_foreign_currency` |
| `amount_net` | NUMBER |  | 23 | 5 |  |  | `purpose`: `net_amount` |
| `calculation_comment` | VARCHAR2 | 65534 |  |  |  |  | `purpose`: `calculation_comment` |
| `tax_basis_amount` | NUMBER |  | 39 | 0 |  |  | `purpose`: `tax_basis_amount` |
| `tax_item_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `tax_item_id` |
| `tax_rate` | NUMBER |  | 21 | 11 |  |  | `purpose`: `tax_rate` |
| `tax_type` | VARCHAR2 | 5 |  |  |  |  | `purpose`: `tax_type` |
| `transaction_id` | NUMBER |  | 39 | 0 | `transaction_lines`.`transaction_id` | primary_key | `purpose`: `transaction_id` |
| `transaction_line_id` | NUMBER |  | 39 | 0 | `transaction_lines`.`transaction_line_id` | primary_key | `purpose`: `transaction_line_id` |

## Foreign keys

| Key | Columns | References table | Referenced columns |
|---|---|---|---|
| `transaction_tax_detail_accounts_fk` | `account_id` | `accounts` | `account_id` |
| `transaction_tax_detail_items_fk` | `tax_item_id` | `items` | `item_id` |
| `transaction_tax_detail_tax_items_fk` | `tax_item_id` | `tax_items` | `item_id` |
| `transaction_tax_detail_transaction_lines_fk` | `transaction_id`, `transaction_line_id` | `transaction_lines` | `transaction_id`, `transaction_line_id` |
