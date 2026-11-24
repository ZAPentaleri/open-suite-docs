# Vendor Payment (`vendorpayment`)
<!-- id: record:vendorpayment -->

| Fact | Value |
|---|---|
| Internal ID | `vendorpayment` |
| Category | transaction |
| Client scripts | yes |
| Server scripts | yes |
| Searchable | yes |
| Search data via | Transaction |
| Tags | `custom_fields_supported`, `scripting_full` |

## Record facts

- `record_actions`: `confirm:in_transit_to_confirmed`, `decline:in_transit_to_declined`
- `governance_note`: `confirm:10`, `decline:10`
- `action_params`: `confirm:exchangerate`, `confirm:confirmationdate`, `confirm:postingperiod`, `confirm:clearpayment`
- `ui_location`: `confirm_and_decline_buttons`
- `requires`: `recordid_parameter`
- `initialization_default_value`: `entity`, `entity_internal_id` (refs `N/record.create`, `N/record.copy`, `N/record.transform`, `N/record.load`)
- `script_types`: `client`, `server`
- `script_functions`: `create`, `update`, `copy`, `delete`, `search`
- `list_sublists`: `credits`
- `record_actions`: `confirm`, `decline`
- `purpose`: `pay_vendor_bills_from_payables`
- `effect`: `posts_expense_and_reduces_accounts_payable`

## Body fields

