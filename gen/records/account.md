# Account (`account`)
<!-- id: record:account -->

| Fact | Value |
|---|---|
| Internal ID | `account` |
| Category | list |
| Client scripts | no |
| Server scripts | yes |
| Searchable | yes |
| Tags | `custom_fields_supported`, `scripting_full` |

## Record facts

- `script_types`: `server`
- `doc_inconsistency`: `script_type_sentence_names_accounting_book`
- `script_functions`: `create`, `update`, `copy`, `delete`, `search`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `acctname` | text | Name | yes | no |  | `purpose`: `account_name`; `max_length`: `31` characters; `displayed_on`: `reports`; `configured_by`: `use_account_numbers_preference` [`shows_number_field`]; `ui_effect`: `number_field_shown_when_use_account_numbers` |
| `acctnumber` | text | Number | yes | no |  | `purpose`: `account_number`; `format`: `alphanumeric`; `max_length`: `60` characters |
| `accttype` | select | Type | yes | no |  | `purpose`: `gaap_account_type`; `effect`: `sets_default_financial_statement_section` |
| `billableexpensesacct` | select | Track Billable Expenses in | no | no |  | `purpose`: `revenue_account_for_billable_expenses`; `shown_only_when`: `account_type_like_cogs_or_expense`; `allowed_values`: `revenue_accounts`, `income_accounts` |
| `cashflowrate` | select | Cash Flow Rate Type | no | no |  | `purpose`: `cash_flow_statement_translation_rate_type`; `options`: `Current`, `Average`, `Historical`; `option_meanings`: `current:rate_effective_at_period_end`, `average:weighted_average_of_period_transactions`, `historical:weighted_average_historical_accounts`; `default_value`: `Average` |
| `category1099misc` | select | 1099-MISC Category | no | no |  | `purpose`: `1099_misc_box_for_cash_basis_vendor_amounts`; `caveat`: `not_reported_until_category_threshold_exceeded`; `allowed_values_listed_in`: `accounting_list_1099_misc_category`; `configured_by`: `1099_misc_category_thresholds` |
| `class` | select | Restrict To Class | no | no |  | `purpose`: `restrict_account_by_class`; `effect`: `account_selectable_only_by_roles_with_access`; `ui_effect`: `hidden_from_account_lists_for_restricted_roles`; `allowed_values_listed_in`: `class_records`; `value_examples`: `class_1_restricts_to_class_1_roles` |
| `currency` | select | Currency | no | no |  | `purpose`: `currency_of_account_funds` |
| `deferralacct` | select | Deferral Account | no | no |  | `purpose`: `deferred_revenue_or_expense_account`; `constraint`: `not_deferred_revenue_clearing_account`; `shown_only_when`: `account_type_allows_deferral` |
| `department` | select | Restrict To Department | no | no |  | `purpose`: `restrict_account_by_department`; `effect`: `account_selectable_only_by_roles_with_access`; `ui_effect`: `hidden_from_account_lists_for_restricted_roles`; `allowed_values_listed_in`: `department_records` |
| `description` | text | Description | no | no |  | `purpose`: `account_description`; `max_length`: `255` characters; `format`: `alphanumeric` |
| `eliminate` | checkbox | Eliminate Intercompany Transactions | no | no |  | `purpose`: `marks_intercompany_account`; `constraint`: `intercompany_ar_ap_only_for_elimination_amounts` |
| `externalid` | text | ExternalId | no | no |  | `purpose`: `external_id` |
| `generalrate` | select | General Rate Type | no | no |  | `purpose`: `general_translation_rate_type`; `options`: `Current`, `Average`, `Historical`; `option_meanings`: `current:rate_effective_at_period_end`, `average:weighted_average_of_period_transactions`, `historical:weighted_average_historical_accounts`; `default_value`: `Current` (when `balance_sheet_account`); `default_value`: `Average` (when `income_statement_account`); `use_when`: `historical_for_equity_accounts` |
| `includechildren` | checkbox | Include Children | no | no |  | `purpose`: `share_account_with_child_subsidiaries` |
| `inventory` | checkbox | Inventory | no | no |  | `purpose`: `include_balance_in_inventory_kpi`; `applies_when`: `other_current_asset_account` |
| `isinactive` | checkbox | Inactive | no | no |  | `purpose`: `inactivate_account`; `ui_effect`: `hidden_from_lists_on_transactions_and_records` |
| `legalname` | textarea | Legal Name | no | no |  | `purpose`: `legal_name_of_account`; `max_length`: `400` characters; `accepts_forms`: `special_characters`, `double_byte_characters`; `shown_only_when`: `use_legal_name_in_account_preference`; `supported_in`: `financial_reports`, `advanced_search`, `suitescript`, `odbc`; `effect`: `changes_audited_in_system_notes`; `use_when`: `legal_name_required_by_law` |
| `location` | select | Restrict To Location | no | no |  | `purpose`: `restrict_account_by_location`; `effect`: `account_selectable_only_by_roles_with_access`; `ui_effect`: `hidden_from_account_lists_for_restricted_roles`; `allowed_values_listed_in`: `location_records` |
| `openingbalance` | text |  | no | no |  | `purpose`: `opening_balance` |
| `parent` | select | Subaccount of | no | no |  | `purpose`: `parent_account_of_subaccount` |
| `restricttoaccountingbook` | select | Restrict to Accounting Book | no | no |  | `purpose`: `restrict_account_to_accounting_books`; `requires_feature`: `Multi-Book Accounting` [`real_name`]; `default_behavior`: `available_in_all_books` (when `blank`); `allowed_values`: `active_books`, `pending_books`, `primary_book`; `caveat`: `inactive_books_listed_only_if_already_selected`; `effect`: `secondary_only_accounts_unavailable_on_book_generic`; `effect`: `primary_selected_unavailable_on_secondary_specific`; `cannot`: `use_secondary_restricted_account_for_budgets`; `not_supported_for`: `statistical_accounts`; `can`: `add_books_after_transactions_posted`; `displayed_on`: `reports`; `effect`: `unmapped_generic_posts_same_account_in_secondary`; `applies_to`: `amortization_and_revrec_journal_accounts`, `allocation_schedule_accounts`, `account_mapping_accounts`; `caveat`: `every_book_and_account_page_respects_restriction` |
| `revalue` | checkbox | Revalue Open Balance for Foreign Currency Transactions | no | no |  | `purpose`: `include_in_open_balance_currency_revaluation` |
| `subsidiary` | select | Subsidiaries | yes | no |  | `purpose`: `restrict_account_to_subsidiaries`; `constraint`: `bank_and_credit_card_accounts_single_subsidiary`; `effect`: `selectable_only_with_matching_subsidiary`; `option_meanings`: `root_subsidiary:all_subsidiaries_access`, `none:no_posting_reporting_only`; `option_meanings`: `selected:only_records_of_those_subsidiaries` |
| `trandate` | date | Date | no | no |  | `purpose`: `opening_balance_date` |
| `unit` | select | Default Unit | yes | no |  | `purpose`: `base_unit_of_unit_type`; `cannot`: `change_default_unit` |
| `unitstype` | select | Unit Type | no | no |  | `purpose`: `unit_type_of_statistical_account` |
| `accttype2` | text | Type | no | no |  |  |

