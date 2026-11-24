# Customer Payment (`customerpayment`)
<!-- id: record:customerpayment -->

| Fact | Value |
|---|---|
| Internal ID | `customerpayment` |
| Category | transaction |
| Client scripts | no |
| Server scripts | yes |
| Search data via | Transaction |
| Tags | `custom_fields_supported`, `no_copy`, `no_create` |

## Record facts

- `initialization_default_value`: `entity`, `entity_internal_id` (refs `N/record.create`, `N/record.copy`, `N/record.transform`, `N/record.load`)
- `script_types`: `server`
- `not_supported`: `create`, `copy`
- `unreadable_in_before_submit_for_external_roles`: `ccnumber`
- `field_stored_only_when`: `isrecurringpayment:paymentmethod_credit_card`
- `list_sublists`: `deposits`
- `ui_quirk`: `apply_sublist_shown_on_invoice_subtab`
- `purpose`: `record_payment_applied_to_invoice_or_cash_sale`
- `caveat`: `due_field_label_may_show_amount_remaining`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `account` | select |  | no | no |  | `purpose`: `deposit_account`; `use_when`: `non_posting_account_for_unapproved_card`; `value_examples`: `approved_card_to_checking_unapproved_to_non_posting` |
| `applied` | currency | Applied | no | no |  | `purpose`: `applied_total`; `value_source`: `sum_of_apply_lines` |
| `aracct` | select | A/R Account | no | no |  | `purpose`: `receivables_account`; `allowed_values_listed_in`: `accounts_receivable_accounts`; `ui_effect`: `new_option_creates_record` |
| `authcode` | text | Auth. Code | no | no |  | `purpose`: `card_authorization_code`; `value_source`: `set_on_approval` (when `netsuite_merchant_account`); `use_when`: `card_validated_outside_netsuite` |
| `autoapply` | checkbox | Auto Apply | no | no |  | `purpose`: `auto_apply`; `effect`: `applies_to_top_receivable_first`; `default_behavior`: `unapplied` (when `cleared`); `caveat`: `existing_credits_and_deposits_not_applied`; `effect`: `setting_remembered_for_next_payment` |
| `balance` | currency | Balance | no | no |  | `purpose`: `customer_balance`; `value_source`: `system_displayed` |
| `ccapproved` | checkbox | CC Approved | no | no |  | `purpose`: `card_approved_externally`; `use_when`: `card_validated_outside_netsuite`; `value_source`: `set_on_approval` (when `ims_merchant_account`); `value_examples`: `card_swipe_terminal` |
| `ccavsstreetmatch` | select | AVS Street Match | no | no |  | `purpose`: `avs_result_code`; `allowed_values`: `Y`, `N`; `help_text_shared_with`: `customerpayment.ccavszipmatch` |
| `ccavszipmatch` | select | AVS Zip Match | no | no |  | `purpose`: `avs_result_code`; `allowed_values`: `Y`, `N`; `help_text_shared_with`: `customerpayment.ccavsstreetmatch` |
| `ccexpiredate` | ccexpdate | Expires (MM/YYYY) | no | no |  | `purpose`: `card_expiration_date`; `format`: `MM/YYYY` |
| `ccname` | text | Name on Card | no | no |  | `purpose`: `cardholder_name`; `default_behavior`: `customer_default_card_holder`; `recommendation`: `fix_value_on_customer_record` |
| `ccnumber` | ccnumber | Credit Card # | no | no |  | `purpose`: `card_number`; `format`: `digits_without_spaces`; `caveat`: `visible_only_to_merchant_and_customer` |
| `ccsecuritycode` | text | CSC | no | no |  | `purpose`: `card_security_code`; `format`: `three_digits_or_four_for_amex` |
| `ccsecuritycodematch` | select | CSC Match | no | no |  | `purpose`: `csc_result_code`; `allowed_values`: `Y`, `N`, `X`; `option_meanings`: `y:matched`, `n:not_matched`, `x:not_supported_or_no_response`; `value_source`: `verisign_response` |
| `ccstreet` | text | Card Street | no | no |  | `purpose`: `card_billing_street`; `value_source`: `customer_billing_address_street`; `recommendation`: `fix_value_on_customer_record` |
| `cczipcode` | text | Card Zip Code | no | no |  | `purpose`: `card_billing_postal_code`; `value_source`: `customer_billing_address_postal_code`; `recommendation`: `fix_value_on_customer_record` |
| `chargeit` | checkbox | Charge Credit Card | no | no |  | `purpose`: `charge_selected_card` |
| `checknum` | text | Check # | no | no |  | `purpose`: `check_number`; `displayed_on`: `registers`; `applies_when`: `payment_by_check` |
| `class` | select | Class | no | no |  | `purpose`: `transaction_class`; `allowed_values_listed_in`: `class_records`; `ui_effect`: `new_option_creates_record` |
| `consolidatebalance` | currency | Consolidated Balance | no | no |  | `purpose`: `consolidated_open_invoices_total`; `value_scope`: `customer_subcustomer_hierarchy` |
| `creditcard` | select | Credit Card Select | no | no |  | `purpose`: `stored_card_for_payment`; `allowed_values_listed_in`: `customer_stored_cards`; `ui_effect`: `new_option_creates_record` |
| `creditcardprocessor` | select | Credit Card Processor | no | no |  | `purpose`: `card_processor`; `allowed_values_listed_in`: `payment_processing_profiles` |
| `currency` | select | Currency | yes | no |  | `purpose`: `payment_currency`; `default_behavior`: `customer_primary_currency`; `requires_one_of`: `multiple_currencies`, `multi_currency_customers` (when `choosing_transaction_currency`); `effect`: `filters_invoices_credits_and_deposits`; `constraint`: `parent_pays_child_invoices_in_parent_currencies` |
| `currencyname` | text | Currency | no | no |  | `purpose`: `customer_currency_name`; `effect`: `all_amounts_in_this_currency` |
| `customer` | select | Customer | yes | no |  | `purpose`: `payment_customer`; `recommendation`: `select_project_not_parent`; `allowed_values_listed_in`: `customer_and_project_records` |
| `customercode` | text | Customer Code | no | no |  | `purpose`: `card_customer_code`; `shown_only_when`: `display_customer_code_web_site_preference`; `requires`: `level_2_card_processing`; `use_when`: `tracking_card_use_per_department` |
| `debitcardissueno` | text | Issue No. | no | no |  | `purpose`: `debit_card_issue_number` |
| `department` | select | Department | no | no |  | `purpose`: `transaction_department`; `allowed_values_listed_in`: `department_records`; `ui_effect`: `new_option_creates_record` |
| `exchangerate` | currency2 | Exchange Rate | yes | no |  | `purpose`: `exchange_rate`; `can`: `update_currency_record_rate` |
| `ignoreavs` | checkbox | Ignore AVS | no | no |  | `purpose`: `ignore_avs_result`; `true_means`: `payment_processed_despite_avs_mismatch` |
| `ignorecsc` | checkbox | Ignore CSC | no | no |  | `purpose`: `ignore_csc_result`; `true_means`: `payment_processed_despite_csc_failure` |
| `isrecurringpayment` | checkbox | Recurring Payment | no | no |  | `purpose`: `recurring_payment`; `effect`: `processes_after_card_expiry`; `use_when`: `billing_schedule_or_memorized_orders` |
| `location` | select | Location | no | no |  | `purpose`: `transaction_location`; `allowed_values_listed_in`: `location_records`; `ui_effect`: `new_option_creates_record` |
| `memo` | text | Memo | no | yes |  | `purpose`: `memo`; `displayed_on`: `registers`, `customer_center`; `value_kind`: `free_text` |
| `payment` | poscurrency | Payment Amount | yes | no |  | `purpose`: `payment_amount` |
| `paymentmethod` | select | Payment Method | no | no |  | `purpose`: `payment_method`; `allowed_values_listed_in`: `accounting_list_payment_method` |
| `pending` | currency | Pending | no | no |  | `purpose`: `pending_card_payments_total`; `value_source`: `sum_of_unapproved_card_payments` |
| `pnrefnum` | text | P/N Ref. | no | no |  | `purpose`: `verisign_authentication_code`; `value_source`: `set_on_approval` |
| `postingperiod` | select | Posting Period | no | no |  | `purpose`: `posting_period`; `cannot`: `post_to_closed_period` |
| `subsidiary` | select | Subsidiary | no | no |  | `purpose`: `subsidiary`; `value_source`: `system_displayed` |
| `total` | currency | To Apply | no | no |  | `purpose`: `total`; `value_source`: `payment_plus_applied_credits`; `help_text_shared_with`: `customerpayment.apply.total`, `customerpayment.credit.total`, `customerpayment.deposit.total` |
| `trandate` | date | Date | yes | no |  | `purpose`: `posting_date`; `default_behavior`: `current_date`; `displayed_on`: `registers` |
| `tranid` | text | Payment # | no | no |  | `purpose`: `document_number`; `value_source`: `system_generated` |
| `unapplied` | currency | Unapplied | no | no |  | `purpose`: `unapplied_amount`; `value_source`: `available_minus_applied` |
| `undepfunds` | radio | Undep. Funds | no | no |  | `purpose`: `undeposited_funds_or_account`; `use_when`: `undeposited_until_bank_deposit` |
| `validfrom` | ccvalidfrom | Valid From / Start Date (MM/YYYY) | no | no |  | `purpose`: `card_valid_from` |
| `allowemptycards` | checkbox | Credits | no | no |  |  |
| `cchold` | text | Detail | no | no |  |  |
| `ccholdetails` | textarea | Detail | no | no |  |  |
| `cciavsmatch` | select | IAVS Match | no | no |  |  |
| `ccispurchasecardbin` | checkbox | Purchase Card BIN | no | no |  |  |
| `ccprocessoraccount` | text | Credit Card Processing Profile | no | no |  |  |
| `checknumber` | text | Check # | no | no |  |  |
| `cleared` | checkbox | Cleared | no |  |  |  |
| `cleareddate` | date | Date Cleared | no |  |  |  |
| `createddate` | datetime |  | no | no |  |  |
| `currencysymbol` | text | Currency Symbol | no | no |  |  |
| `customform` | select | Custom Form | yes | no |  |  |
| `dynamicdescriptor` | text | Soft Descriptor | no | no |  |  |
| `entitynexus` | select | Nexus | no | no |  |  |
| `externalid` | text | ExternalId | no | no |  |  |
| `handlingmode` | select | Handling Mode | no | no |  |  |
| `inputauthcode` | text | Input Auth. Code | no | no |  |  |
| `inputpnrefnum` | text |  | no | no |  |  |
| `inputreferencecode` | text | Input P/N Ref. | yes | no |  |  |
| `isbasecurrency` | checkbox | Base Currency | no | no |  |  |
| `ispurchasecard` | text | Override Hold | no | no |  |  |
| `lastmodifieddate` | datetime |  | no | no |  |  |
| `nexus` | select | Nexus | no | no |  |  |
| `outputauthcode` | text | Auth. Code | no | no |  |  |
| `outputreferencecode` | text | P/N Ref. | no | no |  |  |
| `overridehold` | checkbox | Override Hold | no | no |  |  |
| `overrideholdchecked` | checkbox | Override Hold | no | no |  |  |
| `paymentcardcsc` | text | CSC | no | no |  |  |
| `paymenteventdate` | datetime | Override Hold | no | no |  |  |
| `paymenteventholdreason` | select | Reason | no | no |  |  |
| `paymenteventpurchasedatasent` | text | Override Hold | no | no |  |  |
| `paymenteventresult` | select | Status | no | no |  |  |
| `paymenteventtype` | text | Override Hold | no | no |  |  |
| `paymenteventupdatedby` | text | Override Hold | no | no |  |  |
| `paymentoperation` | select | Payment Operation | no | no |  |  |
| `paymentoption` | select | Payment Option | no | no |  |  |
| `paymentprocessingprofile` | select | Payment Processing Profile | no | no |  |  |
| `softdescriptor` | text | Soft Descriptor | no | no |  |  |
| `status` | text | Status | no | no |  |  |
| `statusref` | text |  | no | no |  |  |
| `threedstatuscode` | text | Payer Authentication Status | no | no |  |  |
| `tobeemailed` | checkbox |  | no | no |  |  |

