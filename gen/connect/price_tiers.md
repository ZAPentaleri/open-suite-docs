# Price_tiers (`price_tiers`)
<!-- id: connect:price_tiers -->

| Fact | Value |
|---|---|
| Table | `price_tiers` |
| Primary key | `price_tier_id` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `from_quantity` | NUMBER |  | 39 | 0 |  |  | `purpose`: `tier_start_quantity`; `caveat`: `tier_runs_to_next_tier_start_quantity` |
| `maximum_quantity` | NUMBER |  | 20 | 2 |  |  | `purpose`: `tier_spend_ceiling`; `effect`: `no_further_charge_after_ceiling_amount_spent`; `value_examples`: `500.00`; `doc_inconsistency`: `quantity_column_described_as_currency_amount` |
| `minimum_quantity` | NUMBER |  | 20 | 2 |  |  | `purpose`: `tier_minimum_spend`; `effect`: `customer_pays_at_least_minimum_amount_even_for_quantity_1`; `doc_inconsistency`: `quantity_column_described_as_currency_amount` |
| `price_plan_id` | NUMBER |  | 39 | 0 | `price_plans`.`price_plan_id` | foreign_key | `purpose`: `price_plan`; `value_source`: `system_assigned`; `used_by`: `price_books`, `subscription_pricing`, `subscription_change_orders` |
| `price_tier` | VARCHAR2 | 45 |  |  |  |  | `purpose`: `quantity_based_price_tier` |
| `price_tier_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `price_tier_id`; `value_source`: `system_assigned` |
| `price_value` | NUMBER |  | 39 | 0 |  |  | `purpose`: `tier_charge_amount`; `value_kind`: `per_item_or_per_tier_by_pricing_option` |
| `pricing_option` | VARCHAR2 | 200 |  |  |  |  | `purpose`: `tier_pricing_option`; `options`: `rate`, `fixed_amount`; `option_meanings`: `rate:amount_is_quantity_times_rate`, `fixed_amount:charge_independent_of_quantity` |

## Foreign keys

| Key | Columns | References table | Referenced columns |
|---|---|---|---|
| `price_tiers_price_plans_fk` | `price_plan_id` | `price_plans` | `price_plan_id` |

## Suspected source errors

> [!SOURCE-ERROR] `quantity_column_described_as_currency_amount` (on `maximum_quantity`)
<!-- source-error: connect:price_tiers | maximum_quantity | quantity_column_described_as_currency_amount -->

> [!SOURCE-ERROR] `quantity_column_described_as_currency_amount` (on `minimum_quantity`)
<!-- source-error: connect:price_tiers | minimum_quantity | quantity_column_described_as_currency_amount -->
