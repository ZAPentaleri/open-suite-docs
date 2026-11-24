# GL Audit Numbering Sequence (`glnumberingsequence`)
<!-- id: record:glnumberingsequence -->

| Fact | Value |
|---|---|
| Internal ID | `glnumberingsequence` |
| Category | transaction |
| Client scripts | yes |
| Server scripts | yes |
| Searchable | yes |
| Tags | `supports_copy`, `supports_create`, `supports_read`, `supports_search`, `supports_update` |

## Record facts

- `script_types`: `client`, `server`
- `script_functions`: `read`, `create`, `update`, `copy`, `search`
- `requires_feature`: `GL audit numbering` [`real_name`]
- `requires_permission`: `manage_accounting_periods`
- `sequence_kinds`: `permanent`, `repeatable`
- `script_functions`: `read`, `create`, `update`, `copy`, `search`
- `search_only_sublists`: `systemnotes`
- `option_meanings`: `permanent:assigned_number_never_changes`, `repeatable:rerun_to_close_numbering_gaps` [`numberingtype`]
- `effect`: `repeatable_rerun_may_assign_different_numbers`
- `can`: `edit_transaction_details_after_permanent_numbering`
- `ui_location`: `transactions_management_gl_audit_numbering_sequences_new`
- `required_when`: `creating` [`sequencename`, `period`, `subsidiaries`, `numberingtype`, `accountingbook`, `initnum`, `ordertype`, `recurringtype`]
- `supported_in`: `search` [`next_run_field_permanent_recurring_subtab`]

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `accountingbook` | select | Accounting Book | yes |  |  | `purpose`: `accounting_book` |
| `dailyrecurring` | select | Daily Recurring | no |  |  | `purpose`: `recurrence_interval`; `allowed_range`: `1`, `999`; `recommendation`: `one_day_or_24_hours`; `help_text_shared_with`: `glnumberingsequence.hourlyrecurring` |
| `excludezerotransactions` | checkbox | Exclude zero amount transactions | no |  |  | `purpose`: `exclude_zero_amount_transactions`; `true_means`: `zero_line_transactions_not_numbered`; `false_means`: `all_transactions_numbered` |
| `firstrun` | select | First Run Time | no |  |  | `purpose`: `first_run_time`; `value_unit`: `local_time` |
| `hourlyrecurring` | integer | Hourly Recurring | no |  |  | `purpose`: `recurrence_interval`; `allowed_range`: `1`, `999`; `recommendation`: `one_day_or_24_hours`; `help_text_shared_with`: `glnumberingsequence.dailyrecurring` |
| `initnum` | posinteger | Initial Number | yes |  |  | `purpose`: `starting_number` |
| `isinactive` | checkbox | Inactive | no |  |  | `purpose`: `inactivate_record` |
| `mindigits` | integer | Minimum Digits | no |  |  | `purpose`: `minimum_digits` |
| `numberingtype` | select | Numbering Type | no |  |  | `purpose`: `gl_numbering_type`; `options`: `Repeatable`, `Permanent`; `option_meanings`: `repeatable:renumbers_all_each_run`, `permanent:numbers_fixed_future_dated_skipped`; `constraint`: `one_permanent_sequence_per_period_type_and_period`; `effect`: `gl_impact_locking_creates_copy_and_reversal` |
| `ordertype` | select | Order By | no |  |  | `purpose`: `numbering_order`; `options`: `transaction_date`, `entry_date` |
| `period` | select | Period | yes |  |  | `purpose`: `posting_period` |
| `prefix` | text | Prefix | no |  |  | `purpose`: `number_prefix` |
| `recurringtype` | select | Recurring Type | no |  |  | `purpose`: `recurring_unit_type`; `shown_only_when`: `numbering_type_permanent` |
| `sequencename` | text | Name | yes |  |  | `purpose`: `sequence_name` |
| `subsidiaries` | multiselect | Subsidiaries | yes |  |  | `purpose`: `available_subsidiaries`; `value_source`: `system_displayed` |
| `suffix` | text | Suffix | no |  |  | `purpose`: `number_suffix` |
| `externalid` | text | ExternalId | no |  |  |  |
| `id` | integer | Internal ID | no |  |  |  |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `accountingbook` | select | Accounting Book |  |
| `excludezerotransactions` | checkbox | Exclude zero amount transactions |  |
| `internalid` | integer | Internal ID |  |
| `internalidnumber` | integer | Internal ID Number |  |
| `isinactive` | checkbox | Inactive |  |
| `lastrunby` | text | Last Run By |  |
| `lastrundate` | date | Last Run Date |  |
| `mindigits` | integer | Minimum Digits |  |
| `nextrun` | date | Next Run |  |
| `period` | select | Posting Period |  |
| `prefix` | text | Prefix |  |
| `recurringtype` | select | Recurring Type |  |
| `sequencename` | text | Name |  |
| `subsidiaries` | select | Subsidiaries |  |
| `suffix` | text | Suffix |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `accountingbook` | text | Accounting Book |  |
| `dailyrecurring` | select | Daily Recurring |  |
| `excludezerotransactions` | checkbox | Exclude zero amount transactions |  |
| `hourlyrecurring` | integer | Hourly Recurring |  |
| `initnum` | integer | Initial Number |  |
| `internalid` | integer | ID |  |
| `isinactive` | checkbox | Inactive |  |
| `lastnumberassigned` | integer | Last Assigned |  |
| `lastrunby` | text | Last Run By |  |
| `lastrundate` | date | Last Run Date |  |
| `mindigits` | integer | Minimum Digits |  |
| `nextrun` | date | Next Run |  |
| `numberingtype` | text | Numbering Type |  |
| `ordertype` | text | Order By |  |
| `period` | text | Sequence Period |  |
| `prefix` | text | Prefix |  |
| `recurringtype` | select | Recurring Type |  |
| `sequencename` | text | Name |  |
| `status` | text | Status |  |
| `subsidiaries` | text | Subsidiaries |  |
| `suffix` | text | Suffix |  |
