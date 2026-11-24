# Deposit (`deposit`)
<!-- id: record:deposit -->

| Fact | Value |
|---|---|
| Internal ID | `deposit` |
| Category | transaction |
| Client scripts | yes |
| Server scripts | yes |
| Search data via | Transaction |
| Tags | `custom_fields_supported`, `no_copy`, `no_dynamic_mode` |

## Record facts

- `script_types`: `client`, `server`
- `script_functions`: `create`, `update`, `delete`, `search`
- `user_events`: `beforeLoad`, `beforeSubmit`, `afterSubmit`
- `user_events`: `beforeLoad`, `beforeSubmit`, `afterSubmit`
- `script_functions`: `create`, `update`, `delete`, `search`
- `not_supported`: `copy`
- `requires`: `one_line_in_payments_other_deposits_or_cash_back`
- `not_scriptable`: `accounting_book_detail_sublist` (when `multi_book_accounting`)
- `raises_error`: `RCRD_HAS_BEEN_CHANGED` (when `payment_line_id_not_set`)
- `init_params`: `disablepaymentfilters:create_and_load`, `deposits:create_only`
- `true_means`: `user_payment_filters_ignored` [`disablepaymentfilters`]
- `purpose`: `adjust_account_balance`
- `ui_location`: `transactions_bank_make_deposits`
- `caveat`: `journal_entry_payment_lines_may_have_nonzero_line_id`
- `false_means`: `user_payment_filters_applied_fewer_payments_loaded` [`disablepaymentfilters`]
- `default_value`: `false` [`disablepaymentfilters`]
- `recommendation`: `keep_filters_enabled_for_many_payments_performance`
- `displayed_on`: `deposits_payments_subtab` [`user_filter_preferences`]
- `value_examples`: `deposits_param_payment_ids_818_819`
- `initialization_default_value`: `disablepaymentfilters`, `disablepaymentfilters` (refs `N/record.create`, `N/record.copy`, `N/record.transform`, `N/record.load`)

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `account` | select | Account | yes | no |  | `purpose`: `bank_account`; `editable_only_when`: `edit_mode`; `allowed_values_listed_in`: `bank_accounts`; `ui_effect`: `new_option_creates_record` |
| `class` | select | Class | no | no |  | `purpose`: `class`; `editable_only_when`: `edit_mode`; `allowed_values_listed_in`: `class_records`; `ui_effect`: `new_option_creates_record` |
| `creditcardprocessor` | select | Credit Card Processing Profile | no | no |  | `purpose`: `card_processing_profile_filter`; `allowed_values_listed_in`: `payment_processing_profiles`; `effect`: `filters_payments_list` |
| `currency` | text | Currency | yes | no |  | `purpose`: `deposit_currency`; `default_behavior`: `deposit_account_currency` |
| `customform` | select | Custom Form | yes | no |  | `purpose`: `deposit_form`; `allowed_values_listed_in`: `standard_and_custom_deposit_forms` |
| `department` | select | Department | no | no |  | `purpose`: `department`; `editable_only_when`: `edit_mode`; `allowed_values_listed_in`: `department_records`; `ui_effect`: `new_option_creates_record` |
| `exchangerate` | currency2 | Exchange Rate | yes | no |  | `purpose`: `exchange_rate`; `read_only_when`: `account_currency_is_subsidiary_base` |
| `location` | select | Location | no | no |  | `purpose`: `location`; `editable_only_when`: `edit_mode`; `allowed_values_listed_in`: `location_records`; `ui_effect`: `new_option_creates_record` |
| `memo` | text | Memo | no | yes |  | `purpose`: `memo`; `displayed_on`: `reports` |
| `postingperiod` | select | Posting Period | no | no |  | `purpose`: `posting_period`; `applies_when`: `accounting_periods_used`; `cannot`: `post_to_closed_period` |
| `subsidiary` | select | Subsidiary | no | no |  | `purpose`: `subsidiary`; `default_behavior`: `bank_account_subsidiary` (when `oneworld`) |
| `tobeprinted` | checkbox | To Be Printed | no | yes |  | `purpose`: `print_deposit_slip_later`; `true_means`: `queued_for_batch_printing`; `use_when`: `printing_several_slips_together` |
| `total` | currency | Amount | no | no |  | `purpose`: `deposit_total`; `value_source`: `payments_other_deposits_and_cash_back` |
| `trandate` | date | Date | yes | no |  | `purpose`: `posting_date`; `default_behavior`: `current_date`; `displayed_on`: `registers` |
| `tranid` | text | Deposit # | no | no |  | `purpose`: `deposit_number`; `value_source`: `system_generated` |
| `cleared` | checkbox | Cleared | no |  |  |  |
| `cleareddate` | date | Date Cleared | no |  |  |  |
| `createddate` | datetime |  | no | no |  |  |
| `currencyname` | text | Currency | no | no |  |  |
| `externalid` | text | ExternalId | no | no |  |  |
| `isbasecurrency` | checkbox | Base Currency | no | no |  |  |
| `lastmodifieddate` | datetime |  | no | no |  |  |
| `taxperiod` | text |  | no | no |  |  |
| `updatecurrency` | text | Update Currency | no | no |  |  |

## Sublist `accountingbookdetail` (Accounting Books)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `accountingbook` | select | Secondary Book | no |  |  |
| `exchangerate` | currency2 | Exchange Rate | yes |  |  |

## Sublist `cashback` (Cash Back)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `account` | select | Account | yes |  |  |
| `amount` | currency | Amount | yes |  |  |
| `class` | select | Class | no |  |  |
| `department` | select | Department | no |  |  |
| `line` | text |  | no |  |  |
| `location` | select | Location | no |  |  |
| `memo` | text | Memo | no |  |  |

## Sublist `other` (Other Deposits)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `account` | select | Account | yes |  |  |
| `amount` | currency | Amount | yes |  |  |
| `class` | select | Class | no |  |  |
| `department` | select | Department | no |  |  |
| `entity` | select | Name | no |  |  |
| `line` | text |  | no |  |  |
| `location` | select | Location | no |  |  |
| `memo` | text | Memo | no |  |  |
| `paymentmethod` | select | Payment Method | no |  |  |
| `refnum` | text | Number | no |  |  |

## Sublist `payment` (Payments)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `class` | select | Class | no |  |  |
| `currency` | select | Currency | no |  |  |
| `department` | select | Department | no |  |  |
| `deposit` | checkbox | Deposit | no |  |  |
| `docdate` | date | Date | no |  |  |
| `docnumber` | text | Number | no |  |  |
| `entity` | select | From | no |  |  |
| `id` | text |  | no |  |  |
| `lineid` | integer |  | no |  |  |
| `location` | select | Location | no |  |  |
| `memo` | text | Memo | no |  |  |
| `paymentamount` | currency | Amount (null) | no |  |  |
| `paymentmethod` | select | Pmt. Meth. | no |  |  |
| `pmtcurrencyprecision` | text | Currency Precision | no |  |  |
| `refnum` | text | Ref No. | no |  |  |
| `transactionamount` | currency | Payment Amount | no |  |  |
| `type` | select | Type | no |  |  |

## Tabs

| Tab ID | Label |
|---|---|
| `custom` | Custom |
| `history` | History |
| `items` | Items |
| `multibook` | Accounting Books |
| `workflow` | Workflow |
