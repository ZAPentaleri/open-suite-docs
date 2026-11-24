# Transaction_book_map (`transaction_book_map`)
<!-- id: connect:transaction_book_map -->

| Fact | Value |
|---|---|
| Table | `transaction_book_map` |
| Primary key | `accounting_book_id`, `transaction_id` |
| Domains | Multibooks |
| Tags | `composite_primary_key` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `accounting_book_id` | NUMBER |  | 39 | 0 | `accounting_books`.`accounting_book_id` | primary_key | `purpose`: `accounting_book_id` |
| `date_revenue_committed` | TIMESTAMP |  |  |  |  |  | `purpose`: `date_revenue_committed` |
| `exchange_rate` | NUMBER |  | 30 | 15 |  |  | `purpose`: `exchange_rate` |
| `is_vsoe_bundle` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `is_vsoe_bundle` |
| `needs_revenue_commitment` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `needs_revenue_commitment` |
| `revenue_commitment_status` | VARCHAR2 | 480 |  |  |  |  | `purpose`: `revenue_commitment_status` |
| `revenue_status` | VARCHAR2 | 480 |  |  |  |  | `purpose`: `revenue_status` |
| `transaction_id` | NUMBER |  | 39 | 0 | `transactions`.`transaction_id` | primary_key | `purpose`: `transaction_id` |

## Foreign keys

| Key | Columns | References table | Referenced columns |
|---|---|---|---|
| `transaction_book_map_accounting_books_fk` | `accounting_book_id` | `accounting_books` | `accounting_book_id` |
| `transaction_book_map_transactions_fk` | `transaction_id` | `transactions` | `transaction_id` |
