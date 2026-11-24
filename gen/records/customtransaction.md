# Custom Transaction (`customtransaction`)
<!-- id: record:customtransaction -->

| Fact | Value |
|---|---|
| Internal ID | `customtransaction` |
| Search data via | Transaction |
| Tags | `custom_fields_supported`, `type_template` |

## Record facts

- `script_types`: `client`, `server`
- `user_events`: `beforeLoad`, `beforeSubmit`, `afterSubmit`
- `script_functions`: `create`, `update`, `copy`, `delete`, `search`
- `id_format`: `customtransaction<type_id>`
- `search_via` (refs `record:transaction`)
- `void_kinds`: `void_by_reversing_journal`
- `not_supported`: `direct_void`
- `status_field`: `transtatus`
- `value_examples`: `type_id_noie:customtransaction_noie`
- `ui_location`: `customization_lists_records_fields_transaction_types` (when `finding_type_id`)
- `requires`: `type_configured_for_void` (when `voiding`)
- `value_source`: `system_set_when_statuses_created` [`transtatus`, `read_only`]
- `allowed_values`: `A`, `Z` [`transtatus_letter_range`]
- `displayed_on`: `transaction_type_statuses_subtab_code_column` [`transtatus`]

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `class` | select | Class | no | no |  |  |
| `cleared` | checkbox | Cleared | no | no |  |  |
| `cleareddate` | date | Date Cleared | no | no |  |  |
| `createddate` | datetime |  | no | no |  |  |
| `credittotal` | currency | Credit | no | no |  |  |
| `currency` | select | Currency | yes | no |  |  |
| `customform` | select | Custom Form | yes | no |  |  |
| `debittotal` | currency | Debit | no | no |  |  |
| `department` | select | Department | no | no |  |  |
| `exchangerate` | currency2 | Exchange Rate | yes | no |  |  |
| `externalid` | text | ExternalId | no | no |  |  |
| `isbasecurrency` | checkbox | Base Currency | no | no |  |  |
| `lastmodifieddate` | datetime |  | no | no |  |  |
| `location` | select | Location | no | no |  |  |
| `memo` | textarea | Memo | no | no |  |  |
| `postingperiod` | select | Posting Period | no | no |  |  |
| `status` | text | Status | no | no |  |  |
| `statusRef` | text |  | no | no |  |  |
| `subsidiary` | select | Subsidiary | yes | no |  |  |
| `taxperiod` | text |  | no | no |  |  |
| `total` | currency | Amount | no | no |  |  |
| `trandate` | date | Date | yes | no |  |  |
| `tranid` | text | Entry No. | no | no |  |  |
| `transtatus` | select | Status | no | no |  |  |

## Sublist `accountingbookdetail` (Accounting Books)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `accountingbook` | select | Secondary Book | no |  |  |
| `exchangerate` | currency2 | Exchange Rate | yes |  |  |

## Sublist `activeworkflows` (Active Workflows)

## Sublist `activities` (Activities)

## Sublist `calls` (Phone Calls)

## Sublist `events` (Events)

## Sublist `line` (Lines)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `account` | select | Account | yes |  |  |
| `amount` | poscurrency | Amount | yes |  |  |
| `class` | select | Class | no |  |  |
| `credit` | currency | Credit | no |  |  |
| `debit` | currency | Debit | no |  |  |
| `department` | select | Department | no |  |  |
| `entity` | select | Name | no |  |  |
| `line` | integer |  | no |  |  |
| `location` | select | Location | no |  |  |
| `memo` | text | Memo | no |  |  |

## Sublist `mediaitem` (Files)

## Sublist `systemnotes` (System Notes)

## Sublist `tasks` (Tasks)

## Sublist `usernotes` (User Notes)

## Sublist `workflowhistory` (Workflow History)

## Tabs

| Tab ID | Label |
|---|---|
| `cmmnctntab` | Communication |
| `custom` | Custom |
| `lines` | Lines |
| `multibook` | Accounting Books |
| `s_sysinfo` | System Information |
| `workflow` | Workflow |
