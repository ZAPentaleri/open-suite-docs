# Subsidiary Settings (`subsidiarysettings`)
<!-- id: record:subsidiarysettings -->

| Fact | Value |
|---|---|
| Internal ID | `subsidiarysettings` |
| Category | list |
| Client scripts | no |
| Server scripts | yes |

## Record facts

- `script_types`: `server`
- `script_functions`: `read`, `update`, `search`
- `script_types`: `server`
- `script_functions`: `read`, `update`, `search`
- `search_via`: `suiteanalytics_workbook_only`
- `requires_permission`: `subsidiary_settings_manager_edit`
- `search_only_sublists`: `systemnotes`
- `rule`: `one_to_one_with_subsidiary`
- `same_as`: `subsidiary_internal_and_external_id`
- `effect`: `created_and_deleted_with_subsidiary`
- `applies_to`: `all_server_script_types`
- `ui_location`: `subsidiary_settings_manager_page`
- `requires`: `feature_enabled_on_enable_features_first`
- `value_examples`: `enable_period_end_journal_entries_for_subsidiary`
- `caveat`: `search_only_via_suiteanalytics_workbook`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `accountingbooks` | multiselect | Accounting Books | no |  |  | `purpose`: `period_end_accounting_books`; `requires_feature`: `Multi-Book Accounting` [`real_name`]; `caveat`: `inaccessible_books_hidden_and_may_block_disabling` |
| `allowperiodendjournalentries` | checkbox | Enable Period End Journal Entries | no |  |  | `purpose`: `enable_period_end_journals` |
| `balancesheetclosingaccount` | select | Balance Sheet Closing Account | no |  |  | `purpose`: `balance_sheet_closing_account`; `constraint`: `equity_account` |
| `balancesheetopeningaccount` | select | Balance Sheet Opening Account | no |  |  | `purpose`: `balance_sheet_opening_account`; `constraint`: `equity_account` |
| `createbscloseandopenjournals` | checkbox | Create Balance Sheet Closing and Opening Journals | no |  |  | `purpose`: `create_balance_sheet_close_and_open_journals`; `effect`: `adds_balance_sheet_closing_step`; `effect`: `closing_reversed_first_day_next_year` |
| `createincomesummaryjournals` | checkbox | Create Income Summary Journals | no |  |  | `purpose`: `create_income_summary_journals`; `effect`: `income_and_expense_zeroed_to_income_summary`; `effect`: `income_summary_moved_to_retained_earnings` |
| `excludebsfromglnumbering` | checkbox | Exclude Balance Sheet Closing Journals From GL Numbering | no |  |  | `purpose`: `exclude_bs_closing_from_gl_numbering`; `shown_only_when`: `gl_audit_numbering_feature` |
| `groupbythesesegments` | multiselect | Group By These Segments | no |  |  | `purpose`: `period_end_segments`; `value_kind`: `multi_select`; `effect`: `separate_journals_per_segment_combination` |
| `incomesummarylossaccount` | select | Income Summary Loss Account | no |  |  | `purpose`: `income_summary_loss_account`; `constraint`: `equity_account` |
| `incomesummaryprofitaccount` | select | Income Summary Profit Account | no |  |  | `purpose`: `income_summary_profit_account`; `constraint`: `equity_account` |
| `requirememoonperiodendjournals` | checkbox | Require Memo on Period End Journals | no |  |  | `purpose`: `require_memo_on_period_end_journals`; `constraint`: `header_and_line_memos_required`; `effect`: `entered_memo_fills_header_and_lines` |
| `subsidiary` | select | Subsidiary | no |  |  | `purpose`: `subsidiary`; `value_source`: `system_displayed` |
| `externalid` | text | External ID | no |  |  |  |
| `id` | integer | Internal ID | no |  |  |  |
