# Subscription_plans (`subscription_plans`)
<!-- id: connect:subscription_plans -->

| Fact | Value |
|---|---|
| Table | `subscription_plans` |
| Record | `subscriptionplan` |
| Primary key | `plan_id` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `advance_renewal_period_number` | VARCHAR2 | 11 |  |  |  |  | `purpose`: `advance_renewal_period_count`; `requires_together`: `advance_renewal_period_number`, `advance_renewal_period_unit_id`; `effect`: `lead_time_of_automatic_renewal` |
| `advance_renewal_period_unit_id` | VARCHAR2 | 10 |  |  |  |  | `purpose`: `advance_renewal_period_unit`; `requires_together`: `advance_renewal_period_number`, `advance_renewal_period_unit_id`; `effect`: `lead_time_of_automatic_renewal` |
| `class_id` | NUMBER |  | 39 | 0 | `classes`.`class_id` | foreign_key | `purpose`: `class_id` |
| `date_created` | TIMESTAMP |  |  |  |  |  | `purpose`: `date_created` |
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `date_last_modified` |
| `default_renewal_method_id` | VARCHAR2 | 30 |  |  |  |  | `purpose`: `default_renewal_method_id` |
| `default_renewal_plan_id` | NUMBER |  | 39 | 0 | `subscription_plans`.`plan_id` | foreign_key | `purpose`: `default_renewal_plan_id` |
| `default_renewal_term_id` | NUMBER |  | 39 | 0 | `subscription_terms`.`term_id` | foreign_key | `purpose`: `default_renewal_term_id` |
| `default_renewal_trantype_id` | VARCHAR2 | 8 |  |  |  |  | `purpose`: `default_renewal_transaction_type` |
| `department_id` | NUMBER |  | 39 | 0 | `departments`.`department_id` | foreign_key | `purpose`: `department_id` |
| `description` | VARCHAR2 | 4000 |  |  |  |  | `purpose`: `description` |
| `display_name` | VARCHAR2 | 4000 |  |  |  |  | `purpose`: `customer_facing_plan_name` |
| `income_account_id` | NUMBER |  | 39 | 0 | `income_accounts`.`income_account_id` | foreign_key | `purpose`: `income_account_id` |
| `initial_term_id` | NUMBER |  | 39 | 0 | `subscription_terms`.`term_id` | foreign_key | `purpose`: `initial_subscription_term_id` |
| `is_auto_renewal` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `whether_subscription_renews_automatically` |
| `is_inactive` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `subscription_plan_inactive` |
| `is_include_children` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `plan_available_to_child_subsidiaries` |
| `location_id` | NUMBER |  | 39 | 0 | `locations`.`location_id` | foreign_key | `purpose`: `location_id` |
| `name` | VARCHAR2 | 250 |  |  |  |  | `purpose`: `name` |
| `plan_extid` | VARCHAR2 | 255 |  |  |  |  | `purpose`: `plan_external_id` |
| `plan_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `subscription_plan_id` |
| `subsidiary_id` | NUMBER |  | 39 | 0 | `subsidiaries`.`subsidiary_id` | foreign_key | `purpose`: `subsidiary_id` |

## Foreign keys

| Key | Columns | References table | Referenced columns |
|---|---|---|---|
| `subscription_plans_classes_fk` | `class_id` | `classes` | `class_id` |
| `subscription_plans_departments_fk` | `department_id` | `departments` | `department_id` |
| `subscription_plans_income_accounts_fk` | `income_account_id` | `income_accounts` | `income_account_id` |
| `subscription_plans_locations_fk` | `location_id` | `locations` | `location_id` |
| `subscription_plans_subscription_plans_fk` | `default_renewal_plan_id` | `subscription_plans` | `plan_id` |
| `subscription_plans_subscription_terms_fk` | `initial_term_id` | `subscription_terms` | `term_id` |
| `subscription_plans_subscription_terms_fk_2` | `default_renewal_term_id` | `subscription_terms` | `term_id` |
| `subscription_plans_subsidiaries_fk` | `subsidiary_id` | `subsidiaries` | `subsidiary_id` |

## Referenced by

| Key | Table | Columns | Columns here |
|---|---|---|---|
| `Billing_subscriptions_subscription_plans_fk` | `billing_subscriptions` | `default_renewal_plan_id` | `plan_id` |
| `Billing_subscriptions_subscription_plans_fk_2` | `billing_subscriptions` | `plan_id` | `plan_id` |
| `Price_books_subscription_plans_fk` | `price_books` | `plan_id` | `plan_id` |
| `Subscription_change_orders_subscription_plans_fk` | `subscription_change_orders` | `renewal_subscription_plan_id` | `plan_id` |
| `Subscription_plan_lines_subscription_plans_fk` | `subscription_plan_lines` | `parent_id` | `plan_id` |
| `Subscription_plans_subscription_plans_fk` | `subscription_plans` | `default_renewal_plan_id` | `plan_id` |
