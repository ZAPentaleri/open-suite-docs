# Subscript_line_price_intervals (`subscript_line_price_intervals`)
<!-- id: connect:subscript_line_price_intervals -->

| Fact | Value |
|---|---|
| Table | `subscript_line_price_intervals` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `charge_frequency_id` | VARCHAR2 | 20 |  |  |  |  | `purpose`: `charge_frequency_id` |
| `date_end_exclusive` | TIMESTAMP |  |  |  |  |  | `purpose`: `end_date_exclusive` |
| `date_start_inclusive` | TIMESTAMP |  |  |  |  |  | `purpose`: `start_date_inclusive` |
| `discount` | NUMBER |  | 39 | 0 |  |  | `purpose`: `discount` |
| `included_quantity` | NUMBER |  | 39 | 0 |  |  | `purpose`: `included_quantity` |
| `is_discount_percentage` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `discount_is_percentage` |
| `plan_line_id` | NUMBER |  | 39 | 0 | `subscription_plan_lines`.`plan_line_id` | foreign_key | `purpose`: `plan_line_id` |
| `plan_line_number` | NUMBER |  | 39 | 0 |  |  | `purpose`: `plan_line_number` |
| `price_plan_id` | NUMBER |  | 39 | 0 | `price_plans`.`price_plan_id` | foreign_key | `purpose`: `price_plan_id` |
| `prorate_by` | VARCHAR2 | 10 |  |  |  |  | `purpose`: `month_or_day` |
| `quantity` | NUMBER |  | 18 | 8 |  |  | `purpose`: `quantity` |
| `repeat_every` | NUMBER |  | 39 | 0 |  |  | `purpose`: `repeat_every` |
| `status_id` | VARCHAR2 | 25 |  |  |  |  | `purpose`: `status_id` |
| `subscription_line_id` | NUMBER |  | 39 | 0 | `billing_subscription_lines`.`subline_id` | foreign_key | `purpose`: `subscription_line_id` |
| `prorate_by_option` | VARCHAR2 | 50 |  |  |  |  |  |
| `total_interval_value` | NUMBER |  | 39 | 0 |  |  |  |

## Foreign keys

| Key | Columns | References table | Referenced columns |
|---|---|---|---|
| `subscript_line_price_intervals_billing_subscription_lines_fk` | `subscription_line_id` | `billing_subscription_lines` | `subline_id` |
| `subscript_line_price_intervals_price_plans_fk` | `price_plan_id` | `price_plans` | `price_plan_id` |
| `subscript_line_price_intervals_subscription_plan_lines_fk` | `plan_line_id` | `subscription_plan_lines` | `plan_line_id` |
