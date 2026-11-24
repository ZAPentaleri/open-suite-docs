# Amortization_sched_lines (`amortization_sched_lines`)
<!-- id: connect:amortization_sched_lines -->

| Fact | Value |
|---|---|
| Table | `amortization_sched_lines` |
| Primary key | `schedule_id`, `journal_id`, `account_id`, `accounting_period_id` |
| Domains | Expense_amortization, Invoice_with_amortization |
| Tags | `composite_primary_key` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `account_id` | NUMBER |  | 39 | 0 | `accounts`.`account_id` | primary_key | `purpose`: `account_id` |
| `accounting_period_id` | NUMBER |  | 39 | 0 | `accounting_periods`.`accounting_period_id` | primary_key | `purpose`: `accounting_period_id` |
| `amount` | VARCHAR2 | 42 |  |  |  |  | `purpose`: `amount` |
| `is_recognized` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `amount_recognized` |
| `journal_id` | NUMBER |  | 39 | 0 | `transactions`.`transaction_id` | primary_key | `purpose`: `journal_id` |
| `schedule_id` | NUMBER |  | 39 | 0 | `amortization_schedules`.`schedule_id` | primary_key | `purpose`: `amortization_schedule_id` |

## Foreign keys

| Key | Columns | References table | Referenced columns |
|---|---|---|---|
| `amortization_sched_lines_accounting_periods_fk` | `accounting_period_id` | `accounting_periods` | `accounting_period_id` |
| `amortization_sched_lines_accounts_fk` | `account_id` | `accounts` | `account_id` |
| `amortization_sched_lines_amortization_schedules_fk` | `schedule_id` | `amortization_schedules` | `schedule_id` |
| `amortization_sched_lines_transactions_fk` | `journal_id` | `transactions` | `transaction_id` |
