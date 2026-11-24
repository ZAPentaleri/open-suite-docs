# Price_book_line_intervals (`price_book_line_intervals`)
<!-- id: connect:price_book_line_intervals -->

| Fact | Value |
|---|---|
| Table | `price_book_line_intervals` |
| Primary key | `pbli_id` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `charge_frequency_id` | VARCHAR2 | 20 |  |  |  |  | `purpose`: `charge_frequency_id` |
| `discount` | NUMBER |  | 39 | 0 |  |  | `purpose`: `discount` |
| `is_discount_percentage` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `discount_is_percentage` |
| `pbli_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `price_book_line_interval_id` |
| `plan_line_id` | NUMBER |  | 39 | 0 | `subscription_plan_lines`.`plan_line_id` | foreign_key | `purpose`: `plan_line_id` |
| `plan_line_number` | NUMBER |  | 39 | 0 |  |  | `purpose`: `plan_line_number` |
| `price_book_id` | NUMBER |  | 39 | 0 | `price_books`.`price_book_id` | foreign_key | `purpose`: `price_book_id` |
| `price_plan_id` | NUMBER |  | 39 | 0 | `price_plans`.`price_plan_id` | foreign_key | `purpose`: `price_plan_id` |
| `repeat_every` | NUMBER |  | 39 | 0 |  |  | `purpose`: `repeat_every` |
| `start_offset` | VARCHAR2 | 70 |  |  |  |  | `purpose`: `start_offset` |
| `usage_multiplier_line_id` | NUMBER |  | 39 | 0 | `subscription_plan_lines`.`plan_line_id` | foreign_key | `purpose`: `usage_multiplier_line_id` |
| `is_overage_discount_percentage` | VARCHAR2 | 3 |  |  |  |  |  |
| `overage_charge_frequency_id` | VARCHAR2 | 20 |  |  |  |  |  |
| `overage_discount` | NUMBER |  | 39 | 0 |  |  |  |
| `overage_price_plan_id` | NUMBER |  | 39 | 0 | `price_plans`.`price_plan_id` | foreign_key |  |
| `overage_repeat_every` | NUMBER |  | 39 | 0 |  |  |  |
| `prorate_by` | VARCHAR2 | 10 |  |  |  |  |  |

## Foreign keys

| Key | Columns | References table | Referenced columns |
|---|---|---|---|
| `price_book_line_intervals_price_books_fk` | `price_book_id` | `price_books` | `price_book_id` |
| `price_book_line_intervals_price_plans_fk` | `price_plan_id` | `price_plans` | `price_plan_id` |
| `price_book_line_intervals_price_plans_fk_2` | `overage_price_plan_id` | `price_plans` | `price_plan_id` |
| `price_book_line_intervals_subscription_plan_lines_fk` | `plan_line_id` | `subscription_plan_lines` | `plan_line_id` |
| `price_book_line_intervals_subscription_plan_lines_fk_2` | `usage_multiplier_line_id` | `subscription_plan_lines` | `plan_line_id` |
