# Period End Journal (`periodendjournal`)
<!-- id: record:periodendjournal -->

| Fact | Value |
|---|---|
| Internal ID | `periodendjournal` |
| Category | transaction |
| Client scripts | yes |
| Server scripts | yes |
| Search data via | Transaction |

## Record facts

- `script_types`: `client`, `server`
- `script_functions`: `read`, `update`, `search`
- `requires_feature`: `Period End Journal Entries`, `Accounting Periods` [`real_name`]
- `requires`: `oneworld`
- `script_types`: `client`, `server`
- `user_events_from_generation`: `false`
- `script_functions`: `read`, `update`, `search`
- `updatable_fields`: `memo`, `non_gl_custom_fields`, `non_gl_custom_segments`
- `search_requires`: `include_period_end_transactions_true`
- `purpose`: `posts_consolidation_and_year_end_closing`
- `effect`: `generated_by_create_period_end_journals_task`
- `ui_location`: `period_close_checklist`
- `can`: `user_events_fire_outside_generation_process`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `class` | select | Class | no | no |  | `purpose`: `segment`; `shown_only_when`: `create_period_end_journals_by_segment`; `help_text_shared_with`: `periodendjournal.department` |
| `credittotal` | currency | Credit | no | no |  | `purpose`: `balancing_total`; `value_source`: `system_displayed`; `help_text_shared_with`: `periodendjournal.debittotal` |
| `currency` | select | Currency | no | no |  | `purpose`: `currency`; `value_source`: `subsidiary_base_currency` |
| `debittotal` | currency | Debit | no | no |  | `purpose`: `balancing_total`; `value_source`: `system_displayed`; `help_text_shared_with`: `periodendjournal.credittotal` |
| `department` | select | Department | no | no |  | `purpose`: `segment`; `shown_only_when`: `create_period_end_journals_by_segment`; `help_text_shared_with`: `periodendjournal.class` |
| `mainaccount` | select | Main Account | yes | no |  | `purpose`: `period_end_main_account`; `allowed_values`: `cumulative_translation_adjustment`, `retained_earnings`, `income_summary_account`, `balance_sheet_closing_or_opening_account`; `configured_by`: `subsidiary_period_end_preferences`; `comparable_to`: `search_account_main` |
| `postingperiod` | select | Posting Period | no | no |  | `purpose`: `period_of_period_end_task`; `value_source`: `system_displayed` |
| `sourcesubsidiary` | select | Source Subsidiary | yes | no |  | `purpose`: `source_subsidiary`; `value_kind`: `child_subsidiary_for_consolidation_journals`; `caveat`: `child_needs_no_period_end_feature` |
| `subsidiary` | select | Subsidiary | yes | no |  | `purpose`: `posting_subsidiary`; `requires`: `enable_period_end_journal_entries_checked` |
| `trandate` | date | Date | yes | no |  | `purpose`: `journal_date`; `value_source`: `last_day_of_period`; `value_source`: `first_day_after_fiscal_year_end` (when `balance_sheet_closing_reversal`) |
| `tranid` | text | Entry No. | no | no |  | `purpose`: `document_number`; `value_source`: `auto_generated_on_save` |
| `accountingbook` | select | Accounting Book | yes | no |  |  |
| `createddate` | datetime |  | no | no |  |  |
| `customform` | select | Custom Form | yes | no |  |  |
| `externalid` | text | ExternalId | no | no |  |  |
| `lastmodifieddate` | datetime |  | no | no |  |  |
| `location` | select | Location | no | no |  |  |
| `memo` | text | Memo | no | yes |  |  |
| `transactionnumber` | text | Transaction Number | no | no |  |  |

## Sublist `line` (Lines)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `account` | select | Account | yes |  |  |
| `credit` | currency | Credit | no |  |  |
| `debit` | currency | Debit | no |  |  |
| `line` | integer |  | no |  |  |
| `memo` | text | Memo | no |  |  |
