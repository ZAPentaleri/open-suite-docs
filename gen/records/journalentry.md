# Journal Entry (`journalentry`)
<!-- id: record:journalentry -->

| Fact | Value |
|---|---|
| Internal ID | `journalentry` |
| Category | transaction |
| Client scripts | yes |
| Server scripts | yes |
| Searchable | yes |
| Search data via | Transaction |
| Tags | `custom_fields_supported`, `scripting_full` |

## Record facts

- `script_types`: `client`, `server`
- `user_events`: `beforeLoad`, `beforeSubmit`, `afterSubmit`
- `user_events`: `beforeLoad`, `beforeSubmit`, `afterSubmit`
- `risk`: `updating_applied_journal_unapplies_payment`
- `book_specific_variant`: `multi_book_accounting`
- `purpose`: `adjust_ledger_balances_without_posting_transactions`
- `ui_location`: `transactions_financial_make_journal_entries`
- `ui_location`: `transactions_financial_make_book_specific_journal_entries` (when `multi_book_accounting`)
- `difference`: `book_specific_has_accountingbook_value_same_record_type`
- `can`: `attach_revenue_plan_to_book_specific_or_intercompany_entry` (when `advanced_revenue_management_essentials`)
- `value_examples`: `bookje_T_creates_book_specific_entry`
- `script_functions`: `create`, `copy`, `update`, `delete`, `search`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `accountingbook` | select | Accounting Book | no | no |  | `purpose`: `accounting_book`; `allowed_values`: `primary_book`, `accessible_secondary_books` |
| `approvalstatus` | select | Approval Status | no | no |  | `purpose`: `approval_status` |
| `approved` | checkbox | Approved | no | no |  | `purpose`: `journal_approved`; `true_means`: `posts_immediately`; `false_means`: `must_be_approved_before_posting` |
| `class` | select | Class | no | no |  | `purpose`: `class`; `allowed_values_listed_in`: `class_records`; `ui_effect`: `new_option_creates_record` |
| `createdfrom` | select | Void Of | no | no |  | `purpose`: `original_transaction_being_voided`; `value_source`: `system_displayed` |
| `credittotal` | currency | Credit | no | no |  | `purpose`: `total_credits`; `value_source`: `sum_of_credit_lines`; `constraint`: `debits_must_equal_credits_to_save` |
| `currency` | select | Currency | yes | no |  | `purpose`: `journal_currency`; `default_behavior`: `company_or_root_subsidiary_base_currency`; `default_behavior`: `initiating_subsidiary_base_currency` (when `oneworld_intercompany`); `allowed_values`: `subsidiary_or_to_subsidiary_currency` (when `oneworld_intercompany`) |
| `customform` | select | Custom Form | yes | no |  | `purpose`: `custom_form` |
| `debittotal` | currency | Debit | no | no |  | `purpose`: `total_debits`; `value_source`: `sum_of_debit_lines`; `constraint`: `debits_must_equal_credits_to_save` |
| `department` | select | Department | no | no |  | `purpose`: `line_department`; `allowed_values_listed_in`: `department_records`; `ui_effect`: `new_option_creates_record`; `help_text_shared_with`: `advintercompanyjournalentry.line.department`, `check.expense.department`, `check.item.department`, `journalentry.line.department`, `statisticaljournalentry.line.department`, `vendorbill.expense.department`, `vendorbill.item.department` |
| `exchangerate` | currency2 | Exchange Rate | yes | no |  | `purpose`: `exchange_rate`; `default_behavior`: `to_subsidiary_to_subsidiary_currency_rate`; `can`: `update_currency_exchange_rates` |
| `memo` | text | Memo | no | yes |  | `purpose`: `memo`; `displayed_on`: `transaction_detail`, `registers`; `value_kind`: `free_text` |
| `nextapprover` | select | Next Approver | no | no |  | `purpose`: `next_approver`; `effect`: `approval_link_in_reminders_portlet` |
| `nexus` | select | Nexus | no | no |  | `purpose`: `tax_nexus` |
| `postingperiod` | select | Posting Period | no | no |  | `purpose`: `posting_period`; `default_behavior`: `current_period`; `default_behavior`: `per_default_posting_period_preference` (when `period_closed_before_approval`); `value_source`: `set_on_approval` (when `subject_to_approval`) |
| `reversaldate` | date | Reversal Date | no | no |  | `purpose`: `reversal_date`; `required_when`: `reversaldefer_checked` |
| `reversaldefer` | checkbox | Defer Entry | no | no |  | `purpose`: `defer_reversal`; `true_means`: `reversal_is_memorized_transaction`; `false_means`: `reversal_entered_immediately`; `requires`: `reversaldate` |
| `subsidiary` | select | Subsidiary | yes | no |  | `purpose`: `journal_subsidiary`; `effect`: `restricts_visibility_to_subsidiary_entities`; `can`: `use_shared_vendors_of_subsidiary_on_lines`; `constraint`: `first_line_posts_to_subsidiary` (when `intercompany_journal`); `constraint`: `lines_post_to_subsidiary_or_to_subsidiary` (when `intercompany_journal`); `can`: `other_lines_post_to_any_accessible` (when `advanced_intercompany_journal`) |
| `subsidiarytaxregnum` | select | Tax Reg. Number | no | no |  | `purpose`: `subsidiary_tax_registration` |
| `tosubsidiary` | select | To Subsidiary | no | no |  | `purpose`: `to_subsidiary`; `constraint`: `lines_post_to_subsidiary_or_to_subsidiary` |
| `trandate` | date | Date | yes | no |  | `purpose`: `journal_date`; `default_behavior`: `current_date` |
| `tranid` | text | Entry No. | no | no |  | `purpose`: `journal_number`; `default_behavior`: `highest_number_plus_one`; `max_length`: `45` characters; `caveat`: `next_number_reverts_to_standard_pattern` |
| `createddate` | datetime |  | no | no |  |  |
| `entitynexus` | select | Nexus | no | no |  |  |
| `expenseallocjournalcount` | text | Count | no | no |  |  |
| `externalid` | text | ExternalId | no | no |  |  |
| `isbasecurrency` | checkbox | Base Currency | no | no |  |  |
| `isreversal` | text |  | no | no |  |  |
| `lastmodifieddate` | datetime |  | no | no |  |  |
| `parentexpensealloc` | select | Created From Allocation | no | no |  |  |
| `status` | text |  | no | no |  |  |
| `taxpointdate` | date | Tax Point Date | no | no |  |  |
| `taxpointdateoverride` | checkbox | Override | no | no |  |  |
| `void` | text |  | no | no |  |  |
| `voided` | text |  | no | no |  |  |

