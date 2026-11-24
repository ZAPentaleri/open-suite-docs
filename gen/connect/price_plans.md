# Price_plans (`price_plans`)
<!-- id: connect:price_plans -->

| Fact | Value |
|---|---|
| Table | `price_plans` |
| Record | `priceplan` |
| Primary key | `price_plan_id` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `currency` | VARCHAR2 | 4 |  |  |  |  | `purpose`: `price_plan_charge_currency`; `value_examples`: `USD` |
| `date_created` | TIMESTAMP |  |  |  |  |  | `purpose`: `price_plan_created_date` |
| `included_quantity` | NUMBER |  | 18 | 8 |  |  | `purpose`: `included_units_before_extra_charges` |
| `price_plan_extid` | VARCHAR2 | 255 |  |  |  |  | `purpose`: `price_plan_external_id`; `value_source`: `user_assigned`; `use_when`: `linking_records_to_external_systems` |
| `price_plan_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `price_plan_id`; `value_source`: `system_assigned`; `used_by`: `price_books`, `subscription_pricing`, `subscription_change_orders` |
| `price_plan_type` | VARCHAR2 | 50 |  |  |  |  | `purpose`: `price_plan_type`; `options`: `tiered`, `volume`; `option_meanings`: `volume:all_quantity_priced_at_its_tier`, `tiered:quantity_fills_tiers_in_order` |

## Referenced by

| Key | Table | Columns | Columns here |
|---|---|---|---|
| `Price_book_line_intervals_price_plans_fk` | `price_book_line_intervals` | `price_plan_id` | `price_plan_id` |
| `Price_book_line_intervals_price_plans_fk_2` | `price_book_line_intervals` | `overage_price_plan_id` | `price_plan_id` |
| `Price_tiers_price_plans_fk` | `price_tiers` | `price_plan_id` | `price_plan_id` |
| `Subscript_line_price_intervals_price_plans_fk` | `subscript_line_price_intervals` | `price_plan_id` | `price_plan_id` |
| `Subscription_line_revisions_price_plans_fk` | `subscription_line_revisions` | `price_plan_id` | `price_plan_id` |
| `Subscription_line_revisions_price_plans_fk_2` | `subscription_line_revisions` | `overage_price_plan_id` | `price_plan_id` |
