# Budget Exchange Rate (`budgetexchangerate`)
<!-- id: record:budgetexchangerate -->

| Fact | Value |
|---|---|
| Internal ID | `budgetexchangerate` |
| Category | list |
| Client scripts | yes |
| Server scripts | yes |
| Searchable | yes |
| Tags | `supports_read`, `supports_search`, `supports_update` |

## Record facts

- `requires_feature`: `Multiple Currencies` [`real_name`]
- `requires_feature`: `Multiple Budgets` [`real_name`]
- `requires_permission`: `currency_full`
- `rate_kinds`: `direct:editable`, `indirect:system_calculated`
- `requires`: `access_to_from_and_to_subsidiaries` [`edit`]
- `purpose`: `child_to_parent_subsidiary_rates_for_budgeting`
- `ui_location`: `lists_accounting_budget_exchange_rates`
- `value_source`: `initially_from_consolidated_exchange_rates` (when `oneworld`)
- `options`: `average`, `current`, `historical` [`rate_types`]
- `configured_by`: `enable_features_company_subtab` [`multiple_currencies`]
- `editable_only_when`: `rate_not_derived_and_period_open` [`isderived`, `isperiodclosed`]
- `cannot`: `edit_rate_1_between_elimination_subsidiary_and_parent` [`iseliminationsubsidiary`]
- `caveat`: `existing_non_1_elimination_rates_stay_editable`
- `effect`: `rates_created_and_deleted_with_subsidiaries_or_books`
- `script_functions`: `read`, `update`, `search`
- `not_supported`: `create`, `copy`, `delete`, `transform`
- `allowed_values`: `currentrate`, `averagerate`, `historicalrate` [`editable_fields`]

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `accountingbook` | select | Accounting Book | no |  |  | `purpose`: `accounting_book_of_budget`; `requires_one_of`: `full_multi_book_accounting`, `adjustment_only_books` |
| `averagerate` | currency2 | Average Exchange Rate | yes |  |  | `purpose`: `average_exchange_rate`; `value_source`: `calculated_weighted_average`; `use_when`: `income_statement_and_retained_earnings` |
| `currentrate` | currency2 | Current Exchange Rate | yes |  |  | `purpose`: `current_exchange_rate`; `value_source`: `rate_effective_at_period_end`; `use_when`: `most_balance_sheet_assets_and_liabilities` |
| `fromcurrency` | select | From Currency | no |  |  | `purpose`: `from_currency` |
| `fromsubsidiary` | select | From Subsidiary | no |  |  | `purpose`: `from_subsidiary` |
| `historicalrate` | currency2 | Historical Exchange Rate | yes |  |  | `purpose`: `historical_exchange_rate`; `value_source`: `calculated_weighted_average`; `use_when`: `equity_accounts` |
| `isderived` | checkbox | Exchange Rate Derived | no |  |  | `purpose`: `indirect_rate_flag`; `true_means`: `rate_between_non_adjacent_subsidiaries`; `value_source`: `system_set` (when `indirect_rate`); `cannot`: `edit` (when `indirect_rate`); `value_examples`: `child_to_grandchild_rate` |
| `iseliminationsubsidiary` | checkbox | Is Elimination Subsidiary | no |  |  | `purpose`: `elimination_subsidiary_flag`; `true_means`: `rates_not_editable` |
| `isperiodclosed` | checkbox | Period Closed | no |  |  | `purpose`: `period_closed_flag` |
| `periodstartdate` | date | Period Start Date | no |  |  | `purpose`: `period_start_date` |
| `postingperiod` | select | Posting Period | no |  |  | `purpose`: `posting_period_of_rate` |
| `tocurrency` | select | To Currency | no |  |  | `purpose`: `to_currency` |
| `tosubsidiary` | select | To Subsidiary | no |  |  | `purpose`: `to_subsidiary` |
| `externalid` | text | External ID | no |  |  |  |
| `id` | integer | Internal ID | no |  |  |  |
| `internalid` | integer | Internal ID | no |  |  |  |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `accountingbook` | select | Accounting Book |  |
| `externalid` | select | External ID |  |
| `externalidstring` | text | External ID (Text) |  |
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
| `fromsubsidiary` | text | From Subsidiary |  |
| `historicalrate` | rate | Historical |  |
| `internalid` | select | Internal ID |  |
| `periodenddate` | date | Period End Date |  |
| `periodname` | text | Period |  |
| `periodstartdate` | date | Period Start Date |  |
| `tosubsidiary` | text | To Subsidiary |  |
