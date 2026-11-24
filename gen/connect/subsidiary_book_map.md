# Subsidiary_book_map (`subsidiary_book_map`)
<!-- id: connect:subsidiary_book_map -->

| Fact | Value |
|---|---|
| Table | `subsidiary_book_map` |
| Primary key | `subsidiary_id`, `accounting_book_id` |
| Tags | `composite_primary_key` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `accounting_book_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `accounting_book_id` |
| `contract_defer_expense_acct_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `contract_deferred_expense_account` |
| `contract_expense_acct_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `contract_expense_account_id` |
| `currency_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `currency_id` |
| `exchange_rate` | NUMBER |  | 30 | 15 |  |  | `purpose`: `exchange_rate` |
| `status` | VARCHAR2 | 128 |  |  |  |  | `purpose`: `status` |
| `subsidiary_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `subsidiary_id` |
