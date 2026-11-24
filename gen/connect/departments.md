# Departments (`departments`)
<!-- id: connect:departments -->

| Fact | Value |
|---|---|
| Table | `departments` |
| Record | `department` |
| Primary key | `department_id` |
| Domains | Invoice_with_amortization, Multibooks |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `date_last_modified` |
| `department_extid` | VARCHAR2 | 255 |  |  |  |  | `purpose`: `department_external_id` |
| `department_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `department_id` |
| `full_name` | VARCHAR2 | 4000 |  |  |  |  | `purpose`: `full_name` |
| `is_including_child_subs` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `include_child_subsidiaries`; `doc_inconsistency`: `class_help_says_department` |
| `isinactive` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `department_is_inactive` |
| `name` | VARCHAR2 | 60 |  |  |  |  | `purpose`: `name` |
| `parent_id` | NUMBER |  | 39 | 0 | `departments`.`department_id` | foreign_key | `purpose`: `subdepartment_of` |

## Foreign keys

| Key | Columns | References table | Referenced columns |
|---|---|---|---|
| `departments_departments_fk` | `parent_id` | `departments` | `department_id` |

## Referenced by

| Key | Table | Columns | Columns here |
|---|---|---|---|
| `Account_activity_departments_fk` | `account_activity` | `department_id` | `department_id` |
| `Account_period_activity_departments_fk` | `account_period_activity` | `department_id` | `department_id` |
| `Accounts_departments_fk` | `accounts` | `department_id` | `department_id` |
| `Billing_subscription_lines_departments_fk` | `billing_subscription_lines` | `department_id` | `department_id` |
| `Billing_subscriptions_departments_fk` | `billing_subscriptions` | `department_id` | `department_id` |
| `Budget_departments_fk` | `budget` | `department_id` | `department_id` |
| `Departments_departments_fk` | `departments` | `parent_id` | `department_id` |
| `Employee_time_departments_fk` | `employee_time` | `department_id` | `department_id` |
| `Employees_departments_fk` | `employees` | `department_id` | `department_id` |
| `Global_account_map_departments_fk` | `global_account_map` | `department_id` | `department_id` |
| `Item_account_map_departments_fk` | `item_account_map` | `department_id` | `department_id` |
| `Opportunity_lines_departments_fk` | `opportunity_lines` | `department_id` | `department_id` |
| `Posting_account_activity_departments_fk` | `posting_account_activity` | `department_id` | `department_id` |
| `Posting_account_activity_pe_departments_fk` | `posting_account_activity_pe` | `department_id` | `department_id` |
| `Subscription_plans_departments_fk` | `subscription_plans` | `department_id` | `department_id` |
| `Subsidiary_department_map_departments_fk` | `subsidiary_department_map` | `department_id` | `department_id` |

## Suspected source errors

> [!SOURCE-ERROR] `class_help_says_department` (on `is_including_child_subs`)
<!-- source-error: connect:departments | is_including_child_subs | class_help_says_department -->
