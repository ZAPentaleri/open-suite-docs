# Price_books (`price_books`)
<!-- id: connect:price_books -->

| Fact | Value |
|---|---|
| Table | `price_books` |
| Record | `pricebook` |
| Primary key | `price_book_id` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `currency` | VARCHAR2 | 4 |  |  |  |  | `purpose`: `price_book_charge_currency`; `value_examples`: `USD` |
| `name` | VARCHAR2 | 32 |  |  |  |  | `purpose`: `price_book_name` |
| `plan_id` | NUMBER |  | 39 | 0 | `subscription_plans`.`plan_id` | foreign_key | `purpose`: `parent_subscription_plan`; `constraint`: `price_book_usable_only_with_this_plan` |
| `price_book_extid` | VARCHAR2 | 255 |  |  |  |  | `purpose`: `price_book_external_id`; `value_source`: `user_assigned`; `use_when`: `linking_records_to_external_systems` |
| `price_book_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `price_book_id`; `value_source`: `system_assigned` |

## Foreign keys

| Key | Columns | References table | Referenced columns |
|---|---|---|---|
| `price_books_subscription_plans_fk` | `plan_id` | `subscription_plans` | `plan_id` |

## Referenced by

| Key | Table | Columns | Columns here |
|---|---|---|---|
| `Price_book_line_intervals_price_books_fk` | `price_book_line_intervals` | `price_book_id` | `price_book_id` |