## Sublist `line` (Lines)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `department` | select | Department | no |  | `purpose`: `line_department`; `allowed_values_listed_in`: `department_records`; `ui_effect`: `new_option_creates_record`; `help_text_shared_with`: `journalentry.department` |
| `location` | select | Location | no |  | `purpose`: `line_location`; `allowed_values_listed_in`: `location_records`; `ui_effect`: `new_option_creates_record`; `help_text_shared_with`: `vendorbill.location`, `vendorcredit.location` |
| `account` | select | Account | yes |  |  |
| `amortizationtype` | text |  | no |  |  |
| `cleared` | checkbox | Cleared | no |  |  |
| `cleareddate` | date | Date Cleared | no |  |  |
| `credit` | currency | Credit | no |  |  |
| `credittax` | currency | Credit Tax | no |  |  |
| `debit` | currency | Debit | no |  |  |
| `debittax` | currency | Debit Tax | no |  |  |
| `eliminate` | checkbox | Eliminate | no |  |  |
| `enddate` | date | End Date | no |  |  |
| `entity` | select | Name | no |  |  |
| `entitytype` | text |  | no |  |  |
| `giftcertcode` | text |  | no |  |  |
| `grossamt` | currency | Gross Amt | no |  |  |
| `item` | integer |  | no |  |  |
| `line` | integer |  | no |  |  |
| `linetaxcode` | select | Tax Code | no |  |  |
| `linetaxrate` | percent | Tax Rate | no |  |  |
| `memo` | text | Memo | no |  |  |
| `residual` | rate | Residual | no |  |  |
| `revenuerecognitionrule` | select | Revenue Recognition Rule | no |  |  |
| `schedule` | select | Schedule | no |  |  |
| `schedulenum` | select | Schedule No. | no |  |  |
| `scheduletype` | text |  | no |  |  |
| `startdate` | date | Start Date | no |  |  |
| `tax1acct` | select | Tax Account | no |  |  |
| `tax1amt` | currency | VAT Amt | no |  |  |
| `taxaccount` | select | Tax Account | no |  |  |
| `taxbasis` | currency | Tax Basis | no |  |  |
| `taxcode` | select | VAT | no |  |  |
| `taxrate1` | percent | VAT Rate | no |  |  |
| `totalamount` | currency | Total Amount | no |  |  |

## Sublist `accountingbookdetail` (Accounting Books)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `accountingbook` | select | Secondary Book | no |  |  |
| `exchangerate` | currency2 | Exchange Rate | yes |  |  |

## Tabs

| Tab ID | Label |
|---|---|
| `custom` | Custom |
| `history` | History |
| `lines` | Lines |
| `workflow` | Workflow |
