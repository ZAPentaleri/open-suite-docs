# Expense_accounts (`expense_accounts`)
<!-- id: connect:expense_accounts -->

| Fact | Value |
|---|---|
| Table | `expense_accounts` |
| Primary key | `expense_account_id` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `account_number` | VARCHAR2 | 60 |  |  |  |  | `purpose`: `account_number` |
| `comments` | VARCHAR2 | 25 |  |  |  |  | `purpose`: `comments` |
| `current_balance` | NUMBER |  | 39 | 0 |  |  | `purpose`: `current_balance` |
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `date_last_modified` |
| `desription` | VARCHAR2 | 60 |  |  |  |  | `purpose`: `description` |
| `expense_account_extid` | VARCHAR2 | 255 |  |  |  |  | `purpose`: `expense_account_external_id` |
| `expense_account_id` | NUMBER |  | 39 | 0 | `accounts`.`account_id` | primary_key | `purpose`: `expense_account_id` |
| `full_name` | VARCHAR2 | 4000 |  |  |  |  | `purpose`: `full_name` |
| `is_including_child_subs` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `whether_includes_child_subsidiaries` |
| `is_summary` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `whether_is_summary` |
| `isinactive` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `account_is_inactive` |
| `legal_name` | VARCHAR2 | 400 |  |  |  |  | `purpose`: `legal_name` |
| `name` | VARCHAR2 | 93 |  |  |  |  | `purpose`: `name` |
| `parent_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `subaccount_of` |

## Foreign keys

| Key | Columns | References table | Referenced columns |
|---|---|---|---|
| `expense_accounts_accounts_fk` | `expense_account_id` | `accounts` | `account_id` |
