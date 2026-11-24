# Currency Rate (`currencyrate`)
<!-- id: record:currencyrate -->

| Fact | Value |
|---|---|
| Internal ID | `currencyrate` |
| Category | list |
| Client scripts | yes |
| Server scripts | yes |
| Searchable | yes |
| Tags | `supports_copy`, `supports_create`, `supports_read`, `supports_search` |

## Record facts

- `script_types`: `client`, `server`
- `requires_feature`: `Multiple Currencies` [`real_name`]
- `script_functions`: `create`, `read`, `copy`, `search`
- `not_scriptable_when`: `multiple_currencies_disabled`
- `purpose`: `default_exchange_rates_base_to_foreign_currency`
- `applies_to`: `all_subsidiaries_sharing_base_currency`
- `ui_location`: `lists_accounting_currency_exchange_rates_new`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `basecurrency` | select | Base Currency | yes | no |  | `purpose`: `base_currency_of_rate`; `allowed_values`: `accessible_subsidiary_base_currencies` (when `oneworld`) |
| `currencyratetype` | select | Exchange Rate Type | yes | no |  | `purpose`: `currency_rate_type`; `shown_only_when`: `currency_exchange_rate_types_feature` |
| `effectivedate` | date | Effective Date | no | no |  | `purpose`: `rate_effective_date` |
| `exchangerate` | currency2 | Exchange Rate | no | no |  | `purpose`: `default_exchange_rate`; `value_kind`: `base_amount_divided_by_foreign_amount`; `effect`: `basis_for_currency_exchange_rates`; `effect`: `basis_for_consolidated_rates` (when `oneworld`); `value_examples`: `1.02_cad_per_usd_gives_1.02` |
| `transactioncurrency` | select | Currency | yes | no |  | `purpose`: `foreign_currency_of_rate`; `allowed_values_listed_in`: `currency_records`; `value_examples`: `euro_rate_against_usd_base` |
| `externalid` | text | External ID | no | no |  |  |
| `fxsourcemethod` | text | Method | no | no |  |  |
| `lastmodifieddate` | datetime | Last Modified Date | no | no |  |  |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `internalid` | select | Internal ID |  |
| `internalidnumber` | integer | Internal ID (Number) |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `internalid` | select | Internal ID |  |
