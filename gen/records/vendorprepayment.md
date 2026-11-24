# Vendor Prepayment (`vendorprepayment`)
<!-- id: record:vendorprepayment -->

| Fact | Value |
|---|---|
| Internal ID | `vendorprepayment` |
| Category | transaction |
| Client scripts | yes |
| Server scripts | yes |
| Searchable | yes |
| Search data via | Transaction |
| Tags | `supports_create`, `supports_delete`, `supports_read`, `supports_search`, `supports_update` |

## Record facts

- `script_types`: `client`, `server`
- `requires_permission`: `vendor_prepayment`
- `standalone_or_linked_to`: `purchaseorder`
- `purpose`: `pay_vendor_before_po_accepted`
- `effect`: `posts_without_offsetting_ap_until_applied`
- `value_examples`: `payee_94_prepayment_500`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `account` | select | Account | yes |  |  | `purpose`: `credited_account`; `constraint`: `account_currency_equals_transaction_currency` |
| `balance` | currency | Balance | no |  |  | `purpose`: `account_available_balance`; `value_source`: `system_displayed` |
| `class` | select | Class | no |  |  | `purpose`: `class`; `constraint`: `only_values_of_selected_subsidiary` |
| `currency` | select | Currency | yes |  |  | `purpose`: `currency`; `doc_inconsistency`: `stray_html_tag_in_help` |
| `department` | select | Department | no |  |  | `purpose`: `department`; `constraint`: `only_values_of_selected_subsidiary` |
| `entity` | select | Payee | yes |  |  | `purpose`: `payee` |
| `exchangerate` | currency2 | Exchange Rate | yes |  |  | `purpose`: `exchange_rate`; `value_source`: `payee_currency`; `cannot`: `change` |
| `location` | select | Location | no |  |  | `purpose`: `location`; `constraint`: `only_values_of_selected_subsidiary`; `doc_inconsistency`: `location_help_says_departments` |
| `memo` | text | Memo | no |  |  | `purpose`: `memo`; `displayed_on`: `prepayments_list`, `reports`; `supported_in`: `workbooks` |
| `payment` | poscurrency | Payment Amount | yes |  |  | `purpose`: `prepayment_amount`; `effect`: `credits_bank_debits_prepayment_account` |
| `postingperiod` | select | Posting Period | no |  |  | `purpose`: `posting_period`; `cannot`: `post_to_closed_period` |
| `prepaymentaccount` | select | Prepayment Account | no |  |  | `purpose`: `prepayment_account`; `default_behavior`: `configured_default_prepayment_account`; `configured_by`: `company_or_subsidiary_prepayment_account` |
| `printvoucher` | checkbox | Voucher | no |  |  | `purpose`: `print_voucher`; `format`: `pdf` |
| `purchaseorder` | select | Purchase Order | no |  |  | `purpose`: `related_purchase_order`; `value_kind`: `po_document_number` |
| `subsidiary` | select | Subsidiary | no |  |  | `purpose`: `subsidiary`; `can`: `change_from_default` |
| `tobeprinted` | checkbox | To Be Printed | no |  |  | `purpose`: `print_check_later`; `format`: `pdf` |
| `trandate` | date | Date | yes |  |  | `purpose`: `posting_date`; `default_behavior`: `current_date` |
| `tranid` | text | Check # | no |  |  | `purpose`: `check_number`; `default_behavior`: `next_number_for_bank_account`; `read_only_when`: `credit_card_account` |
| `transactionnumber` | text | Transaction Number | no |  |  | `purpose`: `transaction_number`; `default_value`: `To be generated` (when `new_transaction`); `value_source`: `system_generated_on_save` |
| `cleared` | checkbox | Cleared | no |  |  |  |
| `cleareddate` | date | Date Cleared | no |  |  |  |
| `createddate` | datetime | Date Created | no |  |  |  |
| `currencyname` | text | Currency | no |  |  |  |
| `currencysymbol` | text | Currency Symbol | no |  |  |  |
| `customform` | select | Custom Form | no |  |  |  |
| `externalid` | text | External ID | no |  |  |  |
| `isbasecurrency` | checkbox | Base Currency | no |  |  |  |
| `lastmodifieddate` | datetime | Date Last Modified | no |  |  |  |
| `status` | select | Status | no |  |  |  |

## Sublist `accountingbookdetail` (Accounting Books)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `accountingbook` | select | Secondary Book | no |  |  |
| `exchangerate` | currency2 | Exchange Rate | yes |  |  |

## Suspected source errors

> [!SOURCE-ERROR] `stray_html_tag_in_help` (on `currency`)
<!-- source-error: record:vendorprepayment | currency | stray_html_tag_in_help -->

> [!SOURCE-ERROR] `location_help_says_departments` (on `location`)
<!-- source-error: record:vendorprepayment | location | location_help_says_departments -->
