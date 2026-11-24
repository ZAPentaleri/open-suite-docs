# Accounts (`accounts`)
<!-- id: connect:accounts -->

| Fact | Value |
|---|---|
| Table | `accounts` |
| Record | `account` |
| Primary key | `account_id` |
| Domains | Expense_amortization, General_accounting, Invoice_with_amortization, Multibooks, Revenue_recognition |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `account_extid` | VARCHAR2 | 255 |  |  |  |  | `purpose`: `account_external_id` |
| `account_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `account_id` |
| `accountnumber` | VARCHAR2 | 60 |  |  |  |  | `purpose`: `account_number` |
| `cashflow_rate_type` | VARCHAR2 | 10 |  |  |  |  | `purpose`: `cash_flow_rate_type` |
| `category_1099_misc` | VARCHAR2 | 60 |  |  |  |  | `purpose`: `1099_misc_category` |
| `category_1099_misc_mthreshold` | NUMBER |  | 20 | 2 |  |  | `purpose`: `1099_misc_category` |
| `class_id` | NUMBER |  | 39 | 0 | `classes`.`class_id` | foreign_key | `purpose`: `restrict_to_class` |
| `currency_id` | NUMBER |  | 39 | 0 | `currencies`.`currency_id` | foreign_key | `purpose`: `currency_id` |
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `date_last_modified` |
| `deferral_account_id` | NUMBER |  | 39 | 0 | `accounts`.`account_id` | foreign_key | `purpose`: `deferral_account_id` |
| `department_id` | NUMBER |  | 39 | 0 | `departments`.`department_id` | foreign_key | `purpose`: `restrict_to_department` |
| `description` | VARCHAR2 | 25 |  |  |  |  | `purpose`: `description` |
| `full_description` | VARCHAR2 | 60 |  |  |  |  | `purpose`: `full_description` |
| `full_name` | VARCHAR2 | 4000 |  |  |  |  | `purpose`: `full_name` |
| `general_rate_type` | VARCHAR2 | 10 |  |  |  |  | `purpose`: `general_rate_type` |
| `is_balancesheet` | VARCHAR2 | 1 |  |  |  |  | `purpose`: `is_balance_sheet` |
| `is_included_in_elimination` | VARCHAR2 | 1 |  |  |  |  | `purpose`: `include_in_elimination` |
| `is_included_in_reval` | VARCHAR2 | 1 |  |  |  |  | `purpose`: `include_in_revaluation` |
| `is_including_child_subs` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `whether_includes_child_subsidiaries` |
| `is_leftside` | VARCHAR2 | 1 |  |  |  |  | `purpose`: `is_debit` |
| `is_summary` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `is_summary_account` |
| `isinactive` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `account_is_inactive` |
| `legal_name` | VARCHAR2 | 400 |  |  |  |  | `purpose`: `legal_name` |
| `location_id` | NUMBER |  | 39 | 0 | `locations`.`location_id` | foreign_key | `purpose`: `restrict_to_location` |
| `name` | VARCHAR2 | 93 |  |  |  |  | `purpose`: `name` |
| `openbalance` | NUMBER |  | 39 | 0 |  |  | `purpose`: `opening_balance` |
| `parent_id` | NUMBER |  | 39 | 0 | `accounts`.`account_id` | foreign_key | `purpose`: `subaccount_of` |
| `type_name` | VARCHAR2 | 128 |  |  |  |  | `purpose`: `type_name` |
| `type_sequence` | NUMBER |  | 39 | 0 |  |  | `purpose`: `type_sequence` |

## Foreign keys

| Key | Columns | References table | Referenced columns |
|---|---|---|---|
| `accounts_accounts_fk` | `deferral_account_id` | `accounts` | `account_id` |
| `accounts_accounts_fk_2` | `parent_id` | `accounts` | `account_id` |
| `accounts_classes_fk` | `class_id` | `classes` | `class_id` |
| `accounts_currencies_fk` | `currency_id` | `currencies` | `currency_id` |
| `accounts_departments_fk` | `department_id` | `departments` | `department_id` |
| `accounts_locations_fk` | `location_id` | `locations` | `location_id` |

## Referenced by

| Key | Table | Columns | Columns here |
|---|---|---|---|
| `Account_activity_accounts_fk` | `account_activity` | `account_id` | `account_id` |
| `Account_period_activity_accounts_fk` | `account_period_activity` | `account_id` | `account_id` |
| `Account_period_activity_pe_accounts_fk` | `account_period_activity_pe` | `account_id` | `account_id` |
| `Accounts_accounts_fk` | `accounts` | `deferral_account_id` | `account_id` |
| `Accounts_accounts_fk_2` | `accounts` | `parent_id` | `account_id` |
| `Amortization_sched_lines_accounts_fk` | `amortization_sched_lines` | `account_id` | `account_id` |
| `Budget_accounts_fk` | `budget` | `account_id` | `account_id` |
| `Customers_accounts_fk` | `customers` | `default_receivables_account_id` | `account_id` |
| `Employees_accounts_fk` | `employees` | `def_acct_corp_card_expenses_id` | `account_id` |
| `Expense_accounts_accounts_fk` | `expense_accounts` | `expense_account_id` | `account_id` |
| `Expense_amortization_rules_accounts_fk` | `expense_amortization_rules` | `contra_account_id` | `account_id` |
| `Expense_plan_lines_accounts_fk` | `expense_plan_lines` | `deferred_expense_account_id` | `account_id` |
| `Expense_plan_lines_accounts_fk_1` | `expense_plan_lines` | `expense_account_id` | `account_id` |
| `Global_account_map_accounts_fk_1` | `global_account_map` | `destination_account_id` | `account_id` |
| `Global_account_map_accounts_fk_2` | `global_account_map` | `source_account_id` | `account_id` |
| `Income_accounts_accounts_fk` | `income_accounts` | `income_account_id` | `account_id` |
| `Item_account_map_accounts_fk_1` | `item_account_map` | `destination_account_id` | `account_id` |
| `Item_account_map_accounts_fk_2` | `item_account_map` | `source_account_id` | `account_id` |
| `Opportunity_lines_accounts_fk` | `opportunity_lines` | `account_id` | `account_id` |
| `Posting_account_activity_accounts_fk` | `posting_account_activity` | `account_id` | `account_id` |
| `Posting_account_activity_pe_accounts_fk` | `posting_account_activity_pe` | `account_id` | `account_id` |
| `Revaluation_accounts_fk` | `revaluation` | `variance_account_id` | `account_id` |
| `Revenue_plan_lines_accounts_fk` | `revenue_plan_lines` | `deferral_account_id` | `account_id` |
| `Revenue_plan_lines_accounts_fk_1` | `revenue_plan_lines` | `recognition_account_id` | `account_id` |
| `Revenue_plan_version_lines_accounts_fk` | `revenue_plan_version_lines` | `deferral_account_id` | `account_id` |
| `Revenue_plan_version_lines_accounts_fk_1` | `revenue_plan_version_lines` | `recognition_account_id` | `account_id` |
| `Revrecschedulelines_accounts_fk` | `revrecschedulelines` | `account_id` | `account_id` |
| `Sales_reps_accounts_fk` | `sales_reps` | `def_acct_corp_card_expenses_id` | `account_id` |
| `Support_reps_accounts_fk` | `support_reps` | `def_acct_corp_card_expenses_id` | `account_id` |
| `Transaction_history_accounts_fk` | `transaction_history` | `account_id` | `account_id` |
| `Transaction_line_book_map_accounts_fk` | `transaction_line_book_map` | `account_id` | `account_id` |
| `Transaction_lines_accounts_fk` | `transaction_lines` | `account_id` | `account_id` |
| `Transaction_tax_detail_accounts_fk` | `transaction_tax_detail` | `account_id` | `account_id` |
| `Transactions_accounts_fk` | `transactions` | `acct_corp_card_expenses_id` | `account_id` |
