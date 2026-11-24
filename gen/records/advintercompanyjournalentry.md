# Advanced Intercompany Journal Entry (`advintercompanyjournalentry`)
<!-- id: record:advintercompanyjournalentry -->

| Fact | Value |
|---|---|
| Internal ID | `advintercompanyjournalentry` |
| Category | transaction |
| Client scripts | yes |
| Server scripts | yes |
| Searchable | yes |
| Search data via | Transaction |
| Tags | `custom_fields_supported`, `scripting_full` |

## Record facts

- `script_types`: `client`, `server`
- `script_functions`: `create`, `update`, `copy`, `delete`, `search`
- `user_events`: `beforeLoad`, `beforeSubmit`, `afterSubmit`
- `user_events`: `beforeLoad`, `beforeSubmit`, `afterSubmit`
- `risk`: `updating_applied_journal_unapplies_payment`
- `book_specific_variant`: `multi_book_accounting`
- `purpose`: `intercompany_journal_with_extra_time_saving_functions`
- `ui_location`: `transactions_financial_make_advanced_intercompany_journals`
- `difference`: `book_specific_has_accountingbook_value_same_record_type`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `approved` | checkbox | Approved | no | no |  | `purpose`: `journal_approved`; `true_means`: `posts_immediately`; `false_means`: `must_be_approved_before_posting` |
| `createdfrom` | select | Void Of | no | no |  | `purpose`: `original_transaction_being_voided`; `value_source`: `system_displayed` |
| `credittotal` | currency | Credit | no | no |  | `purpose`: `total_credits`; `value_source`: `sum_of_credit_lines`; `constraint`: `debits_must_equal_credits_to_save` |
| `currency` | select | Currency | yes | no |  | `purpose`: `journal_currency`; `default_behavior`: `company_or_root_subsidiary_base_currency`; `default_behavior`: `initiating_subsidiary_base_currency` (when `oneworld_intercompany`); `allowed_values`: `subsidiary_or_to_subsidiary_currency` (when `oneworld_intercompany`) |
| `customform` | select | Custom Form | yes | no |  | `purpose`: `custom_form` |
| `debittotal` | currency | Debit | no | no |  | `purpose`: `total_debits`; `value_source`: `sum_of_debit_lines`; `constraint`: `debits_must_equal_credits_to_save` |
| `memo` | text | Memo | no | yes |  | `purpose`: `memo`; `displayed_on`: `transaction_detail`, `registers`; `value_kind`: `free_text` |
| `nexus` | select | Nexus | no | no |  | `purpose`: `tax_nexus` |
| `postingperiod` | select | Posting Period | no | no |  | `purpose`: `posting_period`; `default_behavior`: `current_period`; `default_behavior`: `per_default_posting_period_preference` (when `period_closed_before_approval`); `value_source`: `set_on_approval` (when `subject_to_approval`) |
| `reversaldate` | date | Reversal Date | no | no |  | `purpose`: `reversal_date`; `required_when`: `reversaldefer_checked` |
| `reversaldefer` | checkbox | Defer Entry | no | no |  | `purpose`: `defer_reversal`; `true_means`: `reversal_is_memorized_transaction`; `false_means`: `reversal_entered_immediately`; `requires`: `reversaldate` |
| `subsidiary` | select | Subsidiary | yes | no |  | `purpose`: `journal_subsidiary`; `effect`: `restricts_visibility_to_subsidiary_entities`; `can`: `use_shared_vendors_of_subsidiary_on_lines`; `constraint`: `first_line_posts_to_subsidiary` (when `intercompany_journal`); `constraint`: `lines_post_to_subsidiary_or_to_subsidiary` (when `intercompany_journal`); `can`: `other_lines_post_to_any_accessible` (when `advanced_intercompany_journal`) |
| `trandate` | date | Date | yes | no |  | `purpose`: `journal_date`; `default_behavior`: `current_date` |
| `tranid` | text | Entry No. | no | no |  | `purpose`: `journal_number`; `default_behavior`: `highest_number_plus_one`; `max_length`: `45` characters; `caveat`: `next_number_reverts_to_standard_pattern` |
| `createddate` | datetime |  | no | no |  |  |
| `entitynexus` | select | Nexus | no | no |  |  |
| `expenseallocjournalcount` | text | Count | no | no |  |  |
| `externalid` | text | ExternalId | no | no |  |  |
| `isbasecurrency` | checkbox | Base Currency | no | no |  |  |
| `iscustomapproval` | checkbox | Custom Approval | no | no |  |  |
| `lastmodifieddate` | datetime |  | no | no |  |  |
| `parentexpensealloc` | select | Created From Allocation | no | no |  |  |
| `status` | text |  | no | no |  |  |
| `taxperiod` | text |  | no | no |  |  |
| `tosubsidiaries` | select |  | no | no |  |  |
| `transactionnumber` | text | Transaction Number | no | no |  |  |

## Sublist `line`

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `class` | select | Class | no |  | `purpose`: `line_class`; `allowed_values_listed_in`: `class_records`; `ui_effect`: `new_option_creates_record` |
| `department` | select | Department | no |  | `purpose`: `line_department`; `allowed_values_listed_in`: `department_records`; `ui_effect`: `new_option_creates_record`; `help_text_shared_with`: `journalentry.department` |
| `location` | select | Location | no |  | `purpose`: `line_location`; `allowed_values_listed_in`: `location_records`; `ui_effect`: `new_option_creates_record`; `help_text_shared_with`: `vendorbill.location`, `vendorcredit.location` |
| `account` | select | Account | yes |  |  |
| `amortizationtype` | text |  | no |  |  |
| `credit` | currency | Credit | no |  |  |
| `debit` | currency | Debit | no |  |  |
| `duetofromsubsidiary` | select | Due To/From {Subsidiary} | no |  |  |
| `eliminate` | checkbox | Eliminate | no |  |  |
| `enddate` | date | End Date | no |  |  |
| `entity` | select | Name | no |  |  |
| `entitytype` | text |  | no |  |  |
| `giftcertcode` | text |  | no |  |  |
| `grossamt` | currency | Gross Amt | no |  |  |
| `item` | integer |  | no |  |  |
| `line` | integer |  | no |  |  |
| `linebasecurrency` | select | Base Currency | no |  |  |
| `linefxrate` | currency2 | Exchange Rate | yes |  |  |
| `linesubsidiary` | select | Subsidiary | yes |  |  |
| `linetotalamt` | poscurrency | Total Amount (Base Currency) | no |  |  |
| `memo` | text | Memo | no |  |  |
| `residual` | rate | Residual | no |  |  |
| `schedule` | select | Schedule | no |  |  |
| `schedulenum` | select | Schedule No. | no |  |  |
| `scheduletype` | text |  | no |  |  |
| `startdate` | date | Start Date | no |  |  |
| `tax1acct` | select | Tax Account | no |  |  |
| `tax1amt` | currency | VAT Amt | no |  |  |
| `taxcode` | select | VAT | no |  |  |
| `taxrate1` | percent | VAT Rate | no |  |  |
