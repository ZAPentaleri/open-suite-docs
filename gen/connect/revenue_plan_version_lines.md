# Revenue_plan_version_lines (`revenue_plan_version_lines`)
<!-- id: connect:revenue_plan_version_lines -->

| Fact | Value |
|---|---|
| Table | `revenue_plan_version_lines` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `accounting_period_id` | NUMBER |  | 39 | 0 | `accounting_periods`.`accounting_period_id` | foreign_key | `purpose`: `accounting_period_id` |
| `amount` | NUMBER |  | 20 | 2 |  |  | `purpose`: `amount` |
| `comments` | VARCHAR2 | 200 |  |  |  |  | `purpose`: `period_comments` |
| `date_created` | TIMESTAMP |  |  |  |  |  | `purpose`: `date_created` |
| `deferral_account_id` | NUMBER |  | 39 | 0 | `accounts`.`account_id` | foreign_key | `purpose`: `deferral_account_id` |
| `journal_id` | NUMBER |  | 39 | 0 | `transactions`.`transaction_id` | foreign_key | `purpose`: `revenue_recognition_journal` |
| `plan_version_id` | NUMBER |  | 39 | 0 | `revenue_plan_versions`.`plan_version_id` | foreign_key | `purpose`: `revenue_plan_version` |
| `planned_revenue_type` | VARCHAR2 | 250 |  |  |  |  | `purpose`: `planned_revenue_type` |
| `recognition_account_id` | NUMBER |  | 39 | 0 | `accounts`.`account_id` | foreign_key | `purpose`: `recognition_account_id` |

## Foreign keys

| Key | Columns | References table | Referenced columns |
|---|---|---|---|
| `revenue_plan_version_lines_accounting_periods_fk` | `accounting_period_id` | `accounting_periods` | `accounting_period_id` |
| `revenue_plan_version_lines_accounts_fk` | `deferral_account_id` | `accounts` | `account_id` |
| `revenue_plan_version_lines_accounts_fk_1` | `recognition_account_id` | `accounts` | `account_id` |
| `revenue_plan_version_lines_revenue_plan_versions_fk` | `plan_version_id` | `revenue_plan_versions` | `plan_version_id` |
| `revenue_plan_version_lines_transactions_fk` | `journal_id` | `transactions` | `transaction_id` |
