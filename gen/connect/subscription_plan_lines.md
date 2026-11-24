# Subscription_plan_lines (`subscription_plan_lines`)
<!-- id: connect:subscription_plan_lines -->

| Fact | Value |
|---|---|
| Table | `subscription_plan_lines` |
| Primary key | `plan_line_id` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `billing_mode_id` | VARCHAR2 | 32 |  |  |  |  | `purpose`: `billing_mode_id` |
| `is_prorate_end_date` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `end_date_proration_enabled` |
| `is_prorate_start_date` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `start_date_proration_enabled` |
| `is_required` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `line_required` |
| `item_id` | NUMBER |  | 39 | 0 | `items`.`item_id` | foreign_key | `purpose`: `item_id` |
| `parent_id` | NUMBER |  | 39 | 0 | `subscription_plans`.`plan_id` | foreign_key | `purpose`: `subscription_plan_id` |
| `plan_line_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `subscription_plan_line_id` |
| `plan_line_number` | NUMBER |  | 39 | 0 |  |  | `purpose`: `subscription_plan_line_number` |
| `plan_line_type` | VARCHAR2 | 50 |  |  |  |  | `purpose`: `subscription_plan_line_type` |
| `renewal_option` | VARCHAR2 | 14 |  |  |  |  | `purpose`: `subscription_plan_renewal_option` |
| `revenue_recognition_option_id` | VARCHAR2 | 30 |  |  |  |  | `purpose`: `renewal_recognition_option_id` |
| `usage_multiplier_line_id` | NUMBER |  | 39 | 0 | `subscription_plan_lines`.`plan_line_id` | foreign_key | `purpose`: `usage_multiplier_line_id` |

## Foreign keys

| Key | Columns | References table | Referenced columns |
|---|---|---|---|
| `subscription_plan_lines_items_fk` | `item_id` | `items` | `item_id` |
| `subscription_plan_lines_subscription_plan_lines_fk` | `usage_multiplier_line_id` | `subscription_plan_lines` | `plan_line_id` |
| `subscription_plan_lines_subscription_plans_fk` | `parent_id` | `subscription_plans` | `plan_id` |

## Referenced by

| Key | Table | Columns | Columns here |
|---|---|---|---|
| `Billing_subscription_lines_subscription_plan_lines_fk` | `billing_subscription_lines` | `subscription_plan_line_id` | `plan_line_id` |
| `Price_book_line_intervals_subscription_plan_lines_fk` | `price_book_line_intervals` | `plan_line_id` | `plan_line_id` |
| `Price_book_line_intervals_subscription_plan_lines_fk_2` | `price_book_line_intervals` | `usage_multiplier_line_id` | `plan_line_id` |
| `Subscript_line_price_intervals_subscription_plan_lines_fk` | `subscript_line_price_intervals` | `plan_line_id` | `plan_line_id` |
| `Subscription_plan_lines_subscription_plan_lines_fk` | `subscription_plan_lines` | `usage_multiplier_line_id` | `plan_line_id` |
