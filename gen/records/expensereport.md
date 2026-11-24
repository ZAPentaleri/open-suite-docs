# Expense Report (`expensereport`)
<!-- id: record:expensereport -->

| Fact | Value |
|---|---|
| Internal ID | `expensereport` |
| Category | transaction |
| Client scripts | yes |
| Server scripts | yes |
| Searchable | yes |
| Search data via | Transaction |
| Tags | `custom_fields_supported`, `scripting_full` |

## Record facts

- `initialization_default_value`: `entity`, `entity_internal_id` (refs `N/record.create`, `N/record.copy`, `N/record.transform`, `N/record.load`)
- `script_types`: `client`, `server`
- `script_functions`: `create`, `update`, `copy`, `delete`, `search`, `transform`
- `doc_inconsistency`: `feature_named_estimates`
- `purpose`: `employee_expenses_for_approval_and_billing`
- `effect`: `no_gl_impact_until_approved_then_bill_created`
- `configured_by`: `enable_features_employees_subtab`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `account` | select | Account | no | no |  | `purpose`: `expense_account`; `editable_only_when`: `accounting_approval_or_editing_existing` |
| `accountingapproval` | checkbox | Accounting Approval | no | no |  | `purpose`: `accounting_approval`; `requires`: `accounting_approval_authority` |
| `acctcorpcardexp` | select | Account for Corporate Card Expenses | no | no |  | `purpose`: `corporate_card_account` |
| `advance` | currency | Advance To Apply | no | no |  | `purpose`: `advance_to_apply` |
| `amount` | currency | Amount | no | no |  | `purpose`: `reimbursable_amount`; `value_source`: `lines_minus_advance_and_non_reimbursable` |
| `approvalstatus` | select | Approval Status | no | no |  | `purpose`: `approval_status`; `applies_when`: `suiteflow_expense_approvals`; `allowed_values`: `Pending Approval`, `Approved`, `Rejected`; `option_meanings`: `pending_approval:in_approval_queue`, `approved:approved_for_processing`, `rejected:not_processed` |
| `class` | select | Class | no | no |  | `purpose`: `class`; `allowed_values_listed_in`: `class_records`; `ui_effect`: `new_option_creates_record` |
| `complete` | checkbox | Complete | no | no |  | `purpose`: `report_complete`; `constraint`: `only_complete_reports_submitted`; `cannot`: `change_after_complete` |
| `customform` | select | Custom Form | yes | no |  | `purpose`: `custom_form` |
| `department` | select | Department | no | no |  | `purpose`: `department`; `allowed_values_listed_in`: `department_records`; `ui_effect`: `new_option_creates_record` |
| `duedate` | date | Date Due | no | no |  | `purpose`: `due_date` |
| `entity` | select | Employee | yes | no |  | `purpose`: `employee` |
| `expensereportcurrency` | select | Currency | yes | no |  | `purpose`: `expense_report_currency` |
| `expensereportexchangerate` | currency2 | Exchange Rate | yes | no |  | `purpose`: `expense_report_exchange_rate` |
| `location` | select | Location | no | no |  | `purpose`: `transaction_location`; `allowed_values_listed_in`: `location_records`; `ui_effect`: `new_option_creates_record` |
| `memo` | text | Purpose | no | yes |  | `purpose`: `report_purpose_memo` |
| `nextapprover` | select | Next Approver | no | no |  | `purpose`: `next_approver`; `value_source`: `approval_workflow`; `applies_when`: `suiteflow_expense_approvals`; `value_examples`: `pending_approval_state_sets_supervisor` |
| `nexus` | select | Nexus | no | no |  | `purpose`: `nexus`; `allowed_values`: `subsidiary_nexuses` |
| `postingperiod` | select | Posting Period | yes | no |  | `purpose`: `posting_period`; `default_behavior`: `current_period`; `default_behavior`: `per_default_posting_period_preference` (when `period_closed_before_approval`); `value_source`: `set_on_approval` (when `subject_to_approval`) |
| `subsidiary` | select | Subsidiary | no | no |  | `purpose`: `subsidiary`; `value_source`: `employee_subsidiary`; `requires_feature`: `Intercompany Time and Expense` (when `other_subsidiary_expenses`) [`real_name`]; `requires`: `intercompany_adjustment_journals` (when `other_subsidiary_expenses`) |
| `subsidiarytaxregnum` | select | Tax Reg. Number | no | no |  | `purpose`: `subsidiary_tax_registration`; `value_source`: `selected_nexus`; `effect`: `transaction_date_picks_valid_registration` |
| `supervisorapproval` | checkbox | Supervisor Approval | no | no |  | `purpose`: `supervisor_approval` |
| `total` | currency | Expenses Total | no | no |  | `purpose`: `total_expenses`; `value_source`: `sum_of_lines` |
| `trandate` | date | Date | yes | no |  | `purpose`: `transaction_date`; `default_behavior`: `current_date` |
| `tranid` | text | Exp. Rept. # | no | no |  | `purpose`: `expense_report_number`; `default_behavior`: `highest_number_plus_one`; `max_length`: `45` characters; `caveat`: `next_number_reverts_to_standard_pattern` |
| `usemulticurrency` | checkbox | Use Multi Currency | no | no |  | `purpose`: `use_multicurrency`; `default_behavior`: `use_multicurrency_expense_reports_preference`; `caveat`: `line_conversions_approximate`; `effect`: `foreign_amounts_translated_to_base`; `caveat`: `reimbursed_at_rate_on_reimbursement_date` |
| `createddate` | datetime |  | no | no |  |  |
| `entitynexus` | select | Nexus | no | no |  |  |
| `externalid` | text | ExternalId | no | no |  |  |
| `lastmodifieddate` | datetime |  | no | no |  |  |
| `policyviolated` | checkbox | Policy Violated | no | no |  |  |
| `status` | text | Status | no | no |  |  |
| `statusRef` | text |  | no | no |  |  |
| `taxdetailsoverride` | checkbox | Tax Details Override | no | no |  |  |
| `taxpointdate` | date | Tax Point Date | no | no |  |  |
| `taxpointdateoverride` | checkbox | Override | no | no |  |  |

