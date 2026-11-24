# Intercompany Allocation Schedule (`intercompallocationschedule`)
<!-- id: record:intercompallocationschedule -->

| Fact | Value |
|---|---|
| Internal ID | `intercompallocationschedule` |
| Category | list |
| Client scripts | yes |
| Server scripts | yes |
| Tags | `custom_fields_supported`, `supports_create`, `supports_delete`, `supports_read`, `supports_update` |

## Record facts

- `script_types`: `client`, `server`
- `requires`: `oneworld`
- `requires_feature`: `Accounting Periods`, `Expense Allocation` [`real_name`]
- `not_scriptable`: `currency_field` (when `dynamic_allocation`)
- `not_scriptable`: `history_subtab_fields`
- `purpose`: `share_one_subsidiary_account_balance_across_subsidiaries`
- `configured_by`: `enable_features_accounting_subtab` [`accounting_periods`, `expense_allocation`]
- `available_only_when`: `statistical_accounting` [`statistical_weight_fields`]
- `effect`: `weight_computed_when_journal_generated` (when `dynamic_allocation`)
- `script_functions`: `create`, `read`, `update`, `delete`
- `not_supported`: `copy`, `search`, `transform`
- `doc_inconsistency`: `update_listed_as_supported_and_unsupported`
- `doc_inconsistency`: `double_negative_on_history_subtab_fields`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `accountingbook` | select | Accounting Book | no | no |  | `purpose`: `accounting_book_of_schedule`; `ui_effect`: `list_empty_until_subsidiary_selected`; `option_meanings`: `book:book_specific_schedule`, `blank:book_generic_schedule`; `requires_feature`: `Multi-Book Accounting` (when `book_specific_schedule`) [`real_name`] |
| `accountsfromsource` | checkbox | Use source/credit account(s) | no | no |  | `purpose`: `use_source_or_credit_account`; `constraint`: `destination_segments_within_source_accounts` |
| `allocationmode` | select | Allocation Mode | no | no |  | `purpose`: `allocation_mode`; `options`: `fixed_rate`, `dynamic` |
| `creditaccount` | select | Credit Account | no | no |  | `purpose`: `credit_holding_account` |
| `creditclass` | select | Credit Class | no | no |  | `purpose`: `class_of_credit_account`; `applies_when`: `credit_account_differs_from_source` |
| `creditdepartment` | select | Credit Department | no | no |  | `purpose`: `department_of_credit_account`; `applies_when`: `credit_account_differs_from_source` |
| `creditentity` | select | Credit Name | no | no |  | `purpose`: `entity_of_offsetting_credit` |
| `creditlocation` | select | Credit Location | no | no |  | `purpose`: `location_of_credit_account` |
| `datebasis` | select | Date Basis | no | no |  | `purpose`: `statistical_weight_date_basis`; `options`: `As of Date`, `Period to Date`, `Quarter to Date`, `Year to Date`; `caveat`: `sums_up_to_day_before_next_date`; `option_meanings`: `as_of_date:from_beginning`, `period_to_date:from_period_start`, `quarter_to_date:from_quarter_start`, `year_to_date:from_year_start` |
| `frequency` | select | Frequency | no | no |  | `purpose`: `allocation_frequency`; `not_required_when`: `end_of_period` [`nextdate`]; `constraint`: `only_run_by_batch_schedules_in_batches`; `caveat`: `relative_to_next_date` |
| `inactive` | checkbox | Inactive | no | no |  | `purpose`: `inactivate_schedule_or_batch`; `true_means`: `schedule_does_not_allocate`; `caveat`: `inactive_batches_not_deleted` |
| `name` | text | Name | yes | no |  | `purpose`: `schedule_or_batch_name` |
| `nextdate` | date | Next Date | no | no |  | `purpose`: `next_allocation_date`; `effect`: `end_of_weight_timeline` |
| `remindforever` | radio | Remind Forever | no | no |  | `purpose`: `reallocation_repeat_mode`; `options`: `Remind Forever`, `Number Remaining` |
| `sourceintercompanyaccount` | select | Intercompany Account | yes | no |  | `purpose`: `intercompany_clearing_account` |
| `subsequentdate` | date | Subsequent Date | no | no |  | `purpose`: `following_allocation_date` |
| `subsidiary` | select | Subsidiary | yes | no |  | `purpose`: `source_subsidiary`; `can`: `use_shared_vendors_of_subsidiary` |
| `unitlabel` | text | Unit of Measure | no | no |  | `purpose`: `base_unit_of_unit_type`; `value_source`: `system_displayed` |
| `unitstype` | text | Unit Type | no | no |  | `purpose`: `unit_type_of_statistical_account`; `value_source`: `system_displayed` |
| `weightsinpercentage` | checkbox | Values are percentages | no | no |  | `purpose`: `weights_are_percentages` |
| `weightsource` | select | Weight Source | no | no |  | `purpose`: `statistical_account_for_dynamic_weights` |
| `externalid` | text | ExternalId | no | no |  |  |

## Sublist `allocationdestination` (allocationweights)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `account` | select | Account | yes |  |  |
| `balance` | rtext | Balance | no |  |  |
| `class` | select | Class | no |  |  |
| `department` | select | Department | no |  |  |
| `entity` | select | Name | no |  |  |
| `intercompanyaccount` | select | Intercompany Account | yes |  |  |
| `location` | select | Location | no |  |  |
| `subsidiary` | select | Subsidiary | yes |  |  |
| `weight` | currency | Weight | no |  |  |

## Sublist `allocationsource` (Source of the allocation amounts)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `account` | select | Account | no |  |  |
| `class` | select | Class | no |  |  |
| `department` | select | Department | no |  |  |
| `entity` | select | Name | no |  |  |
| `location` | select | Location | no |  |  |

## Suspected source errors

> [!SOURCE-ERROR] `update_listed_as_supported_and_unsupported`
<!-- source-error: record:intercompallocationschedule | - | update_listed_as_supported_and_unsupported -->

> [!SOURCE-ERROR] `double_negative_on_history_subtab_fields`
<!-- source-error: record:intercompallocationschedule | - | double_negative_on_history_subtab_fields -->
