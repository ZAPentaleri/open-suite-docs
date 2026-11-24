# Billing_subscriptions (`billing_subscriptions`)
<!-- id: connect:billing_subscriptions -->

| Fact | Value |
|---|---|
| Table | `billing_subscriptions` |
| Primary key | `subscription_id` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `advance_renewal_period_number` | NUMBER |  | 39 | 0 |  |  | `purpose`: `auto_renewal_lead_periods` |
| `advance_renewal_period_unit_id` | VARCHAR2 | 10 |  |  |  |  | `purpose`: `advance_renewal_period_unit` |
| `amount` | NUMBER |  | 35 | 15 |  |  | `purpose`: `subscription_total_amount` |
| `billing_account_id` | NUMBER |  | 39 | 0 | `billing_accounts`.`billing_account_id` | foreign_key | `purpose`: `subscription_billing_account` |
| `class_id` | NUMBER |  | 39 | 0 | `classes`.`class_id` | foreign_key | `purpose`: `class_id` |
| `currency` | VARCHAR2 | 4 |  |  |  |  | `purpose`: `currency` |
| `customer_id` | NUMBER |  | 39 | 0 | `customers`.`customer_id` | foreign_key | `purpose`: `customer_id` |
| `date_created` | TIMESTAMP |  |  |  |  |  | `purpose`: `date_subscription_was_created` |
| `date_end` | TIMESTAMP |  |  |  |  |  | `purpose`: `date_subscription_ends` |
| `date_estimated_revrec_end` | TIMESTAMP |  |  |  |  |  | `purpose`: `estimated_revenue_recognition_end`; `applies_to`: `evergreen_subscriptions`; `used_by`: `revenue_recognition_calculation` |
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `date_subscription_last_modified` |
| `date_start` | TIMESTAMP |  |  |  |  |  | `purpose`: `start_date` |
| `default_renewal_method_id` | VARCHAR2 | 30 |  |  |  |  | `purpose`: `default_renewal_method_id` |
| `default_renewal_plan_id` | NUMBER |  | 39 | 0 | `subscription_plans`.`plan_id` | foreign_key | `purpose`: `default_renewal_plan_id` |
| `default_renewal_price_book_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `default_renewal_price_book` |
| `default_renewal_term_id` | NUMBER |  | 39 | 0 | `subscription_terms`.`term_id` | foreign_key | `purpose`: `default_renewal_term_id` |
| `default_renewal_trantype_id` | VARCHAR2 | 8 |  |  |  |  | `purpose`: `default_renewal_transaction_id` |
| `department_id` | NUMBER |  | 39 | 0 | `departments`.`department_id` | foreign_key | `purpose`: `department_id` |
| `description` | VARCHAR2 | 400 |  |  |  |  | `purpose`: `description_of_subscription` |
| `generate_modification_elements` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `generate_modification_elements` |
| `initial_term_id` | NUMBER |  | 39 | 0 | `subscription_terms`.`term_id` | foreign_key | `purpose`: `initial_term_id` |
| `is_auto_renewal` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `subscription_auto_renews` |
| `is_sub_start_date_as_rsd` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `start_date_used_as_recurrence_start` |
| `location_id` | NUMBER |  | 39 | 0 | `locations`.`location_id` | foreign_key | `purpose`: `location_id` |
| `name` | VARCHAR2 | 220 |  |  |  |  | `purpose`: `subscription_name` |
| `plan_id` | NUMBER |  | 39 | 0 | `subscription_plans`.`plan_id` | foreign_key | `purpose`: `price_plan_id` |
| `price_book_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `price_book_id` |
| `sales_order_id` | NUMBER |  | 39 | 0 | `transactions`.`transaction_id` | foreign_key | `purpose`: `sales_order_id` |
| `subscription_extid` | VARCHAR2 | 255 |  |  |  |  | `purpose`: `external_id` |
| `subscription_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `subscription_id` |
| `subscription_number` | VARCHAR2 | 220 |  |  |  |  | `purpose`: `number_of_the_subscription` |
| `subscription_revision` | NUMBER |  | 39 | 0 |  |  | `purpose`: `number_of_revisions` |
| `subscription_status` | VARCHAR2 | 25 |  |  |  |  | `purpose`: `subscription_status` |
| `subsidiary_id` | NUMBER |  | 39 | 0 | `subsidiaries`.`subsidiary_id` | foreign_key | `purpose`: `subsidiary_id` |

## Foreign keys

| Key | Columns | References table | Referenced columns |
|---|---|---|---|
| `billing_subscriptions_billing_accounts_fk` | `billing_account_id` | `billing_accounts` | `billing_account_id` |
| `billing_subscriptions_classes_fk` | `class_id` | `classes` | `class_id` |
| `billing_subscriptions_customers_fk` | `customer_id` | `customers` | `customer_id` |
| `billing_subscriptions_departments_fk` | `department_id` | `departments` | `department_id` |
| `billing_subscriptions_locations_fk` | `location_id` | `locations` | `location_id` |
| `billing_subscriptions_subscription_plans_fk` | `default_renewal_plan_id` | `subscription_plans` | `plan_id` |
| `billing_subscriptions_subscription_plans_fk_2` | `plan_id` | `subscription_plans` | `plan_id` |
| `billing_subscriptions_subscription_terms_fk` | `default_renewal_term_id` | `subscription_terms` | `term_id` |
| `billing_subscriptions_subscription_terms_fk_2` | `initial_term_id` | `subscription_terms` | `term_id` |
| `billing_subscriptions_subsidiaries_fk` | `subsidiary_id` | `subsidiaries` | `subsidiary_id` |
| `billing_subscriptions_transactions_fk` | `sales_order_id` | `transactions` | `transaction_id` |

## Referenced by

| Key | Table | Columns | Columns here |
|---|---|---|---|
| `Billing_subscription_lines_billing_subscriptions_fk` | `billing_subscription_lines` | `subscription_id` | `subscription_id` |
| `Subscription_change_orders_billing_subscriptions_fk` | `subscription_change_orders` | `subscription_id` | `subscription_id` |
| `Subscription_co_lines_billing_subscriptions_fk` | `subscription_co_lines` | `subscription_id` | `subscription_id` |
