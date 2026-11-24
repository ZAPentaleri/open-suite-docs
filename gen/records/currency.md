# Currency (`currency`)
<!-- id: record:currency -->

| Fact | Value |
|---|---|
| Internal ID | `currency` |
| Category | list |
| Client scripts | no |
| Server scripts | yes |
| Tags | `custom_fields_supported` |

## Record facts

- `script_types`: `server`
- `full_scripting_requires`: `multiple_currencies`
- `supported_without_feature`: `load_only`
- `rule`: `currencyprecision_always_number_in_script`
- `not_supported`: `create`, `edit`, `delete`, `search`, `lookupfields` (when `multiple_currencies_disabled`)
- `difference`: `currencyprecision_scriptable_even_when_read_only_in_ui`
- `configured_by`: `allowcurrencyprecisionchange_preference` [`currencyprecision`]

## Body fields

| Field ID | Type | Label | Required | submitField | Read-only | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `currencyprecision` | select | Currency Precision | no | no | yes |  | `purpose`: `currency_precision`; `allowed_values`: `0`, `2`; `effect`: `used_in_inventory_costing`; `caveat`: `reports_round_to_base_currency_precision` |
| `displaysymbol` | text | Symbol | no | no |  |  | `purpose`: `display_symbol`; `value_examples`: `$ USD`, `$CAD`; `caveat`: `include_space_to_separate_symbol` |
| `exchangerate` | currency2 | Default Exchange Rate ( to root subsidiary currency ) | yes | no |  |  | `purpose`: `default_exchange_rate`; `value_kind`: `base_amount_divided_by_foreign_amount`; `effect`: `basis_for_currency_exchange_rates`; `effect`: `basis_for_consolidated_rates` (when `oneworld`); `value_examples`: `1.02_cad_per_usd_gives_1.02` |
| `includeinfxrateupdates` | checkbox | Automatic Update | no | no |  |  | `purpose`: `include_in_daily_rate_updates` |
| `isbasecurrency` | checkbox | Is Base Currency | no | no |  |  | `purpose`: `base_currency_flag`; `cannot`: `change_after_foreign_currency_transactions` |
| `isinactive` | checkbox | Inactive | no | no |  |  | `purpose`: `inactivate_currency`; `cannot`: `inactivate_with_open_transactions` |
| `name` | text | Name | yes | no |  |  | `purpose`: `currency_name`; `constraint`: `unique`; `displayed_on`: `currency_fields`; `recommendation`: `include_country_in_name`; `value_examples`: `Mexican peso` |
| `overridecurrencyformat` | checkbox | Override Currency Format | no | no |  |  | `purpose`: `override_currency_format` |
| `symbol` | text | ISO Code | yes | no |  |  | `purpose`: `iso_currency_code`; `format`: `three_letter_iso_4217`; `value_examples`: `PHP`, `UYU`, `MXN` |
| `symbolplacement` | select | Symbol Placement | no | no |  |  | `purpose`: `symbol_placement`; `options`: `before_number`, `after_number`; `displayed_on`: `transaction_records`; `not_displayed_on`: `reports` |
| `currencyformatsample` | text | Format Sample | no | no |  |  |  |
| `externalid` | text | ExternalId | no | no |  |  |  |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `exchangerate` | float | Exchange Rate |  |
| `externalid` | select | External ID |  |
| `externalidstring` | text | External ID (Text) |  |
| `formuladate` | date | Formula (Date) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulatext` | text | Formula (Text) |  |
| `internalid` | select | Internal ID |  |
| `internalidnumber` | integer | Internal ID (Number) |  |
| `isinactive` | checkbox | Inactive |  |
| `name` | text | Name |  |
| `symbol` | text | Symbol |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `exchangerate` | float | Exchange Rate |  |
| `externalid` | select | External ID |  |
| `formulacurrency` | currency | Formula (Currency) |  |
| `formuladate` | date | Formula (Date) |  |
| `formuladatetime` | datetime | Formula (Date/Time) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulapercent` | percent | Formula (Percent) |  |
| `formulatext` | text | Formula (Text) |  |
| `internalid` | select | Internal ID |  |
| `isinactive` | checkbox | Inactive |  |
| `name` | text | Name |  |
| `symbol` | text | Symbol |  |
