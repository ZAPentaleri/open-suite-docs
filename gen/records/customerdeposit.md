# Customer Deposit (`customerdeposit`)
<!-- id: record:customerdeposit -->

| Fact | Value |
|---|---|
| Internal ID | `customerdeposit` |
| Category | transaction |
| Client scripts | yes |
| Server scripts | yes |
| Searchable | yes |
| Search data via | Transaction |
| Tags | `custom_fields_supported`, `scripting_full` |

## Record facts

- `script_types`: `client`, `server`
- `script_functions`: `create`, `update`, `copy`, `delete`, `search`
- `unreadable_in_before_submit_for_external_roles`: `ccnumber`
- `field_stored_only_when`: `isrecurringpayment:paymentmethod_credit_card`
- `purpose`: `advance_payment_for_order`
- `effect`: `gl_liability_until_delivery_no_ar_effect`
- `effect`: `applied_against_invoice_after_order_filled`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `authcode` | text | Auth. Code | no | no |  | `purpose`: `card_authorization_code`; `value_source`: `set_on_approval` (when `netsuite_merchant_account`); `use_when`: `card_validated_outside_netsuite` |
| `ccapproved` | checkbox | CC Approved | no | no |  | `purpose`: `card_approved_externally`; `use_when`: `card_processed_outside_netsuite`; `value_source`: `set_on_approval` (when `netsuite_merchant_account`); `value_examples`: `card_swipe_terminal` |
| `ccavsstreetmatch` | select | AVS Street Match | no | no |  | `purpose`: `avs_result_code`; `allowed_values`: `Y`, `N`, `X`; `option_meanings`: `y:matched`, `n:not_matched`, `x:not_supported_or_no_response`; `value_source`: `verisign_response`; `help_text_shared_with`: `cashsale.ccavszipmatch`, `customerdeposit.ccavszipmatch` |
| `ccavszipmatch` | select | AVS Zip Match | no | no |  | `purpose`: `avs_result_code`; `allowed_values`: `Y`, `N`, `X`; `option_meanings`: `y:matched`, `n:not_matched`, `x:not_supported_or_no_response`; `value_source`: `verisign_response`; `help_text_shared_with`: `cashsale.ccavsstreetmatch`, `customerdeposit.ccavsstreetmatch` |
| `ccexpiredate` | ccexpdate | Expires (MM/YYYY) | no | no |  | `purpose`: `card_expiration_date`; `format`: `MM/YYYY` |
| `ccname` | text | Name on Card | no | no |  | `purpose`: `cardholder_name`; `default_behavior`: `customer_default_card_holder`; `recommendation`: `fix_value_on_customer_record` |
| `ccnumber` | ccnumber | Credit Card # | no | no |  | `purpose`: `card_number`; `format`: `digits_without_spaces` |
| `ccsecuritycode` | text | CSC | no | no |  | `purpose`: `card_security_code`; `format`: `three_digits` |
| `ccsecuritycodematch` | select | CSC Match | no | no |  | `purpose`: `csc_result_code`; `allowed_values`: `Y`, `N`, `X`; `option_meanings`: `y:matched`, `n:not_matched`, `x:not_supported_or_no_response`; `value_source`: `verisign_response` |
| `ccstreet` | text | Card Street | no | no |  | `purpose`: `card_billing_street`; `value_source`: `customer_billing_address_street`; `recommendation`: `fix_value_on_customer_record` |
| `cczipcode` | text | Card Zip Code | no | no |  | `purpose`: `card_billing_postal_code`; `value_source`: `customer_billing_address_postal_code`; `recommendation`: `fix_value_on_customer_record` |
| `chargeit` | checkbox | Charge Credit Card | no | no |  | `purpose`: `charge_card_online`; `effect`: `funds_move_if_accepted`; `requires`: `netsuite_merchant_account` |
| `checknum` | text | Check # | no | no |  | `purpose`: `check_number` |
| `class` | select | Class | no | no |  | `purpose`: `transaction_class`; `allowed_values_listed_in`: `class_records`; `ui_effect`: `new_option_creates_record` |
| `creditcard` | select | Credit Card Select | no | no |  | `purpose`: `stored_card_for_deposit`; `allowed_values_listed_in`: `customer_stored_cards`; `ui_effect`: `new_option_creates_record` |
| `creditcardprocessor` | select | Credit Card Processor | no | no |  | `purpose`: `card_processor`; `allowed_values_listed_in`: `payment_processing_profiles` |
| `currency` | select | Currency | yes | no |  | `purpose`: `transaction_currency`; `default_behavior`: `customer_primary_currency`; `requires_feature`: `Multi-Currency Customers` (when `choosing_transaction_currency`) [`real_name`]; `ui_effect`: `reprices_items_and_recalculates_tax_and_profit`; `caveat`: `custom_prices_not_repriced` |
| `customer` | select | Customer | yes | no |  | `purpose`: `deposit_customer`; `recommendation`: `select_project_not_parent`; `allowed_values_listed_in`: `customer_and_project_records`; `ui_effect`: `new_option_creates_record` |
| `customercode` | text | Customer Code | no | no |  | `purpose`: `card_customer_code`; `shown_only_when`: `display_customer_code_web_site_preference`; `requires`: `level_2_card_processing`; `use_when`: `tracking_card_use_per_department` |
| `debitcardissueno` | text | Issue No. | no | no |  | `purpose`: `debit_card_issue_number` |
| `department` | select | Department | no | no |  | `purpose`: `department`; `allowed_values_listed_in`: `department_records`; `ui_effect`: `new_option_creates_record` |
| `exchangerate` | currency2 | Exchange Rate | yes | no |  | `purpose`: `exchange_rate`; `can`: `update_currency_record_rate` |
| `ignoreavs` | checkbox | Ignore AVS | no | no |  | `purpose`: `ignore_avs_result`; `true_means`: `payment_processed_despite_avs_mismatch` |
| `ignorecsc` | checkbox | Ignore CSC | no | no |  | `purpose`: `ignore_csc_result`; `true_means`: `payment_processed_despite_csc_mismatch` |
| `isrecurringpayment` | checkbox | Recurring Payment | no | no |  | `purpose`: `recurring_payment`; `effect`: `processes_after_card_expiry`; `use_when`: `billing_schedule_or_memorized_orders` |
| `location` | select | Location | no | no |  | `purpose`: `transaction_location`; `allowed_values_listed_in`: `location_records`; `ui_effect`: `new_option_creates_record` |
| `memo` | text | Memo | no | yes |  | `purpose`: `memo` |
| `payment` | poscurrency | Payment Amount | yes | no |  | `purpose`: `deposit_amount` |
| `paymentmethod` | select | Payment Method | no | no |  | `purpose`: `payment_method`; `allowed_values_listed_in`: `accounting_list_payment_method` |
| `pnrefnum` | text | P/N Ref. | no | no |  | `purpose`: `verisign_authentication_code`; `value_source`: `set_on_approval` |
| `postingperiod` | select | Posting Period | no | no |  | `purpose`: `posting_period`; `cannot`: `post_to_closed_period` |
| `salesorder` | select | Sales Order | no | no |  | `purpose`: `reserved_for_sales_order`; `constraint`: `applies_only_to_this_order`; `effect`: `excess_becomes_separate_deposit` |
| `subsidiary` | select | Subsidiary | no | no |  | `purpose`: `subsidiary` |
| `trandate` | date | Date | yes | no |  | `purpose`: `deposit_date`; `default_behavior`: `current_date`; `displayed_on`: `registers`; `caveat`: `deposits_liability_not_in_ar_balance` |
| `tranid` | text | Deposit # | no | no |  | `purpose`: `deposit_number`; `value_source`: `system_generated` |
| `undepfunds` | radio | Undep. Funds | no | no |  | `purpose`: `undeposited_funds_or_account`; `use_when`: `undeposited_for_cash_or_check`; `options`: `Undep. Funds`, `Account`; `requires`: `make_deposits_later` (when `undeposited_funds`); `use_when`: `account_for_card_payments` |
| `validfrom` | ccvalidfrom | Valid From / Start Date (MM/YYYY) | no | no |  | `purpose`: `card_valid_from` |
| `account` | select |  | no | no |  |  |
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
| `currencyname` | text | Currency | no | no |  |  |
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
| `salesorderrequireddepositdue` | poscurrency | Sales Order Required Deposit Due | no | no |  |  |
| `softdescriptor` | text | Soft Descriptor | no | no |  |  |
| `status` | text | Status | no | no |  |  |
| `statusRef` | text |  | no | no |  |  |
| `threedstatuscode` | text | Payer Authentication Status | no | no |  |  |
| `tobeemailed` | checkbox |  | no | no |  |  |

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

## Transform targets

| Target record | Default fields |
|---|---|
| `depositapplication` | `customform` |
