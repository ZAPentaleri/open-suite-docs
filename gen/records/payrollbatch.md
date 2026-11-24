# Payroll Batch (`payrollbatch`)
<!-- id: record:payrollbatch -->

| Fact | Value |
|---|---|
| Internal ID | `payrollbatch` |
| Category | transaction |
| Client scripts | yes |
| Server scripts | yes |
| Searchable | yes |
| Tags | `custom_fields_supported`, `scripting_full` |

## Record facts

- `script_types`: `client`, `server`
- `script_functions`: `create`, `update`, `copy`, `delete`, `search`
- `requires`: `suitepeople_us_payroll`
- `workflow`: `create`, `submit_to_calculate`, `reload_until_status`, `submit_to_commit`
- `status_values`: `A:Created`, `B:Calculated`, `C:Edited`, `D:About to Commit`, `E:Committed_At_Service`, `F:Committed`, `P:Completed`, `R:Reversed`, `X:Error`
- `recommendation`: `poll_status_in_loop_for_large_batches`
- `rule`: `reload_until_calculated_committed_or_error`
- `illustrates`: `add_employee_to_payroll_batch`
- `doc_inconsistency`: `supported_records_table_id_contains_space` [`needs_human_review`]

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `account` | select | Account | no | no |  | `purpose`: `payroll_funding_account`; `value_source`: `set_up_payroll_page` |
| `balance` | currency | Balance | no | no |  | `purpose`: `bank_account_balance`; `value_source`: `system_displayed` |
| `offcycle` | checkbox | Off Cycle | no | no |  | `purpose`: `off_cycle_batch`; `caveat`: `does_not_update_last_paid_date` |
| `payfrequency` | select | Pay Frequency | yes | no |  | `purpose`: `pay_frequency`; `constraint`: `matches_employee_pay_frequency` |
| `periodending` | date | Period Ending | yes | no |  | `purpose`: `payroll_period_end` |
| `subsidiary` | select | Subsidiary | yes | no |  | `purpose`: `payroll_issuing_subsidiary`; `requires`: `valid_us_address_and_unique_ein` |
| `count` | integer | Count | no | no |  |  |
| `externalid` | text | ExternalId | no | no |  |  |
| `message` | text | Message | no | no |  |  |
| `payrolltype` | select | Type | no | no |  |  |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `batchnumber` | integer | Batch Number |  |
| `checkdate` | date | Check Date |  |
| `datecreated` | datetime | Date Created |  |
| `internalid` | select | Internal ID |  |
| `internalidnumber` | integer | Internal ID (Number) |  |
| `payfrequency` | text | Pay Frequency |  |
| `periodending` | date | Period Ending Date |  |
| `status` | select | Status |  |
| `subsidiary` | select | Subsidiary |  |
| `type` | text | Type |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `batchnumber` | text | Batch Number |  |
| `checkdate` | date | Check Date |  |
| `datecreated` | datetime | Date Created |  |
| `internalid` | select | Internal ID |  |
| `payfrequency` | text | Pay frequency |  |
| `periodending` | date | Period Ending Date |  |
| `status` | text | Status |  |
| `subsidiary` | text | Subsidiary |  |
| `type` | text | Type |  |

## Search joins

| Join ID | Label | Joined record |
|---|---|---|
| `user` | User | Employee |

## Suspected source errors

> [!SOURCE-ERROR] `supported_records_table_id_contains_space`
<!-- source-error: record:payrollbatch | - | supported_records_table_id_contains_space -->
