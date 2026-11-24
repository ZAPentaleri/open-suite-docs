# Transaction_line_book_map (`transaction_line_book_map`)
<!-- id: connect:transaction_line_book_map -->

| Fact | Value |
|---|---|
| Table | `transaction_line_book_map` |
| Primary key | `accounting_book_id`, `transaction_id`, `transaction_line_id` |
| Domains | Multibooks, Revenue_recognition |
| Tags | `composite_primary_key` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `account_id` | NUMBER |  | 39 | 0 | `accounts`.`account_id` | foreign_key | `purpose`: `account_id` |
| `accounting_book_id` | NUMBER |  | 39 | 0 | `accounting_books`.`accounting_book_id` | primary_key | `purpose`: `accounting_book_id` |
| `alt_sales_amount` | NUMBER |  | 20 | 2 |  |  | `purpose`: `alt_sales_amount` |
| `amortization_residual` | VARCHAR2 | 42 |  |  |  |  | `purpose`: `amortization_residual` |
| `amount` | NUMBER |  | 20 | 2 |  |  | `purpose`: `amount` |
| `amount_linked` | NUMBER |  | 20 | 2 |  |  | `purpose`: `amount_linked` |
| `bill_variance_status` | VARCHAR2 | 128 |  |  |  |  | `purpose`: `bill_variance_status` |
| `catch_up_period_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `catch_up_period_id` |
| `date_created` | TIMESTAMP |  |  |  |  |  | `purpose`: `date_created` |
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `date_last_modified` |
| `date_rev_rec_end` | TIMESTAMP |  |  |  |  |  | `purpose`: `date_revenue_received_end` |
| `date_rev_rec_start` | TIMESTAMP |  |  |  |  |  | `purpose`: `date_revenue_received_start` |
| `date_revenue_committed` | TIMESTAMP |  |  |  |  |  | `purpose`: `date_revenue_committed` |
| `gl_number` | VARCHAR2 | 80 |  |  |  |  | `purpose`: `gl_number` |
| `gl_sequence` | VARCHAR2 | 256 |  |  |  |  | `purpose`: `gl_sequence` |
| `gl_sequence_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `gl_sequence_id` |
| `gross_amount` | NUMBER |  | 39 | 0 |  |  | `purpose`: `gross_amount` |
| `is_hold_rev_rec` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `hold_revenue_recognition_plan` |
| `quantity_committed` | NUMBER |  | 18 | 8 |  |  | `purpose`: `quantity_committed` |
| `schedule_id` | NUMBER |  | 39 | 0 | `revrecschedules`.`schedule_id` | foreign_key | `purpose`: `schedule_id` |
| `transaction_id` | NUMBER |  | 39 | 0 | `transaction_lines`.`transaction_id` | primary_key | `purpose`: `transaction_id` |
| `transaction_line_id` | NUMBER |  | 39 | 0 | `transaction_lines`.`transaction_line_id` | primary_key | `purpose`: `transaction_line_id` |
| `vsoe_allocation` | NUMBER |  | 20 | 2 |  |  | `purpose`: `vsoe_allocation` |

## Foreign keys

| Key | Columns | References table | Referenced columns |
|---|---|---|---|
| `transaction_line_book_map_accounting_books_fk` | `accounting_book_id` | `accounting_books` | `accounting_book_id` |
| `transaction_line_book_map_accounts_fk` | `account_id` | `accounts` | `account_id` |
| `transaction_line_book_map_revrecschedules_fk` | `schedule_id` | `revrecschedules` | `schedule_id` |
| `transaction_line_book_map_transaction_lines_fk` | `transaction_id`, `transaction_line_id` | `transaction_lines` | `transaction_id`, `transaction_line_id` |
