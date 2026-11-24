# Global Account Mapping (`globalaccountmapping`)
<!-- id: record:globalaccountmapping -->

| Fact | Value |
|---|---|
| Internal ID | `globalaccountmapping` |
| Category | list |
| Client scripts | no |
| Server scripts | yes |
| Searchable | yes |
| Tags | `custom_fields_supported`, `scripting_full` |

## Record facts

- `script_types`: `server`
- `user_events`: `beforeLoad`, `beforeSubmit`, `afterSubmit`
- `requires_feature`: `Multi-Book Accounting`, `Chart of Accounts Mapping` [`real_name`]
- `user_events`: `beforeLoad`, `beforeSubmit`, `afterSubmit`
- `purpose`: `secondary_book_posts_to_other_account_than_primary`
- `applies_to`: `transactions_with_user_selected_posting_account`
- `configured_by`: `enable_features_accounting_subtab` [`chart_of_accounts_mapping`]
- `ui_location`: `setup_accounting_global_account_mappings_new`
- `script_functions`: `create`, `update`, `copy`, `delete`, `search`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `accountingbook` | select | Accounting Book | yes | no |  | `purpose`: `accounting_book`; `requires`: `set_before_subsidiary`; `allowed_values`: `active_accounting_books` |
| `class` | select | Class | no | no |  | `purpose`: `mapping_rule_class`; `effect`: `rule_matches_transactions_with_class` |
| `department` | select | Department | no | no |  | `purpose`: `mapping_rule_department`; `effect`: `rule_matches_transactions_with_department` |
| `destinationaccount` | select | Destination Account | yes | no |  | `purpose`: `secondary_book_destination_account`; `special_values`: `-Same-`; `option_meanings`: `same:skip_mapping_for_dimensions` |
| `effectivedate` | date | Effective Date | yes | no |  | `purpose`: `mapping_effective_date` |
| `enddate` | date | End Date | no | no |  | `purpose`: `mapping_end_date`; `default_behavior`: `continues_indefinitely` (when `blank`) |
| `location` | select | Location | no | no |  | `purpose`: `mapping_rule_location`; `effect`: `rule_matches_transactions_with_location` |
| `sourceaccount` | select | Source Account | yes | no |  | `purpose`: `primary_book_source_account`; `allowed_values`: `primary_book_accounts`, `secondary_book_accounts` |
| `subsidiary` | select | Subsidiary | no | no |  | `purpose`: `mapping_rule_subsidiary`; `effect`: `rule_matches_transactions_with_subsidiary`; `requires`: `accountingbook` |
| `customdimension` | select | Custom Dimension | no | no |  |  |
| `customform` | select | Custom Form | no | no |  |  |
| `externalid` | text | ExternalId | no | no |  |  |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `accountingbook` | select | Accounting Book |  |
| `class` | select | Class |  |
| `customdimension` | select | Custom Dimension |  |
| `department` | select | Department |  |
| `destinationaccount` | select | Destination Account |  |
| `effectivedate` | date | Effective Date |  |
| `enddate` | date | End Date |  |
| `externalid` | select | External ID |  |
| `externalidstring` | text | External ID (Text) |  |
| `formuladate` | date | Formula (Date) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulatext` | text | Formula (Text) |  |
| `internalid` | select | Internal ID |  |
| `internalidnumber` | integer | Internal ID (Number) |  |
| `location` | select | Location |  |
| `sourceaccount` | select | Source Account |  |
| `subsidiary` | select | Subsidiary |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `accountingbook` | select | Accounting Book |  |
| `class` | select | Class |  |
| `customdimension` | select | Custom Dimension |  |
| `department` | select | Department |  |
| `destinationaccount` | select | Destination Account |  |
| `effectivedate` | date | Effective Date |  |
| `enddate` | date | End Date |  |
| `externalid` | select | External ID |  |
| `formulacurrency` | currency | Formula (Currency) |  |
| `formuladate` | date | Formula (Date) |  |
| `formuladatetime` | datetime | Formula (Date/Time) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulapercent` | percent | Formula (Percent) |  |
| `formulatext` | text | Formula (Text) |  |
| `internalid` | select | Internal ID |  |
| `location` | select | Location |  |
| `sourceaccount` | select | Source Account |  |
| `subsidiary` | select | Subsidiary |  |

## Search joins

| Join ID | Label | Joined record |
|---|---|---|
| `accountingbook` | Accounting Book | Accounting Book |
| `department` | Department | Department |
| `destinationaccount` | Destination Account | Account |
| `location` | Location | Location |
| `sourceaccount` | Source Account | Account |
| `subsidiary` | Subsidiary | Subsidiary |
