# Billing_subscription_lines (`billing_subscription_lines`)
<!-- id: connect:billing_subscription_lines -->

| Fact | Value |
|---|---|
| Table | `billing_subscription_lines` |
| Primary key | `subline_id` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `billing_mode_id` | VARCHAR2 | 32 |  |  |  |  | `purpose`: `billing_mode_id` |
| `catalog_type` | VARCHAR2 | 10 |  |  |  |  | `purpose`: `catalog_type` |
| `class_id` | NUMBER |  | 39 | 0 | `classes`.`class_id` | foreign_key | `purpose`: `class_id` |
| `date_created` | TIMESTAMP |  |  |  |  |  | `purpose`: `date_created` |
| `date_end` | TIMESTAMP |  |  |  |  |  | `purpose`: `end_date` |
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `date_last_modified` |
| `date_recurrence_start` | TIMESTAMP |  |  |  |  |  | `purpose`: `recurring_start_date` |
| `date_start` | TIMESTAMP |  |  |  |  |  | `purpose`: `start_date` |
| `date_termination` | TIMESTAMP |  |  |  |  |  | `purpose`: `termination_date` |
| `department_id` | NUMBER |  | 39 | 0 | `departments`.`department_id` | foreign_key | `purpose`: `department_id` |
| `discount` | NUMBER |  | 18 | 8 |  |  | `purpose`: `discount` |
| `is_discount_percentage` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `discount_is_percentage` |
| `is_include_in_renewal` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `line_included_in_renewal` |
| `is_prorate_end_date` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `end_date_prorated` |
| `is_prorate_start_date` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `start_date_prorated` |
| `item_id` | NUMBER |  | 39 | 0 | `items`.`item_id` | foreign_key | `purpose`: `item_id` |
| `line_type` | VARCHAR2 | 50 |  |  |  |  | `purpose`: `line_type` |
| `location_id` | NUMBER |  | 39 | 0 | `locations`.`location_id` | foreign_key | `purpose`: `location_id` |
| `period_amount` | NUMBER |  | 20 | 2 |  |  | `purpose`: `period_amount` |
| `price_interval_group_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `price_interval_group_id` |
| `purchase_order_id` | VARCHAR2 | 138 |  |  | `transactions`.`transaction_id` | foreign_key | `purpose`: `purchase_order_id` |
| `quantity` | NUMBER |  | 18 | 8 |  |  | `purpose`: `quantity` |
| `recurring_amount` | NUMBER |  | 20 | 2 |  |  | `purpose`: `recurring_revenue_over_term` |
| `sales_order_id` | NUMBER |  | 39 | 0 | `transactions`.`transaction_id` | foreign_key | `purpose`: `sales_order_id` |
| `sales_order_line_number` | NUMBER |  | 39 | 0 |  |  | `purpose`: `sales_order_line_number` |
| `status_id` | VARCHAR2 | 25 |  |  |  |  | `purpose`: `status_id` |
| `subline_extid` | VARCHAR2 | 255 |  |  |  |  | `purpose`: `subline_external_id` |
| `subline_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `subline_id` |
| `subline_number` | NUMBER |  | 39 | 0 |  |  | `purpose`: `subline_number` |
| `subscription_id` | NUMBER |  | 39 | 0 | `billing_subscriptions`.`subscription_id` | foreign_key | `purpose`: `subscription_id` |
| `subscription_plan_line_id` | NUMBER |  | 39 | 0 | `subscription_plan_lines`.`plan_line_id` | foreign_key | `purpose`: `subscription_plan_line_id` |
| `total` | NUMBER |  | 35 | 15 |  |  | `purpose`: `total` |
| `usage_multiplier_line_id` | NUMBER |  | 39 | 0 | `billing_subscription_lines`.`subline_id` | foreign_key | `purpose`: `usage_multiplier_line_id` |

## Foreign keys

| Key | Columns | References table | Referenced columns |
|---|---|---|---|
| `billing_subscription_lines_billing_subscription_lines_fk` | `usage_multiplier_line_id` | `billing_subscription_lines` | `subline_id` |
| `billing_subscription_lines_billing_subscriptions_fk` | `subscription_id` | `billing_subscriptions` | `subscription_id` |
| `billing_subscription_lines_classes_fk` | `class_id` | `classes` | `class_id` |
| `billing_subscription_lines_departments_fk` | `department_id` | `departments` | `department_id` |
| `billing_subscription_lines_items_fk` | `item_id` | `items` | `item_id` |
| `billing_subscription_lines_locations_fk` | `location_id` | `locations` | `location_id` |
| `billing_subscription_lines_subscription_plan_lines_fk` | `subscription_plan_line_id` | `subscription_plan_lines` | `plan_line_id` |
| `billing_subscription_lines_transactions_fk` | `purchase_order_id` | `transactions` | `transaction_id` |
| `billing_subscription_lines_transactions_fk_2` | `sales_order_id` | `transactions` | `transaction_id` |

## Referenced by

| Key | Table | Columns | Columns here |
|---|---|---|---|
| `Billing_subscription_lines_billing_subscription_lines_fk` | `billing_subscription_lines` | `usage_multiplier_line_id` | `subline_id` |
| `Subscript_line_price_intervals_billing_subscription_lines_fk` | `subscript_line_price_intervals` | `subscription_line_id` | `subline_id` |
| `Subscription_co_lines_billing_subscription_lines_fk` | `subscription_co_lines` | `subscription_line_id` | `subline_id` |
| `Subscription_line_revisions_billing_subscription_lines_fk` | `subscription_line_revisions` | `subscription_line_id` | `subline_id` |
