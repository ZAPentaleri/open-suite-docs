# Account_activity (`account_activity`)
<!-- id: connect:account_activity -->

| Fact | Value |
|---|---|
| Table | `account_activity` |
| Primary key | `accounting_book_id`, `account_id`, `activity_date`, `subsidiary_id`, `department_id`, `is_hidden_custom_line` |
| Domains | General_accounting |
| Tags | `composite_primary_key` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `account_id` | NUMBER |  | 39 | 0 | `accounts`.`account_id` | primary_key | `purpose`: `account_id` |
| `accounting_book_id` | NUMBER |  | 39 | 0 | `accounting_books`.`accounting_book_id` | primary_key | `purpose`: `accounting_book_id` |
| `activity_date` | TIMESTAMP |  |  |  |  | primary_key | `purpose`: `activity_date` |
| `amount` | NUMBER |  | 39 | 0 |  |  | `purpose`: `amount` |
| `department_id` | NUMBER |  | 39 | 0 | `departments`.`department_id` | primary_key | `purpose`: `department_id` |
| `is_hidden_custom_line` | VARCHAR2 | 3 |  |  |  | primary_key | `purpose`: `custom_line_hidden` |
| `subsidiary_id` | NUMBER |  | 39 | 0 | `subsidiaries`.`subsidiary_id` | primary_key | `purpose`: `subsidiary_id` |

## Foreign keys

| Key | Columns | References table | Referenced columns |
|---|---|---|---|
| `account_activity_accounting_books_fk` | `accounting_book_id` | `accounting_books` | `accounting_book_id` |
| `account_activity_accounts_fk` | `account_id` | `accounts` | `account_id` |
| `account_activity_departments_fk` | `department_id` | `departments` | `department_id` |
| `account_activity_subsidiaries_fk` | `subsidiary_id` | `subsidiaries` | `subsidiary_id` |
