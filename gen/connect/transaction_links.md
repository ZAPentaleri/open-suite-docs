# Transaction_links (`transaction_links`)
<!-- id: connect:transaction_links -->

| Fact | Value |
|---|---|
| Table | `transaction_links` |
| Primary key | `original_transaction_id`, `original_transaction_line_id`, `applied_transaction_id`, `applied_transaction_line_id`, `link_type_code`, `inventory_number` |
| Tags | `composite_primary_key` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `amount_foreign_linked` | NUMBER |  | 20 | 2 |  |  | `purpose`: `foreign_currency_amount_linked` |
| `amount_linked` | NUMBER |  | 20 | 2 |  |  | `purpose`: `amount_linked` |
| `applied_date_posted` | TIMESTAMP |  |  |  |  |  | `purpose`: `applied_date_posted` |
| `applied_transaction_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `applied_transaction_id` |
| `applied_transaction_line_id` | NUMBER |  | 39 | 0 | `transaction_lines`.`transaction_line_id` | primary_key | `purpose`: `applied_transaction_line_id` |
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `date_last_modified`; `time_zone`: `gmt` |
| `discount` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `discount` |
| `inventory_number` | VARCHAR2 | 100 |  |  |  | primary_key | `purpose`: `inventory_number` |
| `link_type` | VARCHAR2 | 4000 |  |  |  |  | `purpose`: `link_type` |
| `link_type_code` | VARCHAR2 | 8 |  |  |  | primary_key | `purpose`: `link_type_code` |
| `original_date_posted` | TIMESTAMP |  |  |  |  |  | `purpose`: `original_date_posted` |
| `original_transaction_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `original_transaction_id` |
| `original_transaction_line_id` | NUMBER |  | 39 | 0 | `transaction_lines`.`transaction_line_id` | primary_key | `purpose`: `original_transaction_line_id` |
| `quantity_linked` | NUMBER |  | 39 | 0 |  |  | `purpose`: `quantity_linked` |

## Foreign keys

| Key | Columns | References table | Referenced columns |
|---|---|---|---|
| `transaction_links_applied_transaction_lines_fk` | `applied_transaction_id`, `applied_transaction_line_id` | `transaction_lines` | `transaction_id`, `transaction_line_id` |
| `transaction_links_applied_transactions_fk` | `applied_transaction_id` | `transactions` | `transaction_id` |
| `transaction_links_original_transaction_lines_fk` | `original_transaction_id`, `original_transaction_line_id` | `transaction_lines` | `transaction_id`, `transaction_line_id` |
| `transaction_links_original_transactions_fk` | `original_transaction_id` | `transactions` | `transaction_id` |