## Sublist `localizations` (Localization)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `accountingcontext` | select | Accounting Context | no |  |  |
| `acctname` | text | Name | no |  |  |
| `acctnumber` | text | Number | no |  |  |
| `legalname` | text | Legal Name | no |  |  |
| `locale` | select | Language | no |  |  |

## Tabs

| Tab ID | Label |
|---|---|
| `history` | System Notes |
| `localization` | Localization |
| `translation` | Translation |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `accountingcontext` | select | Accounting Context |  |
| `balance` | currency | Balance |  |
| `cashflowratetype` | select | Cash Flow Rate Type |  |
| `category1099misc` | select | 1099-MISC Category |  |
| `description` | text | Description |  |
| `externalid` | select | External ID |  |
| `externalidstring` | text | External ID (Text) |  |
| `formuladate` | date | Formula (Date) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulatext` | text | Formula (Text) |  |
| `generalratetype` | select | General Rate Type |  |
| `internalid` | select | Internal ID |  |
| `internalidnumber` | integer | Internal ID (Number) |  |
| `isinactive` | checkbox | Inactive |  |
| `legalname` | text | Legal Name |  |
| `locale` | select | Language |  |
| `localizedlegalname` | text | Localized Legal Name |  |
| `localizedname` | text | Localized Name |  |
| `localizednumber` | text | Localized Number |  |
| `name` | text | Name |  |
| `number` | text | Number |  |
| `parent` | select | Sub-Account Of |  |
| `subsidiary` | select | Subsidiary |  |
| `type` | select | Type |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `accountingcontext` | text | Accounting Context |  |
| `balance` | currency | Balance |  |
| `cashflowratetype` | select | Cash Flow Rate Type |  |
| `category1099misc` | select | 1099-MISC Category |  |
| `description` | text | Description |  |
| `externalid` | select | External ID |  |
| `formulacurrency` | currency | Formula (Currency) |  |
| `formuladate` | date | Formula (Date) |  |
| `formuladatetime` | datetime | Formula (Date/Time) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulapercent` | percent | Formula (Percent) |  |
| `formulatext` | text | Formula (Text) |  |
| `generalratetype` | select | General Rate Type |  |
| `internalid` | select | Internal ID |  |
| `isinactive` | checkbox | Inactive |  |
| `legalname` | text | Legal Name |  |
| `locale` | text | Language |  |
| `localizedlegalname` | text | Localized Legal Name |  |
| `localizedname` | text | Localized Name |  |
| `localizednumber` | text | Localized Number |  |
| `name` | text | Name |  |
| `number` | text | Number |  |
| `subsidiary` | select | Subsidiary |  |
| `subsidiarynohierarchy` | select | Subsidiary (no hierarchy) |  |
| `type` | select | Account Type |  |

## Suspected source errors

> [!SOURCE-ERROR] `script_type_sentence_names_accounting_book`
<!-- source-error: record:account | - | script_type_sentence_names_accounting_book -->
