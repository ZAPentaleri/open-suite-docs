# Customer Refund (`customerrefund`)
<!-- id: record:customerrefund -->

| Fact | Value |
|---|---|
| Internal ID | `customerrefund` |
| Category | transaction |
| Client scripts | no |
| Server scripts | yes |
| Searchable | yes |
| Search data via | Transaction |
| Tags | `custom_fields_supported`, `scripting_full` |

## Record facts

- `initialization_default_value`: `entity`, `entity_internal_id` (refs `N/record.create`, `N/record.copy`, `N/record.transform`, `N/record.load`)
- `script_types`: `server`
- `script_functions`: `create`, `update`, `copy`, `delete`, `search`
- `unreadable_in_before_submit_for_external_roles`: `ccnumber`
- `body_only_scriptable_fields`: `estgrossprofit`, `estgrossprofitpercent`, `totalcostestimate`
- `address_subrecord`: `payee_address`
- `list_sublists`: `deposits`
- `doc_inconsistency`: `functions_sentence_names_customer_deposit`
- `ui_effect`: `payee_address_shown_as_sublist`
- `risk`: `scripts_using_old_payee_address_text_may_break`
- `caveat`: `gross_profit_fields_not_scriptable_at_line_level`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `account` | select | Account | yes | no |  | `purpose`: `refund_bank_account`; `allowed_values_listed_in`: `bank_accounts`; `ui_effect`: `new_option_creates_record` |
| `aracct` | select | A/R Account | no | no |  | `purpose`: `receivables_account`; `allowed_values_listed_in`: `accounts_receivable_accounts`; `ui_effect`: `new_option_creates_record` |
| `balance` | currency | Balance | no | no |  | `purpose`: `customer_balance`; `value_source`: `system_displayed` |
| `ccapproved` | checkbox | CC Approved | no | no |  | `purpose`: `card_refund_approved_externally`; `value_source`: `set_on_approval` (when `netsuite_merchant_account`); `use_when`: `card_refund_validated_outside_netsuite` |
| `ccexpiredate` | ccexpdate | Expires (MM/YYYY) | no | no |  | `purpose`: `card_expiration_date`; `format`: `MM/YYYY` |
| `ccname` | text | Name on Card | no | no |  | `purpose`: `cardholder_name`; `default_behavior`: `customer_default_card_holder`; `recommendation`: `fix_value_on_customer_record` |
| `ccnumber` | ccnumber | Credit Card # | no | no |  | `purpose`: `card_number`; `format`: `digits_without_spaces`; `caveat`: `visible_only_to_merchant_and_customer` |
| `ccstreet` | text | Card Street | no | no |  | `purpose`: `card_billing_street`; `value_source`: `customer_billing_address_street`; `recommendation`: `fix_value_on_customer_record` |
| `cczipcode` | text | Card Zip Code | no | no |  | `purpose`: `card_billing_postal_code`; `value_source`: `customer_billing_address_postal_code`; `recommendation`: `fix_value_on_customer_record` |
| `chargeit` | checkbox | Process Credit Card | no | no |  | `purpose`: `process_card_refund`; `requires`: `netsuite_merchant_account` |
| `class` | select | Class | no | no |  | `purpose`: `class` |
| `consolidatebalance` | currency | Consolidated Balance | no | no |  | `purpose`: `consolidated_open_invoices_total`; `value_scope`: `customer_subcustomer_hierarchy` |
| `creditcard` | select | Credit Card Select | no | no |  | `purpose`: `stored_card_for_refund`; `allowed_values_listed_in`: `customer_stored_cards`; `ui_effect`: `new_option_creates_record` |
| `creditcardprocessor` | select | Credit Card Processor | no | no |  | `purpose`: `card_processor`; `allowed_values_listed_in`: `payment_processing_profiles` |
| `currency` | select | Currency | yes | no |  | `purpose`: `refund_currency`; `requires_feature`: `Multi-Currency Customers` (when `choosing_refund_currency`) [`real_name`]; `effect`: `filters_apply_sublist_credits_and_deposits` |
| `currencyname` | text | Currency | no | no |  | `purpose`: `customer_currency_name`; `effect`: `all_amounts_in_this_currency` |
| `customer` | select | Customer | yes | no |  | `purpose`: `refund_customer`; `recommendation`: `select_project_not_parent`; `allowed_values_listed_in`: `customer_and_project_records`; `ui_effect`: `new_option_creates_record` |
| `customercode` | text | Customer Code | no | no |  | `purpose`: `card_customer_code`; `shown_only_when`: `display_customer_code_web_site_preference`; `requires`: `level_2_card_processing` |
| `debitcardissueno` | text | Issue No. | no | no |  | `purpose`: `debit_card_issue_number` |
| `department` | select | Department | no | no |  | `purpose`: `department`; `allowed_values_listed_in`: `department_records`; `ui_effect`: `new_option_creates_record` |
| `exchangerate` | currency2 | Exchange Rate | yes | no |  | `purpose`: `exchange_rate`; `can`: `update_currency_record_rate` |
| `location` | select | Location | no | no |  | `purpose`: `transaction_location`; `allowed_values_listed_in`: `location_records`; `ui_effect`: `new_option_creates_record` |
| `memo` | text | Memo | no | yes |  | `purpose`: `memo`; `displayed_on`: `checks`, `registers`; `value_examples`: `original_transaction_number` |
| `paymentmethod` | select | Refund Method | yes | no |  | `purpose`: `refund_method`; `allowed_values_listed_in`: `accounting_list_payment_method` |
| `pnrefnum` | text | P/N Ref. | no | no |  | `purpose`: `verisign_authentication_code`; `value_source`: `set_on_approval` |
| `postingperiod` | select | Posting Period | no | no |  | `purpose`: `posting_period`; `cannot`: `post_to_closed_period` |
| `softdescriptor` | text | Soft Descriptor | no | no |  | `purpose`: `card_statement_soft_descriptor`; `displayed_on`: `customer_card_statement`; `allowed_values_listed_in`: `credit_card_soft_descriptors` |
| `subsidiary` | select | Subsidiary | no | no |  | `purpose`: `subsidiary`; `value_source`: `system_displayed` |
| `tobeprinted` | checkbox | To Be Printed | no | yes |  | `purpose`: `print_later`; `true_means`: `queued_for_printing` |
| `total` | currency | Refund Amount | no | no |  | `purpose`: `refund_total`; `value_source`: `sum_of_apply_lines`; `help_text_shared_with`: `customerrefund.apply.total` |
| `trandate` | date | Date | yes | no |  | `purpose`: `refund_date`; `default_behavior`: `current_date` |
| `tranid` | text | Check # | no | no |  | `purpose`: `check_number`; `default_behavior`: `next_consecutive_number` (when `issuing_check`) |
| `transactionnumber` | text | Transaction Number | no | no |  | `purpose`: `transaction_number`; `value_source`: `system_generated` |
| `validfrom` | ccvalidfrom | Valid From / Start Date (MM/YYYY) | no | no |  | `purpose`: `card_valid_from` |
| `allowemptycards` | checkbox | Credits | no | no |  |  |
| `cchold` | text | Detail | no | no |  |  |
| `ccholdetails` | textarea | Detail | no | no |  |  |
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
| `payeeaddress` | summary |  | no | no |  |  |
| `payeeaddresslist` | select | Address To Select | no | no |  |  |
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
| `status` | text | Status | no | no |  |  |
| `statusref` | text |  | no | no |  |  |
| `estgrossprofit` |  | Est. Gross Profit |  |  |  |  |
| `estgrossprofitpercent` |  | Est. Gross Profit Percent |  |  |  |  |
| `totalcostestimate` |  | Est. Extended Cost |  |  |  |  |

