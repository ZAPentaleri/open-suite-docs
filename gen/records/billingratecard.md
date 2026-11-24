# Billing Rate Card (`billingratecard`)
<!-- id: record:billingratecard -->

| Fact | Value |
|---|---|
| Internal ID | `billingratecard` |
| Category | list |
| Client scripts | yes |
| Server scripts | yes |
| Searchable | yes |
| Tags | `custom_fields_supported`, `no_copy`, `no_transform`, `supports_create`, `supports_delete`, `supports_read`, `supports_search`, `supports_update` |

## Record facts

- `record_macros`: `modifyPriceByPercent:recalculate_price_list`
- `macro_params`: `percent:required_positive_or_negative`
- `ui_location`: `recalculate_button`
- `value_examples`: `increase_rate_card_1_by_10_percent`
- `script_types`: `client`, `server`
- `requires_feature`: `Billing Rate Cards`, `Per-Employee Billing Rates`, `Charge-Based Billing` [`real_name`]
- `script_functions`: `read`, `create`, `edit`, `delete`, `search`
- `mutually_exclusive_sublists`: `ratecardpricing:single_currency`, `ratecardpricingmulti:multiple_currencies_matrix`
- `record_macros`: `modifyPriceByPercent`
- `doc_inconsistency`: `script_type_sentence_names_billing_account`
- `purpose`: `billing_rates_for_groups_of_billing_classes`
- `used_by`: `time_based_charge_rules_on_charge_based_projects`
- `configured_by`: `enable_features_employees_subtab` [`billing_rate_cards`]
- `ui_location`: `setup_accounting_billing_rate_cards_new`
- `effect`: `single_currency_prices_become_column_with_multicurrency`
- `not_supported`: `copy`, `transform`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `isinactive` | checkbox | Inactive | no | yes |  | `purpose`: `inactivate_record`; `can`: `reactivate_any_time`; `doc_inconsistency`: `says_removes_all_references` |
| `name` | text | Name | yes | yes |  | `purpose`: `record_name`; `displayed_on`: `lists` |
| `externalid` | text | ExternalId | no | no |  |  |

## Sublist `ratecardpricing` (Pricing)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `billingclassid` | integer | Billing Class | no |  |  |
| `price` | poscurrency | Price | no |  |  |
| `saleunit` | select | Sale Unit | no |  |  |
| `unitstype` | select | Units Type | no |  |  |

## Sublist `ratecardpricingmulti` (Pricing)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `billingclassid` | integer | Billing Class | no |  |  |
| `saleunit` | select | Sale Unit | no |  |  |
| `unitstype` | select | Units Type | no |  |  |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `billingclass` | select | Billing Class |  |
| `currency` | select | Currency |  |
| `customer` | select | Customer |  |
| `externalid` | select | External ID |  |
| `externalidstring` | text | External ID (Text) |  |
| `id` | select | Internal ID |  |
| `internalidnumber` | integer | Internal ID (Number) |  |
| `isinactive` | checkbox | Inactive |  |
| `laborbasedprojectrevenuerule` | select | Labor Based Project Revenue Rule |  |
| `name` | text | Name |  |
| `price` | currency | Price |  |
| `saleunit` | select | Sale Unit |  |
| `timebasedchargerule` | select | Time-Based Charge Rule |  |
| `unitstype` | select | Units Type |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `billingclass` | text | Billing Class |  |
| `currency` | text | Currency |  |
| `customer` | text | Customer |  |
| `externalid` | select | External ID |  |
| `id` | select | Internal ID |  |
| `isinactive` | checkbox | Inactive |  |
| `laborbasedprojectrevenuerule` | text | Labor Based Project Revenue Rule |  |
| `name` | text | Name |  |
| `price` | currency | Price |  |
| `saleunit` | text | Sale Unit |  |
| `timebasedchargerule` | text | Time-Based Charge Rule |  |
| `unitstype` | text | Units Type |  |

## Search joins

| Join ID | Label | Joined record |
|---|---|---|
| `billingclass` | Billing Class | Billing Class |
| `customer` | Customer | Customer |

## Suspected source errors

> [!SOURCE-ERROR] `script_type_sentence_names_billing_account`
<!-- source-error: record:billingratecard | - | script_type_sentence_names_billing_account -->

> [!SOURCE-ERROR] `says_removes_all_references` (on `isinactive`)
<!-- source-error: record:billingratecard | isinactive | says_removes_all_references -->
