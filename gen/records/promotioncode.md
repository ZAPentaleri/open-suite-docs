# Promotion (`promotioncode`)
<!-- id: record:promotioncode -->

| Fact | Value |
|---|---|
| Internal ID | `promotioncode` |
| Category | marketing |
| Client scripts | yes |
| Server scripts | yes |
| Searchable | yes |
| Tags | `custom_fields_supported`, `scripting_full` |

## Record facts

- `script_types`: `client`, `server`
- `script_functions`: `create`, `update`, `copy`, `delete`, `search`
- `allowed_values`: `percent`, `flat` [`discounttype`]
- `rule`: `free_gift_not_auto_added_or_removed`
- `column_alias`: `discount_amount:discountrate`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `applydiscountto` | select | Apply Discount To | no | no |  | `purpose`: `uses_per_customer`; `options`: `One time only`, `Unlimited amount of times`; `caveat`: `usage_limits_apply_to_multi_use_codes_only` |
| `code` | text | Coupon Code | yes | no |  | `purpose`: `coupon_code`; `options`: `multiple_use`, `single_use`; `constraint`: `single_use_one_transaction_one_customer` |
| `codepattern` | text | Code Pattern | no | no |  | `purpose`: `coupon_code_pattern`; `accepts_forms`: `[A.#]_random_letters`, `[N.#]_random_numbers`, `[AN.#]_random_alphanumerics`, `literal_characters`, `dashes`; `constraint`: `excludes_1_0_I_L_O`; `value_examples`: `ABC-[AN.3]-[A.3]-[N.3]`, `ABC-J3N-FLN-647`; `recommendation`: `enough_variables_for_unique_codes` |
| `couponcodefile` | file | Import File | no | no |  | `purpose`: `single_use_codes_source`; `options`: `csv_file_import`, `code_pattern_and_count` |
| `customform` | select | Custom Form | yes | no |  | `purpose`: `custom_form` |
| `description` | textarea | Description | no | no |  | `purpose`: `promotion_description` |
| `discount` | select | Discount | no | no |  | `purpose`: `discount_item`; `effect`: `discount_posts_to_item_account`; `caveat`: `no_effect_on_inventory_valuation`; `allowed_values_listed_in`: `discount_items` |
| `discounttype` | radio | % | no | no |  | `purpose`: `discount_type`; `options`: `%`, `Flat` |
| `displaylinediscounts` | checkbox | Display line discounts | no | no |  | `purpose`: `display_line_discounts`; `true_means`: `discount_line_under_qualifying_items`; `false_means`: `discount_in_transaction_total`; `requires`: `percentage_discount` (when `web_store_strikeout_pricing`); `requires`: `click_calculate_before_save` (when `line_discount_promotion`) |
| `enddate` | date | End Date | no | no |  | `purpose`: `promotion_end_date` |
| `excludeitems` | checkbox | Exclude Items | no | no |  | `purpose`: `exclude_listed_items`; `true_means`: `applies_to_all_but_listed`; `false_means`: `applies_only_to_listed` |
| `freeshipmethod` | select | Free Shipping Method | no | no |  | `purpose`: `free_ship_method` |
| `isinactive` | checkbox | Inactive | no | no |  | `purpose`: `inactivate_promotion`; `cannot`: `apply_inactive_promotion` |
| `ispublic` | checkbox | Available to all Customers | no | no |  | `purpose`: `public_promotion`; `false_means`: `only_customers_of_listed_partners` |
| `location` | select | Location | no | no |  | `purpose`: `eligible_locations`; `value_kind`: `multi_select`; `applies_when`: `specific_locations_option` |
| `name` | text | Name | no | no |  | `purpose`: `promotion_name`; `caveat`: `internal_and_reporting_only`; `comparable_to`: `coupon_code_on_codes_subtab` |
| `numbertogenerate` | integer | Codes to Generate | no | no |  | `purpose`: `number_of_codes_to_generate` |
| `rate` | rate | Rate | no | no |  | `purpose`: `discount_rate`; `requires`: `discounttype` |
| `startdate` | date | Start Date | no | no |  | `purpose`: `promotion_start_date` |
| `usetype` | select | Number of Uses | no | no |  | `purpose`: `coupon_use_type`; `options`: `multiple_use`, `single_use`; `constraint`: `single_use_one_transaction_one_customer` |
| `externalid` | text | ExternalId | no | no |  |  |

## Tabs

| Tab ID | Label |
|---|---|
| `campaign` | Campaigns |
| `code` | Codes |
| `item` | Items |
| `minimumorderamount` | Minimum Order Amount |
| `partner` | Partners |
| `workflow` | Workflow |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `applydiscountto` | select | Apply Discount To |  |
| `code` | text | Code |  |
| `description` | text | Description |  |
| `discount` | select | Discount |  |
| `discountamount` | currency | Discount Amount |  |
| `enddate` | date | End Date |  |
| `externalid` | select | External ID |  |
| `externalidstring` | text | External ID (Text) |  |
| `formuladate` | date | Formula (Date) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulatext` | text | Formula (Text) |  |
| `freeshipitem` | select | Free Shipping Method |  |
| `internalid` | select | Internal ID |  |
| `internalidnumber` | integer | Internal ID (Number) |  |
| `isinactive` | checkbox | Inactive |  |
| `ispublic` | checkbox | Public |  |
| `item` | select | Item |  |
| `location` | select | Location |  |
| `name` | text | Promotion Name |  |
| `partner` | select | Partner |  |
| `startdate` | date | Start Date |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `code` | text | Code |  |
| `description` | text | Description |  |
| `discount` | text | Discount |  |
| `discountamount` | currency | Discount Amount |  |
| `enddate` | date | End Date |  |
| `externalid` | select | External ID |  |
| `formulacurrency` | currency | Formula (Currency) |  |
| `formuladate` | date | Formula (Date) |  |
| `formuladatetime` | datetime | Formula (Date/Time) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulapercent` | percent | Formula (Percent) |  |
| `formulatext` | text | Formula (Text) |  |
| `internalid` | select | Internal ID |  |
| `isinactive` | checkbox | Inactive |  |
| `ispublic` | checkbox | Available to all Customers |  |
| `location` | multiselect | Locations |  |
| `name` | text | Name |  |
| `startdate` | date | Start Date |  |
