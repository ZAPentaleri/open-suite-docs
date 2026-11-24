# Subscription_change_orders (`subscription_change_orders`)
<!-- id: connect:subscription_change_orders -->

| Fact | Value |
|---|---|
| Table | `subscription_change_orders` |
| Record | `subscriptionchangeorder` |
| Primary key | `change_order_id` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `action_id` | VARCHAR2 | 23 |  |  |  |  | `purpose`: `action_id` |
| `approval_status_id` | VARCHAR2 | 18 |  |  |  |  | `purpose`: `approval_status_id` |
| `change_order_extid` | VARCHAR2 | 255 |  |  |  |  | `purpose`: `change_order_external_id` |
| `change_order_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `change_order_id` |
| `change_order_number` | VARCHAR2 | 60 |  |  |  |  | `purpose`: `change_order_number` |
| `date_approval` | TIMESTAMP |  |  |  |  |  | `purpose`: `date_approved` |
| `date_created` | TIMESTAMP |  |  |  |  |  | `purpose`: `date_created` |
| `date_effective` | TIMESTAMP |  |  |  |  |  | `purpose`: `effective_date` |
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `date_last_modified` |
| `date_new_end` | TIMESTAMP |  |  |  |  |  | `purpose`: `new_end_date` |
| `date_renewal_start` | TIMESTAMP |  |  |  |  |  | `purpose`: `renewal_start_date` |
| `is_terminate_at_start_of_day` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `termination_at_start_of_day`; `false_means`: `termination_at_end_of_day` |
| `memo` | VARCHAR2 | 400 |  |  |  |  | `purpose`: `activity_memo`; `caveat`: `long_memos_truncated`; `workaround`: `join_message_table_for_full_message` |
| `modification_type` | VARCHAR2 | 25 |  |  |  |  | `purpose`: `change_order_modification_type`; `effect`: `how_change_order_counts_in_recurring_revenue_analytics` |
| `reactivation_option_id` | VARCHAR2 | 31 |  |  |  |  | `purpose`: `reactivation_option_id` |
| `renewal_method_id` | VARCHAR2 | 30 |  |  |  |  | `purpose`: `renewal_method_id` |
| `renewal_price_book_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `renewal_price_book_id` |
| `renewal_subscription_plan_id` | NUMBER |  | 39 | 0 | `subscription_plans`.`plan_id` | foreign_key | `purpose`: `renewal_subscription_plan_id` |
| `renewal_term_id` | NUMBER |  | 39 | 0 | `subscription_terms`.`term_id` | foreign_key | `purpose`: `renewal_term_id` |
| `renewal_transaction_type` | VARCHAR2 | 8 |  |  |  |  | `purpose`: `renewal_transaction_type` |
| `requester_id` | NUMBER |  | 39 | 0 | `employees`.`employee_id` | foreign_key | `purpose`: `requester_id` |
| `status_id` | VARCHAR2 | 30 |  |  |  |  | `purpose`: `status_id` |
| `subscription_id` | NUMBER |  | 39 | 0 | `billing_subscriptions`.`subscription_id` | foreign_key | `purpose`: `subscription_id` |

## Foreign keys

| Key | Columns | References table | Referenced columns |
|---|---|---|---|
| `subscription_change_orders_billing_subscriptions_fk` | `subscription_id` | `billing_subscriptions` | `subscription_id` |
| `subscription_change_orders_employees_fk` | `requester_id` | `employees` | `employee_id` |
| `subscription_change_orders_subscription_plans_fk` | `renewal_subscription_plan_id` | `subscription_plans` | `plan_id` |
| `subscription_change_orders_subscription_terms_fk` | `renewal_term_id` | `subscription_terms` | `term_id` |

## Referenced by

| Key | Table | Columns | Columns here |
|---|---|---|---|
| `Subscription_co_lines_subscription_change_orders_fk` | `subscription_co_lines` | `change_order_id` | `change_order_id` |
| `Subscription_line_revisions_subscription_change_orders_fk` | `subscription_line_revisions` | `change_order_id` | `change_order_id` |