| Field ID | Type | Label | Required | submitField | Read-only | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `account` | select | Account | yes | no |  |  | `purpose`: `payment_bank_account` |
| `apacct` | select | A/P Account | no | no |  |  | `purpose`: `payables_account`; `ui_effect`: `new_option_creates_record`; `help_text_shared_with`: `vendorcredit.account` |
| `approvalstatus` | select | Approval Status | no | no |  |  | `purpose`: `payment_approval_status`; `allowed_values`: `Approved`, `Pending Approval`, `Rejected`; `option_meanings`: `rejected:cannot_be_processed` |
| `availablebalance` | currency | Available Balance | no | no |  |  | `purpose`: `available_balance`; `value_source`: `balance_minus_in_transit_payments` |
| `balance` | currency | Balance | no | no |  |  | `purpose`: `bank_balance`; `value_source`: `system_displayed` |
| `billpay` | checkbox | Bill Pay | no | no |  |  | `purpose`: `send_payment_online`; `requires_feature`: `Online Bill Pay` [`real_name`]; `caveat`: `limited_transactions_included` |
| `class` | select | Class | no | no |  |  | `purpose`: `class`; `ui_effect`: `new_option_creates_record` |
| `currency` | select | Currency | yes | no |  |  | `purpose`: `payment_currency` |
| `currencyname` | text | Currency | no | no |  |  | `purpose`: `vendor_currency_name`; `effect`: `all_amounts_in_this_currency` |
| `customform` | select | Custom Form | yes | no |  |  | `purpose`: `entry_form` |
| `department` | select | Department | no | no |  |  | `purpose`: `department`; `ui_effect`: `new_option_creates_record` |
| `entity` | select | Payee | yes | no |  |  | `purpose`: `payee`; `accepts_forms`: `vendor`, `employee`; `default_behavior`: `vendor_primary_subsidiary` [`subsidiary`] |
| `exchangerate` | currency2 | Exchange Rate | yes | no |  |  | `purpose`: `exchange_rate`; `can`: `update_currency_record_rate` |
| `isintransitpayment` | checkbox | In-Transit | no | no |  |  | `purpose`: `in_transit_payment`; `configured_by`: `use_in_transit_vendor_payments_by_default`; `editable_only_when`: `creating_payment`; `cannot`: `change_after_save` |
| `location` | select | Location | no | no |  |  | `purpose`: `transaction_location`; `allowed_values_listed_in`: `location_records`; `ui_effect`: `new_option_creates_record` |
| `memo` | text | Memo | no | yes |  |  | `purpose`: `memo`; `displayed_on`: `check_memo_line` |
| `nextapprover` | select | Next Approver | no | no |  |  | `purpose`: `next_approver`; `value_source`: `employee_approver_or_supervisor`; `caveat`: `either_supervisor_or_approver_may_approve` |
| `postingperiod` | select | Posting Period | no | no |  |  | `purpose`: `posting_period`; `cannot`: `post_to_closed_period`; `value_source`: `set_on_approval` (when `subject_to_approval`) |
| `printvoucher` | checkbox | Voucher | no | no |  |  | `purpose`: `print_voucher`; `true_means`: `voucher_queued_for_printing` |
| `subsidiary` | select | Subsidiary | no | no | yes |  | `purpose`: `subsidiary`; `requires_feature`: `OneWorld` [`real_name`] |
| `toach` | checkbox | To ACH | no | no |  |  | `purpose`: `send_via_ach`; `requires`: `ach_processing_and_vendor_ach_set_up` |
| `tobeprinted` | checkbox | To Be Printed | no | yes |  |  | `purpose`: `print_later`; `true_means`: `queued_for_check_printing` |
| `total` | currency | Amount | no | no |  |  | `purpose`: `payment_total`; `effect`: `updated_from_lines` |
| `trandate` | date | Date | yes | no |  |  | `purpose`: `posting_date`; `default_behavior`: `current_date`; `displayed_on`: `registers` |
| `tranid` | text | Check # | no | no |  |  | `purpose`: `check_number`; `default_behavior`: `highest_number_plus_one` (when `printing_now`); `caveat`: `numbers_chosen_at_print` (when `tobeprinted`) |
| `transactionnumber` | text | Transaction Number | no | no |  |  | `purpose`: `transaction_number`; `value_source`: `system_generated` |
| `cleared` | checkbox | Cleared | no |  |  |  |  |
| `cleareddate` | date | Date Cleared | no |  |  |  |  |
| `createddate` | datetime |  | no | no |  |  |  |
| `currencysymbol` | text | Currency Symbol | no | no |  |  |  |
| `entitynexus` | select | Nexus | no | no |  |  |  |
| `externalid` | text | ExternalId | no | no |  |  |  |
| `isbasecurrency` | checkbox | Base Currency | no | no |  |  |  |
| `lastmodifieddate` | datetime |  | no | no |  |  |  |
| `nexus` | select | Nexus | no | no |  |  |  |
| `payeeaddress` | summary |  | no | no |  |  |  |
| `payeeaddresslist` | select | Address To Select | no | no |  |  |  |
| `status` | text | Status | no | no |  |  |  |
| `statusref` | text |  | no | no |  |  |  |
| `tobeemailed` | checkbox |  | no | no |  |  |  |
| `unapplied` | currency | Unapplied | no | no |  |  |  |

## Sublist `accountingbookdetail` (Accounting Books)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `accountingbook` | select | Secondary Book | no |  |  |
| `exchangerate` | currency2 | Exchange Rate | yes |  |  |

## Sublist `apply` (Apply)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `amount` | currency | Payment | no |  |  |
| `apply` | checkbox | Apply | no |  |  |
| `applydate` | date | Date Due | no |  |  |
| `createdfrom` | text |  | no |  |  |
| `disc` | poscurrency | Disc. Taken | no |  |  |
| `discamt` | currency | Disc. Avail. | no |  |  |
| `discdate` | date | Disc. Date | no |  |  |
| `doc` | text |  | no |  |  |
| `due` | currency | Amt. Due | no |  |  |
| `duedate` | date |  | no |  |  |
| `internalid` | text | Hidden | no |  |  |
| `line` | integer |  | no |  |  |
| `refnum` | text | Ref No. | no |  |  |
| `total` | currency | Orig. Amt. | no |  |  |
| `url` | text |  | no |  |  |

## Tabs

| Tab ID | Label |
|---|---|
| `applications` | Applications |
| `custom` | Custom |
| `history` | History |
| `workflow` | Workflow |
