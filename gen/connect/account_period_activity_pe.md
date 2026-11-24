# Account_period_activity_pe (`account_period_activity_pe`)
<!-- id: connect:account_period_activity_pe -->

| Fact | Value |
|---|---|
| Table | `account_period_activity_pe` |
| Primary key | `accounting_book_id`, `account_id`, `accounting_period_id`, `subsidiary_id`, `department_id`, `balance_from_subsidiary_id`, `is_hidden_custom_line` |
| Tags | `composite_primary_key` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `account_id` | NUMBER |  | 39 | 0 | `accounts`.`account_id` | primary_key | `purpose`: `account_id` |
| `accounting_book_id` | NUMBER |  | 39 | 0 | `accounting_books`.`accounting_book_id` | primary_key | `purpose`: `accounting_book_id` |
| `accounting_period_id` | NUMBER |  | 39 | 0 | `accounting_periods`.`accounting_period_id` | primary_key | `purpose`: `accounting_period_id` |
| `amount` | NUMBER |  | 39 | 0 |  |  | `purpose`: `amount` |
| `balance_from_subsidiary_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `source_subsidiary_id` |
| `department_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `department_id` |
| `is_hidden_custom_line` | VARCHAR2 | 3 |  |  |  | primary_key | `purpose`: `custom_line_hidden` |
| `subsidiary_id` | NUMBER |  | 39 | 0 | `subsidiaries`.`subsidiary_id` | primary_key | `purpose`: `posting_subsidiary_id` |

## Foreign keys

| Key | Columns | References table | Referenced columns |
|---|---|---|---|
| `account_period_activity_pe_accounting_books_fk` | `accounting_book_id` | `accounting_books` | `accounting_book_id` |
| `account_period_activity_pe_accounting_periods_fk` | `accounting_period_id` | `accounting_periods` | `accounting_period_id` |
| `account_period_activity_pe_accounts_fk` | `account_id` | `accounts` | `account_id` |
| `account_period_activity_pe_subsidiaries_fk` | `subsidiary_id` | `subsidiaries` | `subsidiary_id` |
