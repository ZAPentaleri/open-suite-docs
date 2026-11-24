# Fair Value Price (`fairvalueprice`)
<!-- id: record:fairvalueprice -->

| Fact | Value |
|---|---|
| Internal ID | `fairvalueprice` |
| Tags | `custom_fields_supported` |

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `currency` | select | Currency | yes | no |  | `purpose`: `fair_value_currency`; `effect`: `other_currencies_via_transaction_rate` |
| `enddate` | date | End Date | no | no |  | `purpose`: `fair_value_end_date`; `requires`: `startdate` (when `set`); `constraint`: `after_start_date` |
| `fairvalue` | float | Base Fair Value | yes | no |  | `purpose`: `base_fair_value_per_unit`; `use_when`: `fair_value_range_checking` |
| `fairvalueformula` | select | Fair Value Formula | yes | no |  | `purpose`: `fair_value_formula`; `default_behavior`: `item_revenue_category_formula`; `allowed_values_listed_in`: `fair_value_formula_records`; `ui_effect`: `new_icon_opens_formula_popup` |
| `fairvaluerangepolicy` | select | Fair Value Range Checking Policy | no | no |  | `purpose`: `fair_value_range_policy`; `options`: `Boundary`, `Fair Value`, `High`, `Low`; `default_behavior`: `item_revenue_category_policy`; `option_meanings`: `boundary:inside_range_actual_outside_nearest_bound`, `fair_value:outside_range_uses_fair_value`, `high:outside_range_uses_high`, `low:outside_range_uses_low` |
| `highvalue` | float | High Value | no | no |  | `purpose`: `high_range_value`; `effect`: `highvaluepercent_calculated`; `default_behavior`: `from_highvaluepercent` (when `blank`) |
| `highvaluepercent` | percent | High Value Percent | no | no |  | `purpose`: `high_range_percent`; `format`: `percent_without_sign`; `effect`: `highvalue_calculated` |
| `isvsoeprice` | checkbox | Is VSOE Price? | no | no |  | `purpose`: `fair_value_is_vsoe`; `false_means`: `fair_value_is_estimate`; `true_means`: `fair_value_based_on_vsoe` |
| `item` | select | Item | no | no |  | `purpose`: `fair_value_item`; `not_required_when`: `itemrevenuecategory_set`; `effect`: `fills_itemrevenuecategory_from_item` |
| `itemrevenuecategory` | select | Item Revenue Category | no | no |  | `purpose`: `item_revenue_category`; `effect`: `item_may_be_left_blank` |
| `lowvalue` | float | Low Value | no | no |  | `purpose`: `low_range_value`; `effect`: `lowvaluepercent_calculated`; `default_behavior`: `from_lowvaluepercent` (when `blank`) |
| `lowvaluepercent` | percent | Low Value Percent | no | no |  | `purpose`: `low_range_percent`; `format`: `percent_without_sign`; `effect`: `lowvalue_calculated` |
| `startdate` | date | Start Date | no | no |  | `purpose`: `fair_value_start_date`; `constraint`: `before_end_date` |
| `units` | text | Units | no | no |  | `purpose`: `units`; `effect`: `formulas_use_alternate_quantity` |
| `unitstype` | select | Units Type | no | no |  | `purpose`: `units_type` |
| `customform` | select | Custom Form | no | no |  |  |
| `externalid` | text | ExternalId | no | no |  |  |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `currency` | select | Currency |  |
| `enddate` | date | End Date |  |
| `externalid` | select | External ID |  |
| `externalidstring` | text | External ID (Text) |  |
| `fairvalue` | float | Base Fair Value |  |
| `fairvalueformula` | select | Fair Value Formula |  |
| `fairvaluerangepolicy` | select | Fair Value Range Checking Policy |  |
| `highvalue` | float | High Value |  |
| `highvaluepercent` | percent | High Value Percent |  |
| `internalid` | select | Internal ID |  |
| `internalidnumber` | integer | Internal ID (Number) |  |
| `isvsoeprice` | checkbox | Is VSOE? |  |
| `item` | select | Item |  |
| `itemrevenuecategory` | select | Item Revenue Category |  |
| `lowvalue` | float | Low Value |  |
| `lowvaluepercent` | percent | Low Value Percent |  |
| `startdate` | date | Start Date |  |
| `unitstype` | select | Units Type |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `currency` | select | Currency |  |
| `enddate` | date | End Date |  |
| `externalid` | select | External ID |  |
| `fairvalue` | float | Base Fair Value |  |
| `fairvalueformula` | select | Fair Value Formula |  |
| `fairvaluerangepolicy` | select | Fair Value Range Checking Policy |  |
| `highvalue` | float | High Value |  |
| `highvaluepercent` | percent | High Value Percent |  |
| `internalid` | select | Internal ID |  |
| `isvsoeprice` | checkbox | Is VSOE? |  |
| `item` | select | Item |  |
| `itemrevenuecategory` | select | Item Revenue Category |  |
| `lowvalue` | float | Low Value |  |
| `lowvaluepercent` | percent | Low Value Percent |  |
| `startdate` | date | Start Date |  |
| `units` | select | Units |  |
| `unitstype` | select | Units Type |  |