## Sublist `apply` (Invoices)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `total` | currency | Orig. Amt. | no |  | `purpose`: `total`; `value_source`: `payment_plus_applied_credits`; `help_text_shared_with`: `customerpayment.total` |
| `amount` | currency | Payment | no |  |  |
| `apply` | checkbox | Apply | no |  |  |
| `applydate` | date | Date | no |  |  |
| `createdfrom` | text |  | no |  |  |
| `disc` | poscurrency | Disc. Taken | no |  |  |
| `discamt` | currency | Disc. Avail. | no |  |  |
| `discdate` | date | Disc. Date | no |  |  |
| `doc` | text |  | no |  |  |
| `due` | currency | Amt. Due | no |  |  |
| `duedate` | date |  | no |  |  |
| `internalid` | text | Hidden | no |  |  |
| `job` | integer |  | no |  |  |
| `line` | integer |  | no |  |  |
| `refnum` | text | Ref No. | no |  |  |
| `url` | text |  | no |  |  |

## Sublist `credit` (Credits)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `total` | currency | Orig. Amt. | no |  | `purpose`: `total`; `value_source`: `payment_plus_applied_credits`; `help_text_shared_with`: `customerpayment.total` |
| `amount` | currency | Credit | no |  |  |
| `apply` | checkbox | Apply | no |  |  |
| `createdfrom` | text |  | no |  |  |
| `creditdate` | date | Date | no |  |  |
| `doc` | text |  | no |  |  |
| `due` | currency | Amount Remaining | no |  |  |
| `duedate` | date |  | no |  |  |
| `internalid` | text | Hidden | no |  |  |
| `line` | integer |  | no |  |  |
| `refnum` | text | Ref No. | no |  |  |
| `url` | text |  | no |  |  |

## Sublist `deposit` (Deposits)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `total` | currency | Orig. Amt. | no |  | `purpose`: `total`; `value_source`: `payment_plus_applied_credits`; `help_text_shared_with`: `customerpayment.total` |
| `amount` | currency | Payment | no |  |  |
| `apply` | checkbox | Apply | no |  |  |
| `currency` | text |  | no |  |  |
| `depositdate` | date | Date | no |  |  |
| `doc` | text |  | no |  |  |
| `remaining` | currency | Amount Remaining | no |  |  |
| `url` | text |  | no |  |  |

## Sublist `accountingbookdetail` (Accounting Books)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `accountingbook` | select | Secondary Book | no |  |  |
| `exchangerate` | currency2 | Exchange Rate | yes |  |  |

## Tabs

| Tab ID | Label |
|---|---|
| `applications` | Applications |
| `custom` | Custom |
| `history` | History |
| `payment` | Payment |
| `workflow` | Workflow |