## Sublist `apply` (Credits)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `total` | currency | Orig. Amt. | no |  | `purpose`: `refund_total`; `value_source`: `sum_of_apply_lines`; `help_text_shared_with`: `customerrefund.total` |
| `amount` | currency | Payment | no |  |  |
| `apply` | checkbox | Apply | no |  |  |
| `applydate` | date | Date | no |  |  |
| `createdfrom` | text |  | no |  |  |
| `doc` | text |  | no |  |  |
| `due` | currency | Amount Remaining | no |  |  |
| `duedate` | date |  | no |  |  |
| `internalid` | text | Hidden | no |  |  |
| `line` | integer |  | no |  |  |
| `refnum` | text | Ref No. | no |  |  |
| `url` | text |  | no |  |  |

## Sublist `accountingbookdetail` (Accounting Books)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `accountingbook` | select | Secondary Book | no |  |  |
| `exchangerate` | currency2 | Exchange Rate | yes |  |  |

## Sublist `deposit` (Deposits)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `amount` | currency | Payment | no |  |  |
| `apply` | checkbox | Apply | no |  |  |
| `currency` | text |  | no |  |  |
| `depositdate` | date | Date | no |  |  |
| `doc` | text |  | no |  |  |
| `remaining` | currency | Amount Remaining | no |  |  |
| `total` | currency | Orig. Amt. | no |  |  |
| `url` | text |  | no |  |  |

## Tabs

| Tab ID | Label |
|---|---|
| `applications` | Applications |
| `custom` | Custom |
| `history` | History |
| `payment` | Payment |
| `workflow` | Workflow |

## Suspected source errors

> [!SOURCE-ERROR] `functions_sentence_names_customer_deposit`
<!-- source-error: record:customerrefund | - | functions_sentence_names_customer_deposit -->
