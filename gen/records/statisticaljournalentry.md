# Statistical Journal Entry (`statisticaljournalentry`)
<!-- id: record:statisticaljournalentry -->

| Fact | Value |
|---|---|
| Internal ID | `statisticaljournalentry` |
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
- `requires_feature`: `Statistical Accounts` [`real_name`]
- `requires`: `existing_statistical_account`
- `user_events`: `beforeLoad`, `beforeSubmit`, `afterSubmit`
- `read_only_fields`: `unitlabel`
- `not_updatable_fields`: `unitstype`
- `field_alias`: `amount:debit`
- `rule`: `at_least_one_line`
- `rule`: `all_lines_same_unit_of_measure`
- `purpose`: `raise_or_lower_statistical_account_balance`
- `configured_by`: `enable_features_accounting_subtab` [`statistical_accounts`]
- `ui_location`: `transactions_financial_make_statistical_journal_entries`
- `value_source`: `unitstype_base_unit` [`unitlabel`]
- `editable_only_when`: `dynamic_mode` [`subsidiary`]
- `doc_inconsistency`: `subsidiary_sample_sentence_garbled`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `approved` | checkbox | Approved | no | no |  | `purpose`: `journal_approved`; `true_means`: `posts_immediately`; `false_means`: `must_be_approved_before_posting` |
| `customform` | select | Custom Form | yes | no |  | `purpose`: `custom_form` |
| `memo` | text | Memo | no | yes |  | `purpose`: `memo`; `displayed_on`: `transaction_detail`, `registers`; `value_kind`: `free_text` |
| `postingperiod` | select | Accounting Period | no | no |  | `purpose`: `posting_period`; `default_behavior`: `current_period`; `default_behavior`: `per_default_posting_period_preference` (when `period_closed_before_approval`); `value_source`: `set_on_approval` (when `subject_to_approval`) |
| `reversaldate` | date | Reversal Date | no | no |  | `purpose`: `reversal_date`; `required_when`: `reversaldefer_checked` |
| `reversaldefer` | checkbox | Defer Entry | no | no |  | `purpose`: `defer_reversal`; `true_means`: `reversal_is_memorized_transaction`; `false_means`: `reversal_entered_immediately`; `requires`: `reversaldate` |
| `subsidiary` | select | Subsidiary | yes | no |  | `purpose`: `journal_subsidiary`; `effect`: `restricts_visibility_to_subsidiary_entities`; `can`: `use_shared_vendors_of_subsidiary_on_lines`; `constraint`: `first_line_posts_to_subsidiary` (when `intercompany_journal`); `constraint`: `lines_post_to_subsidiary_or_to_subsidiary` (when `intercompany_journal`); `can`: `other_lines_post_to_any_accessible` (when `advanced_intercompany_journal`) |
| `trandate` | date | Date | yes | no |  | `purpose`: `journal_date`; `default_behavior`: `current_date` |
| `tranid` | text | Entry No. | no | no |  | `purpose`: `journal_number`; `default_behavior`: `highest_number_plus_one`; `max_length`: `45` characters; `caveat`: `next_number_reverts_to_standard_pattern` |
| `unitstype` | select | Unit of Measure Type | no | no |  | `purpose`: `unit_type`; `value_source`: `system_displayed` |
| `createddate` | datetime |  | no | no |  |  |
| `externalid` | text | ExternalId | no | no |  |  |
| `parentexpensealloc` | select | Created From Allocation | no | no |  |  |

## Sublist `line` (Lines)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `class` | select | Class | no |  | `purpose`: `line_class`; `allowed_values_listed_in`: `class_records`; `ui_effect`: `new_option_creates_record` |
| `department` | select | Department | no |  | `purpose`: `line_department`; `allowed_values_listed_in`: `department_records`; `ui_effect`: `new_option_creates_record`; `help_text_shared_with`: `journalentry.department` |
| `location` | select | Location | no |  | `purpose`: `line_location`; `allowed_values_listed_in`: `location_records`; `ui_effect`: `new_option_creates_record`; `help_text_shared_with`: `vendorbill.location`, `vendorcredit.location` |
| `account` | select | Account | yes |  |  |
| `debit` | currency | Amount | yes |  |  |
| `entity` | select | Name | no |  |  |
| `line` | integer |  | no |  |  |
| `lineunit` | select | Units | yes |  |  |
| `memo` | text | Memo | no |  |  |
| `previewdebit` | text | Amount (Actual) | no |  |  |
| `schedulenum` | select | Schedule No. | no |  |  |

## Tabs

| Tab ID | Label |
|---|---|
| `cmmnctntab` | Communication |
| `custom` | Custom |
| `lines` | Lines |
| `rlrcdstab` | Related Records |
| `s_sysinfo` | System Information |

## Suspected source errors

> [!SOURCE-ERROR] `subsidiary_sample_sentence_garbled`
<!-- source-error: record:statisticaljournalentry | - | subsidiary_sample_sentence_garbled -->
