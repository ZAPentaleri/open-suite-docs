# Price Plan (`priceplan`)
<!-- id: record:priceplan -->

| Fact | Value |
|---|---|
| Internal ID | `priceplan` |
| Category | list |
| Client scripts | yes |
| Server scripts | yes |
| Searchable | yes |
| Tags | `custom_fields_supported`, `scripting_full` |

## Record facts

- `script_types`: `client`, `server`
- `script_functions`: `create`, `update`, `copy`, `delete`, `search`
- `requires_feature`: `SuiteBilling` [`real_name`]
- `purpose`: `subscription_pricing_with_price_books`
- `configured_by`: `enable_features_transactions_subtab` [`suitebilling`]
- `value_examples`: `tiered_price`
- `time_based_pricing`: `intervals_change_price_quantity_or_discount` [`free_trials`, `ramp_pricing`, `introductory_pricing`]
- `requires_feature`: `Time-Based Pricing` [`enabled_in_suitebilling_features`]
- `related_suiteapp`: `SuiteBilling Enhanced UI`
- `former_name`: `Time-Based Pricing SuiteApp` [`suitebilling_enhanced_ui`]
- `prerequisites`: `subscription_plan`, `price_book`, `price_plan` [`before_intervals`]
- `ui_action`: `Add Interval to All` [`adds_interval_to_every_item`]
- `technique`: `make_copy_of_item_line_to_add_interval_for_one_item` [`keeps_tiers_and_field_values`]
- `rule`: `one_time_items_have_single_interval`
- `effect`: `identical_adjacent_intervals_merged`
- `ui_effect`: `columns_interval_start_on_shown_after_save`
- `can`: `delete_interval_used_by_subscription` (when `item_in_draft_status`)
- `ui_action`: `Remove` [`deletes_interval`]
- `pass_through`: `change_order_between_intervals_updates_later_intervals` [`unless_later_interval_changes_same_field`]

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `currency` | select | Currency | no |  |  | `purpose`: `subscription_currency`; `default_behavior`: `customer_primary_currency`; `requires_feature`: `Multi-Currency Customers` (when `choosing_transaction_currency`) [`real_name`] |
| `externalid` | text | ExternalId | no |  |  |  |
| `id` | integer | Internal ID | no |  |  |  |
| `internalid` | integer | Internal ID | no |  |  |  |
| `priceplantype` | select | Type | yes |  |  |  |

## Sublist `pricetiers` (Price Plan Price Tiers)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `fromval` | integer | Above Quantity | yes |  |  |
| `lineid` | integer | Line Id | no |  |  |
| `maxamount` | currency2 | Maximum Amount | no |  |  |
| `minamount` | currency2 | Minimum Amount | no |  |  |
| `priceplan` | select | Price Plan | no |  |  |
| `pricetier` | text | Tiers | no |  |  |
| `pricingoption` | select | Pricing Option | yes |  |  |
| `value` | rate | Value | yes |  |  |
