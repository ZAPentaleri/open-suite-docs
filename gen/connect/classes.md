# Classes (`classes`)
<!-- id: connect:classes -->

| Fact | Value |
|---|---|
| Table | `classes` |
| Primary key | `class_id` |
| Domains | Expense_amortization, Invoice_with_amortization, Multibooks |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `class_extid` | VARCHAR2 | 255 |  |  |  |  | `purpose`: `class_external_id` |
| `class_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `class_id` |
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `date_last_modified` |
| `full_name` | VARCHAR2 | 4000 |  |  |  |  | `purpose`: `full_name` |
| `is_including_child_subs` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `include_child_subsidiaries`; `doc_inconsistency`: `class_help_says_department` |
| `isinactive` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `inactive` |
| `name` | VARCHAR2 | 60 |  |  |  |  | `purpose`: `name` |
| `parent_id` | NUMBER |  | 39 | 0 | `classes`.`class_id` | foreign_key | `purpose`: `subclass_of` |

## Foreign keys

| Key | Columns | References table | Referenced columns |
|---|---|---|---|
| `classes_classes_fk` | `parent_id` | `classes` | `class_id` |

## Referenced by

| Key | Table | Columns | Columns here |
|---|---|---|---|
| `Accounts_classes_fk` | `accounts` | `class_id` | `class_id` |
| `Billing_subscription_lines_classes_fk` | `billing_subscription_lines` | `class_id` | `class_id` |
| `Billing_subscriptions_classes_fk` | `billing_subscriptions` | `class_id` | `class_id` |
| `Budget_classes_fk` | `budget` | `class_id` | `class_id` |
| `Classes_classes_fk` | `classes` | `parent_id` | `class_id` |
| `Employee_time_classes_fk` | `employee_time` | `class_id` | `class_id` |
| `Employees_classes_fk` | `employees` | `class_id` | `class_id` |
| `Global_account_map_classes_fk` | `global_account_map` | `class_id` | `class_id` |
| `Item_account_map_classes_fk` | `item_account_map` | `class_id` | `class_id` |
| `Opportunity_lines_classes_fk` | `opportunity_lines` | `class_id` | `class_id` |
| `Posting_account_activity_classes_fk` | `posting_account_activity` | `class_id` | `class_id` |
| `Posting_account_activity_pe_classes_fk` | `posting_account_activity_pe` | `class_id` | `class_id` |
| `Subscription_plans_classes_fk` | `subscription_plans` | `class_id` | `class_id` |
| `Subsidiary_class_map_classes_fk` | `subsidiary_class_map` | `class_id` | `class_id` |

## Suspected source errors

> [!SOURCE-ERROR] `class_help_says_department` (on `is_including_child_subs`)
<!-- source-error: connect:classes | is_including_child_subs | class_help_says_department -->
