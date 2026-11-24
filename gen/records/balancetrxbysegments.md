# Balance Transactions by Segments (`balancetrxbysegments`)
<!-- id: record:balancetrxbysegments -->

| Fact | Value |
|---|---|
| Internal ID | `balancetrxbysegments` |
| Category | transaction |
| Client scripts | no |
| Server scripts | yes |
| Searchable | yes |
| Tags | `supports_create`, `supports_read`, `supports_search` |

## Record facts

- `script_types`: `server`
- `requires_feature`: `Balancing Segments`, `Custom Segments` [`real_name`]
- `script_functions`: `create`, `read`, `search`
- `not_supported`: `update`, `copy`, `transform`, `delete`
- `not_scriptable`: `transactions_subtab`, `results_subtab`
- `purpose`: `select_transactions_for_balancing_by_segment`
- `effect`: `generates_balancing_journals`
- `displayed_on`: `view_mode_transactions_and_results_subtabs`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `accountingbook` | select | Accounting Books | yes | no |  | `purpose`: `accounting_books_of_run`; `value_kind`: `multi_select` |
| `balancingsegments` | select | Balancing Segments | no | no |  | `purpose`: `balancing_segments` |
| `datetimecreated` | datetime | Date/Time | no | no |  | `purpose`: `run_date_time`; `value_source`: `system_recorded` |
| `includechildren` | checkbox | Include Children | no | no |  | `purpose`: `include_child_subsidiaries` |
| `internalid` | integer | Internal ID | no | no |  | `purpose`: `internal_id`; `value_source`: `system_recorded` |
| `memo` | text | Memo | no | no |  | `purpose`: `memo`; `displayed_on`: `balancing_journals`, `balancing_journal_list` |
| `percentagecomplete` | text | Percent Complete | no | no |  | `purpose`: `percent_complete`; `value_source`: `process_progress` |
| `postingperiod` | select | Posting Period | yes | no |  | `purpose`: `posting_period_to_balance` |
| `status` | select | Status | no | no |  | `purpose`: `process_status`; `value_source`: `system_displayed` |
| `subsidiary` | select | Subsidiaries | yes | no |  | `purpose`: `subsidiaries_to_balance`; `value_kind`: `multi_select` |
| `createdby` | select | Created by | no | no |  |  |
| `externalid` | text | External ID | no | no |  |  |
