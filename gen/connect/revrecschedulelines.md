# Revrecschedulelines (`revrecschedulelines`)
<!-- id: connect:revrecschedulelines -->

| Fact | Value |
|---|---|
| Table | `revrecschedulelines` |
| Domains | General_accounting, Revenue_recognition |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `account_id` | NUMBER |  | 39 | 0 | `accounts`.`account_id` | foreign_key | `purpose`: `account_id` |
| `accounting_period_id` | NUMBER |  | 39 | 0 | `accounting_periods`.`accounting_period_id` | foreign_key | `purpose`: `accounting_period_id` |
| `amount` | VARCHAR2 | 42 |  |  |  |  | `purpose`: `amount` |
| `is_recognized` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `schedule_line_manually_recognized` |
| `journal_id` | NUMBER |  | 39 | 0 | `transactions`.`transaction_id` | foreign_key | `purpose`: `journal_id` |
| `schedule_id` | NUMBER |  | 39 | 0 | `revrecschedules`.`schedule_id` | foreign_key | `purpose`: `revenue_recognition_schedule_id` |

## Foreign keys

| Key | Columns | References table | Referenced columns |
|---|---|---|---|
| `revrecschedulelines_accounting_periods_fk` | `accounting_period_id` | `accounting_periods` | `accounting_period_id` |
| `revrecschedulelines_accounts_fk` | `account_id` | `accounts` | `account_id` |
| `revrecschedulelines_revrecschedules_fk` | `schedule_id` | `revrecschedules` | `schedule_id` |
| `revrecschedulelines_transactions_fk` | `journal_id` | `transactions` | `transaction_id` |
