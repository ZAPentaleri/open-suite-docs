# Expense_plan_lines (`expense_plan_lines`)
<!-- id: connect:expense_plan_lines -->

| Fact | Value |
|---|---|
| Table | `expense_plan_lines` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `accounting_period_id` | NUMBER |  | 39 | 0 | `accounting_periods`.`accounting_period_id` | foreign_key | `purpose`: `accounting_period_id` |
| `amount` | NUMBER |  | 20 | 2 |  |  | `purpose`: `amount` |
| `date_created` | TIMESTAMP |  |  |  |  |  | `purpose`: `date_created` |
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `date_last_modified` |
| `deferred_expense_account_id` | NUMBER |  | 39 | 0 | `accounts`.`account_id` | foreign_key | `purpose`: `deferred_expense_account_id` |
| `expense_account_id` | NUMBER |  | 39 | 0 | `accounts`.`account_id` | foreign_key | `purpose`: `expense_account_id` |
| `is_recognized` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `is_recognized` |
| `journal_id` | NUMBER |  | 39 | 0 | `transactions`.`transaction_id` | foreign_key | `purpose`: `journal_id` |
| `plan_id` | NUMBER |  | 39 | 0 | `expense_plans`.`plan_id` | foreign_key | `purpose`: `plan_id` |
| `planned_expense_type` | VARCHAR2 | 250 |  |  |  |  | `purpose`: `planned_expense_type` |

## Foreign keys

| Key | Columns | References table | Referenced columns |
|---|---|---|---|
| `expense_plan_lines_accounting_periods_fk` | `accounting_period_id` | `accounting_periods` | `accounting_period_id` |
| `expense_plan_lines_accounts_fk` | `deferred_expense_account_id` | `accounts` | `account_id` |
| `expense_plan_lines_accounts_fk_1` | `expense_account_id` | `accounts` | `account_id` |
| `expense_plan_lines_expense_plans_fk` | `plan_id` | `expense_plans` | `plan_id` |
| `expense_plan_lines_transactions_fk` | `journal_id` | `transactions` | `transaction_id` |
