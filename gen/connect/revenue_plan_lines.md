# Revenue_plan_lines (`revenue_plan_lines`)
<!-- id: connect:revenue_plan_lines -->

| Fact | Value |
|---|---|
| Table | `revenue_plan_lines` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `accounting_period_id` | NUMBER |  | 39 | 0 | `accounting_periods`.`accounting_period_id` | foreign_key | `purpose`: `accounting_period_id` |
| `amount` | NUMBER |  | 20 | 2 |  |  | `purpose`: `amount` |
| `date_created` | TIMESTAMP |  |  |  |  |  | `purpose`: `date_created` |
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `date_last_modified` |
| `deferral_account_id` | NUMBER |  | 39 | 0 | `accounts`.`account_id` | foreign_key | `purpose`: `deferral_account_id` |
| `is_recognized` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `revenue_recognized` |
| `journal_id` | NUMBER |  | 39 | 0 | `transactions`.`transaction_id` | foreign_key | `purpose`: `journal_id` |
| `plan_id` | NUMBER |  | 39 | 0 | `revenue_plans`.`plan_id` | foreign_key | `purpose`: `plan_id` |
| `planned_revenue_type` | VARCHAR2 | 250 |  |  |  |  | `purpose`: `planned_revenue_type` |
| `recognition_account_id` | NUMBER |  | 39 | 0 | `accounts`.`account_id` | foreign_key | `purpose`: `recognition_account_id` |

## Foreign keys

| Key | Columns | References table | Referenced columns |
|---|---|---|---|
| `revenue_plan_lines_accounting_periods_fk` | `accounting_period_id` | `accounting_periods` | `accounting_period_id` |
| `revenue_plan_lines_accounts_fk` | `deferral_account_id` | `accounts` | `account_id` |
| `revenue_plan_lines_accounts_fk_1` | `recognition_account_id` | `accounts` | `account_id` |
| `revenue_plan_lines_revenue_plans_fk` | `plan_id` | `revenue_plans` | `plan_id` |
| `revenue_plan_lines_transactions_fk` | `journal_id` | `transactions` | `transaction_id` |
