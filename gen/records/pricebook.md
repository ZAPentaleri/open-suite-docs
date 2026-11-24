# Price Book (`pricebook`)
<!-- id: record:pricebook -->

| Fact | Value |
|---|---|
| Internal ID | `pricebook` |
| Category | list |
| Client scripts | yes |
| Server scripts | yes |
| Searchable | yes |
| Tags | `custom_fields_supported`, `scripting_full` |

## Record facts

- `script_types`: `client`, `server`
- `script_functions`: `create`, `update`, `copy`, `delete`, `search`
- `requires_feature`: `SuiteBilling` [`real_name`]
- `purpose`: `subscription_pricing_with_price_plans`
- `configured_by`: `enable_features_transactions_subtab` [`suitebilling`]
- `purpose`: `pricing_for_items_of_subscription_plan`
- `rule`: `every_plan_item_needs_price_plan_before_save`
- `max_per_subscription_plan`: `many` [`e_g_per_currency_or_billing_period`]
- `not_needed_when`: `subscription_with_add_on_lines_only` [`standard_form`]
- `ui_location`: `subscription_plan_price_books_subtab` [`default_subtab_once_plan_saved`]
- `ui_action`: `New Price Book`
- `ui_fields`: `Name`, `Currency`
- `line_subtab`: `Price Book Lines` [`edit_icon_opens_price_plan`]
- `line_columns`: `Interval`, `Start On`, `Included Quantity Multiplier`, `Discount`, `Charge Frequency`, `Repeat Every`, `Prorate By`
- `interval_values`: `Day`, `Week`, `Month`, `Year`
- `rule`: `start_on_is_relative_period_number` [`converted_to_dates_from_subscription_start`]
- `discount_formats`: `percent_with_trailing_percent_sign`, `flat_amount_no_notation` [`optional_default_discount`]
- `charge_frequency_values`: `Weekly`, `Monthly`, `Annually`
- `rule`: `repeat_every_sets_charge_recurrence` [`non_one_time_items`]
- `prorate_by_values`: `Month`, `Day`
- `prorate_by_default`: `Day` [`recurring_items`]
- `effect`: `prorate_by_month_gives_equal_period_charges` [`prorate_by_day_varies_with_days_in_month`]
- `can`: `pick_renewal_plan_and_book_at_renewal`
- `copy_via`: `actions_menu_make_copy` [`e_g_other_currency`]
- `delete_via`: `actions_menu_delete` (when `no_subscriptions_created_from_it`)
- `can`: `limit_editing_of_books_and_plans_by_permission` [`non_admins_still_change_discount_and_quantity`]
- `doc_inconsistency`: `price_plan_instructions_link_points_to_same_page` [`needs_human_review`, `self_reference`]

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `currency` | select | Currency | yes |  |  | `purpose`: `subscription_currency`; `default_behavior`: `customer_primary_currency`; `requires_feature`: `Multi-Currency Customers` (when `choosing_transaction_currency`) [`real_name`] |
| `name` | text | Name | yes |  |  | `purpose`: `subscription_name`; `default_behavior`: `auto_generated_on_save` (when `blank`) |
| `subscriptionplan` | select | Subscription Plan | no |  |  | `purpose`: `subscription_plan`; `ui_effect`: `plus_sign_creates_plan` |
| `externalid` | text | ExternalId | no |  |  |  |
| `id` | integer | Internal ID | no |  |  |  |
| `internalid` | integer | Internal ID | no |  |  |  |

## Sublist `priceinterval` (Price Book Lines)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `chargetype` | select | Subscription Line Type | no |  |  |
| `discount` | ratehighprecision | Discount | no |  |  |
| `frequency` | select | Charge Frequency | yes |  |  |
| `isrequired` | checkbox | Required | no |  |  |
| `item` | select | Item | no |  |  |
| `linenumber` | integer | Line Number | no |  |  |
| `multiplierline` | select | Included Quantity Multiplier | no |  |  |
| `price` | textarea | Price | no |  |  |
| `priceplan` | select | Price Plan | yes |  |  |
| `prorateby` | select | Prorate By | no |  |  |
| `repeatevery` | select | Repeat Every | yes |  |  |
| `startoffsetunit` | select | Interval | yes |  |  |
| `startoffsetvalue` | integer | Start On | yes |  |  |
| `subscriptionplanline` | integer | Subscription Plan Line | no |  |  |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `prorateby` | text | Prorate By |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `prorateby` | text | Prorate By |  |

## Suspected source errors

> [!SOURCE-ERROR] `price_plan_instructions_link_points_to_same_page`
<!-- source-error: record:pricebook | - | price_plan_instructions_link_points_to_same_page -->
