# Consolidated Exchange Rate (`consolidatedexchangerate`)
<!-- id: record:consolidatedexchangerate -->

| Fact | Value |
|---|---|
| Internal ID | `consolidatedexchangerate` |
| Category | list |
| Client scripts | yes |
| Server scripts | yes |
| Searchable | yes |
| Tags | `custom_fields_supported`, `no_create`, `no_delete`, `supports_read`, `supports_search`, `supports_update` |

## Record facts

- `script_types`: `client`, `server`
- `requires_feature`: `Multiple Currencies` [`real_name`]
- `requires_permission`: `currency_full`
- `requires`: `oneworld`
- `editable_fields`: `currentrate`, `averagerate`, `historicalrate`, `externalid`
- `created_by`: `subsidiary_or_accounting_book_creation`
- `rule`: `derived_rates_not_editable`
- `purpose`: `roll_up_amounts_child_to_parent_base_currency_per_period`
- `options`: `current`, `average`, `historical` [`rate_types`]
- `configured_by`: `enable_features_company_subtab` [`multiple_currencies`]
- `requires`: `access_to_from_and_to_subsidiaries` (when `editing_rates`)
- `editable_only_when`: `rate_not_derived_and_period_open` [`isderived`, `isperiodclosed`]
- `cannot`: `edit_rate_1_between_elimination_subsidiary_and_parent` [`iseliminationsubsidiary`]
- `caveat`: `existing_non_1_elimination_rates_stay_editable`
- `script_functions`: `read`, `update`, `search`
- `not_supported`: `create`, `delete`, `transform`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `accountingbook` | text | Accounting Book | no | no |  | `purpose`: `accounting_book_of_rate`; `requires_feature`: `Multi-Book Accounting` [`real_name`] |
| `averagerate` | currency2 | Average Exchange Rate | yes | yes |  | `purpose`: `average_exchange_rate`; `value_source`: `calculated_weighted_average`; `use_when`: `income_statement_and_retained_earnings` |
| `currentrate` | currency2 | Current Exchange Rate | yes | yes |  | `purpose`: `current_exchange_rate`; `value_source`: `rate_effective_at_period_end`; `use_when`: `most_balance_sheet_assets_and_liabilities` |
| `fromcurrency` | text | From Currency | no | no |  | `purpose`: `from_currency` |
| `fromsubsidiary` | text | From Subsidiary | no | no |  | `purpose`: `from_subsidiary` |
| `historicalrate` | currency2 | Historical Exchange Rate | yes | no |  | `purpose`: `historical_exchange_rate`; `value_source`: `calculated_weighted_average`; `use_when`: `equity_accounts` |
| `isderived` | checkbox | Exchange Rate Derived | no | no |  | `purpose`: `indirect_rate_flag`; `true_means`: `rate_between_non_adjacent_subsidiaries`; `value_source`: `system_set` (when `indirect_rate`); `cannot`: `edit` (when `indirect_rate`); `value_examples`: `child_to_grandchild_rate` |
| `iseliminationsubsidiary` | checkbox | Is Elimination Subsidiary | no | no |  | `purpose`: `elimination_subsidiary_flag`; `true_means`: `rates_not_editable` |
| `isperiodclosed` | checkbox | Period Closed | no | no |  | `purpose`: `period_closed_flag` |
| `postingperiod` | select | Posting Period | no | no |  | `purpose`: `posting_period_of_rate` |
| `tocurrency` | text | To Currency | no | no |  | `purpose`: `to_currency` |
| `tosubsidiary` | text | To Subsidiary | no | no |  | `purpose`: `to_subsidiary` |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `accountingbook` | select | Accounting Book |  |
| `externalid` | select | External ID |  |
| `externalidstring` | text | External ID (Text) |  |
| `fromsubsidiary` | select | From Subsidiary |  |
| `internalid` | select | Internal ID |  |
| `internalidnumber` | integer | Internal ID (Number) |  |
| `isderivedrate` | checkbox | Is Derived Rate |  |
| `period` | select | Period |  |
| `periodstartdate` | date | Period Start Date |  |
| `tosubsidiary` | select | To Subsidiary |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `accountingbook` | text | Accounting Book |  |
| `averagerate` | rate | Average |  |
| `closed` | checkbox | Closed |  |
| `currentrate` | rate | Current |  |
| `externalid` | select | External ID |  |
| `fromcurrency` | text | From Currency |  |
| `fromsubsidiary` | text | From Subsidiary |  |
| `historicalrate` | rate | Historical |  |
| `internalid` | select | Internal ID |  |
| `periodenddate` | date | Period End Date |  |
| `periodname` | text | Period |  |
| `periodstartdate` | date | Period Start Date |  |
| `tocurrency` | text | To Currency |  |
| `tosubsidiary` | text | To Subsidiary |  |
