# Accounting_periods (`accounting_periods`)
<!-- id: connect:accounting_periods -->

| Fact | Value |
|---|---|
| Table | `accounting_periods` |
| Record | `accountingperiod` |
| Primary key | `accounting_period_id` |
| Domains | Expense_amortization, General_accounting, Invoice_with_amortization, Revenue_recognition |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `accounting_period_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `accounting_period_id` |
| `closed` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `period_closed` |
| `closed_accounts_payable` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `period_closed_for_payables`; `caveat`: `backward_compatibility_only` |
| `closed_accounts_receivable` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `period_closed_for_receivables`; `caveat`: `backward_compatibility_only` |
| `closed_all` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `period_closed_for_all`; `caveat`: `backward_compatibility_only` |
| `closed_on` | TIMESTAMP |  |  |  |  |  | `purpose`: `date_period_was_closed` |
| `closed_payroll` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `period_closed_for_payroll`; `caveat`: `backward_compatibility_only` |
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `date_last_modified` |
| `ending` | TIMESTAMP |  |  |  |  |  | `purpose`: `end_date` |
| `fiscal_calendar_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `fiscal_calendar_id` |
| `full_name` | VARCHAR2 | 597 |  |  |  |  | `purpose`: `full_name` |
| `is_adjustment` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `is_adjustment_period` |
| `isinactive` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `accounting_period_is_inactive` |
| `locked_accounts_payable` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `whether_a_p_locked` |
| `locked_accounts_receivable` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `whether_a_r_locked` |
| `locked_all` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `whether_all_locked` |
| `locked_payroll` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `whether_payroll_locked` |
| `name` | VARCHAR2 | 64 |  |  |  |  | `purpose`: `name` |
| `parent_id` | NUMBER |  | 39 | 0 | `accounting_periods`.`accounting_period_id` | foreign_key | `purpose`: `subperiod_of` |
| `quarter` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `period_is_a_quarter` |
| `starting` | TIMESTAMP |  |  |  |  |  | `purpose`: `start_date` |
| `year_0` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `year` |
| `year_id` | NUMBER |  | 39 | 0 | `accounting_periods`.`accounting_period_id` | foreign_key | `purpose`: `year_of_accounting_period` |

## Foreign keys

| Key | Columns | References table | Referenced columns |
|---|---|---|---|
| `accounting_periods_accounting_periods_fk` | `parent_id` | `accounting_periods` | `accounting_period_id` |
| `accounting_periods_accounting_periods_fk_2` | `year_id` | `accounting_periods` | `accounting_period_id` |

## Referenced by

| Key | Table | Columns | Columns here |
|---|---|---|---|
| `Account_period_activity_accounting_periods_fk` | `account_period_activity` | `accounting_period_id` | `accounting_period_id` |
| `Account_period_activity_pe_accounting_periods_fk` | `account_period_activity_pe` | `accounting_period_id` | `accounting_period_id` |
| `Accounting_periods_accounting_periods_fk` | `accounting_periods` | `parent_id` | `accounting_period_id` |
| `Accounting_periods_accounting_periods_fk_2` | `accounting_periods` | `year_id` | `accounting_period_id` |
| `Amortization_sched_lines_accounting_periods_fk` | `amortization_sched_lines` | `accounting_period_id` | `accounting_period_id` |
| `Budget_accounting_periods_fk` | `budget` | `accounting_period_id` | `accounting_period_id` |
| `Consolidated_exchange_rates_accounting_periods_fk` | `consolidated_exchange_rates` | `accounting_period_id` | `accounting_period_id` |
| `Customers_accounting_periods_fk` | `customers` | `first_sale_period_id` | `accounting_period_id` |
| `Customers_accounting_periods_fk_2` | `customers` | `last_sale_period_id` | `accounting_period_id` |
| `Expense_plan_lines_accounting_periods_fk` | `expense_plan_lines` | `accounting_period_id` | `accounting_period_id` |
| `Expense_plans_accounting_periods_fk` | `expense_plans` | `accounting_period_id` | `accounting_period_id` |
| `Opportunities_accounting_periods_fk` | `opportunities` | `accounting_period_id` | `accounting_period_id` |
| `Posting_account_activity_accounting_periods_fk` | `posting_account_activity` | `accounting_period_id` | `accounting_period_id` |
| `Posting_account_activity_pe_accounting_periods_fk` | `posting_account_activity_pe` | `accounting_period_id` | `accounting_period_id` |
| `Revenue_plan_lines_accounting_periods_fk` | `revenue_plan_lines` | `accounting_period_id` | `accounting_period_id` |
| `Revenue_plan_version_lines_accounting_periods_fk` | `revenue_plan_version_lines` | `accounting_period_id` | `accounting_period_id` |
| `Revenue_plans_accounting_periods_fk` | `revenue_plans` | `accounting_period_id` | `accounting_period_id` |
| `Revrecschedulelines_accounting_periods_fk` | `revrecschedulelines` | `accounting_period_id` | `accounting_period_id` |