## Sublist `accountingbookdetail` (Accounting Books)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `accountingbook` | select | Secondary Book | no |  |  |
| `exchangerate` | currency2 | Exchange Rate | yes |  |  |

## Sublist `expense` (Expenses)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `amount` | currency | Amount | yes |  |  |
| `category` | select | Category | yes |  |  |
| `class` | select | Class | no |  |  |
| `corporatecreditcard` | checkbox | Corporate Card | no |  |  |
| `currency` | select | Currency | yes |  |  |
| `customer` | select | Customer | no |  |  |
| `department` | select | Department | no |  |  |
| `exchangerate` | currency2 | Exchange Rate | no |  |  |
| `expenseaccount` | select | Expense Account | yes |  |  |
| `expensedate` | date | Date | yes |  |  |
| `foreignamount` | currency2 | Foreign Amount | no |  |  |
| `isbillable` | checkbox | Billable | no |  |  |
| `isnonreimbursable` | checkbox | Non-reimbursable | no |  |  |
| `line` | text |  | no |  |  |
| `location` | select | Location | no |  |  |
| `memo` | text | Memo | no |  |  |
| `receipt` | checkbox | Receipt | no |  |  |
| `taxdetailsreference` | text | Tax Details Reference | no |  |  |

## Sublist `taxdetails` (Taxes)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `calcdetail` | textarea | Details | no |  |  |
| `taxamount` | currency | Tax Amount | yes |  |  |
| `taxbasis` | currency | Tax Basis | yes |  |  |
| `taxcode` | select | Tax Code | yes |  |  |
| `taxdetailsreference` | text | Tax Details Reference | yes |  |  |
| `taxrate` | percent | Tax Rate | yes |  |  |
| `taxtype` | select | Tax Type | yes |  |  |

## Tabs

| Tab ID | Label |
|---|---|
| `custom` | Custom |
| `history` | History |
| `items` | Items |
| `workflow` | Workflow |

## Suspected source errors

> [!SOURCE-ERROR] `feature_named_estimates`
<!-- source-error: record:expensereport | - | feature_named_estimates -->
