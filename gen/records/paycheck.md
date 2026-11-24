# Paycheck (`paycheck`)
<!-- id: record:paycheck -->

| Fact | Value |
|---|---|
| Internal ID | `paycheck` |
| Category | transaction |
| Client scripts | yes |
| Server scripts | yes |
| Tags | `custom_fields_supported`, `supports_read`, `supports_update` |

## Record facts

- `script_types`: `client`, `server`
- `requires_feature`: `Payroll` [`real_name`]
- `script_functions`: `read`, `update`
- `created_by` (refs `record:payrollbatch`)
- `configured_by`: `enable_features_employees_subtab` [`payroll`]
- `ui_location`: `transactions_employees_create_payroll`
- `doc_inconsistency`: `says_read_update_only_but_sample_searches`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `account` | select | Account | no | no |  | `purpose`: `paycheck_bank_account` |
| `address` | address | Address | no | no |  | `purpose`: `employee_address` |
| `balance` | currency | Balance | no | no |  | `purpose`: `bank_account_balance`; `value_source`: `system_displayed` |
| `class` | select | Class | no | no |  | `purpose`: `class`; `default_behavior`: `employee_class` |
| `department` | select | Department | no | no |  | `purpose`: `department`; `default_behavior`: `employee_department` |
| `entity` | select | Payee | yes | no |  | `purpose`: `employee` |
| `memo` | text | Memo | no | no |  | `purpose`: `memo`; `displayed_on`: `checks` |
| `periodending` | date | Period Ending | no | no |  | `purpose`: `payroll_period_end` |
| `postingperiod` | select | Posting Period | no | no |  | `purpose`: `posting_period`; `cannot`: `post_to_closed_period` |
| `trandate` | date | Check Date | yes | no |  | `purpose`: `posting_date`; `default_behavior`: `current_date`; `displayed_on`: `registers` |
| `tranid` | rtext | Check # | no | no |  | `purpose`: `check_number`; `default_behavior`: `highest_number_plus_one` (when `printing_now`); `caveat`: `numbers_chosen_at_print` (when `tobeprinted`) |
| `useramount` | currency | Amount | yes | no |  | `purpose`: `net_pay_amount`; `value_source`: `calculated_from_lines` |
| `workplace` | select | Workplace | no | no |  | `purpose`: `workplace`; `value_source`: `employee_workplace` |
| `chkamount` | currency | Amount | no | no |  |  |
| `cleared` | checkbox | Cleared | no |  |  |  |
| `cleareddate` | date | Date Cleared | no |  |  |  |
| `createddate` | datetime |  | no | no |  |  |
| `externalid` | text | ExternalId | no | no |  |  |
| `lastmodifieddate` | datetime |  | no | no |  |  |
| `startdate` | date |  | no | no |  |  |
| `status` | text | Status | no | no |  |  |
| `statusref` | text |  | no | no |  |  |
| `taxperiod` | text |  | no | no |  |  |

## Sublist `paycontrib` (Contributions)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `amount` | currency | Amount | yes |  |  |
| `manualentry` | checkbox | Manual Entry | no |  |  |
| `payitem` | select | Contributions | yes |  |  |
| `wagebase` | currency | Wage Base | no |  |  |

## Sublist `paydeduct` (Deductions)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `amount` | currency | Amount | yes |  |  |
| `manualentry` | checkbox | Manual Entry | no |  |  |
| `payitem` | select | Deductions | yes |  |  |
| `wagebase` | currency | Wage Base | no |  |  |

## Sublist `payearn` (Earnings)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `count` | float | Hours/Base | no |  |  |
| `grossamount` | currency | Amount | yes |  |  |
| `manualentry` | checkbox | Manual Entry | no |  |  |
| `payitem` | select | Earnings | yes |  |  |
| `rate` | rate | Rate | no |  |  |
| `serviceitem` | select | Service Item | no |  |  |

## Sublist `paytax` (Taxes)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `amount` | currency | Amount | yes |  |  |
| `isexempt` | checkbox | Exempt | no |  |  |
| `isresidenttax` | checkbox | Resident Status | no |  |  |
| `paytax` | text | Taxes | no |  |  |
| `reportablewagebase` | currency | Reportable Wage Base | no |  |  |
| `sequence` | integer | Sequence # | no |  |  |
| `taxablewagebase` | currency | Taxable Wage Base | no |  |  |
| `taxedwagebase` | currency | Taxed Wage Base | no |  |  |

## Sublist `paytime` (Time)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `amount` | currency | Amount | no |  |  |
| `apply` | checkbox | Pay | no |  |  |
| `count` | float | Hours | no |  |  |
| `custjob` | text | Customer:Job | no |  |  |
| `ddate` | date | Date | no |  |  |
| `payitemname` | text | Time Type | no |  |  |
| `rate` | currency | Rate | no |  |  |
| `serviceitem` | text | Service Item | no |  |  |

## Suspected source errors

> [!SOURCE-ERROR] `says_read_update_only_but_sample_searches`
<!-- source-error: record:paycheck | - | says_read_update_only_but_sample_searches -->
