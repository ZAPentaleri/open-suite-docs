# Vendor (`vendor`)
<!-- id: record:vendor -->

| Fact | Value |
|---|---|
| Internal ID | `vendor` |
| Category | entity |
| Client scripts | yes |
| Server scripts | yes |
| Searchable | yes |
| Tags | `custom_fields_supported`, `scripting_full` |

## Record facts

- `script_types`: `client`, `server`
- `script_functions`: `create`, `update`, `copy`, `delete`, `search`, `transform`
- `unreadable_in_before_submit_for_external_roles`: `password`, `password2`
- `purpose`: `company_or_person_you_buy_from`
- `value_examples`: `transform_to_purchaseorder`, `transform_to_vendorbill`, `transform_to_vendorpayment`

## Body fields

| Field ID | Type | Label | Required | submitField | Read-only | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `accountnumber` | text | Account | no | yes |  |  | `purpose`: `your_account_number_with_vendor`; `displayed_on`: `purchase_order_vendor_number`, `check_memo` |
| `altphone` | phone | Alt. Phone | no | yes |  |  | `purpose`: `alternate_phone`; `accepts_forms`: `nnn-nnn-nnnn`, `1-nnn-nnn-nnnn`, `(nnn) nnn-nnnn`, `1(nnn) nnn-nnnn`, `nnn-nnn-nnnn ext nnnn` |
| `autoname` | checkbox | Auto | no | no |  |  | `purpose`: `auto_name`; `true_means`: `name_from_auto_generated_numbers`; `false_means`: `enter_name_manually` |
| `balance` | currency | Balance | no | no |  |  | `purpose`: `ap_balance`; `value_source`: `system_displayed` |
| `balanceprimary` | currency | Balance | no | no | yes |  | `purpose`: `ap_balance_in_currency`; `value_source`: `calculated` |
| `bcn` | text | Business Number | no | no |  |  | `purpose`: `ccra_business_number`; `format`: `15_digits` |
| `billpay` | checkbox | Enable Online Bill Pay | no | no |  |  | `purpose`: `pay_vendor_online`; `requires`: `online_bill_pay_set_up`; `doc_inconsistency`: `unresolved_navigation_placeholders` |
| `category` | select | Category | no | yes |  |  | `purpose`: `vendor_category`; `allowed_values_listed_in`: `accounting_list_vendor_category`; `required_when`: `tax_agencies_and_payroll_providers` [`tax_agency_category`] |
| `comments` | textarea | Comments | no | yes |  |  | `purpose`: `comments` |
| `companyname` | text | Company Name | no | yes |  |  | `purpose`: `supplier_legal_name`; `recommendation`: `enter_when_auto_numbering` |
| `contact` | select | Primary Contact | no | no |  |  | `purpose`: `check_and_order_contact`; `displayed_on`: `checks`, `orders`; `ui_effect`: `new_option_creates_record` |
| `creditlimit` | poscurrency | Credit Limit | no | yes |  |  | `purpose`: `vendor_credit_limit`; `value_unit`: `vendor_primary_currency`; `default_behavior`: `no_limit` (when `blank`); `special_values`: `0`; `option_meanings`: `0:vendor_on_hold_warning`; `constraint`: `not_negative`; `effect`: `validates_po_and_bill_against_global_limit`; `caveat`: `subsidiary_limits_not_in_global_validation` |
| `currency` | select | Primary Currency | yes | no |  |  | `purpose`: `vendor_currency`; `requires_feature`: `Multi-Currency Vendors` (when `primary_currency`) [`real_name`]; `allowed_values_listed_in`: `currency_records` |
| `customform` | select | Custom Form | yes | no |  |  | `purpose`: `entry_form`; `default_value`: `Standard Vendor Form` |
| `datecreated` | datetime | Date Created | no | no |  |  | `purpose`: `date_created`; `value_source`: `system_displayed` |
| `defaultaddress` | address | Address | no | no |  |  | `purpose`: `default_billing_address`; `value_source`: `address_sublist_default_billing` |
| `defaultbankaccount` | select | Bank Account | no | no |  |  | `purpose`: `default_bank_account` |
| `defaulttaxreg` | select | Default Tax Reg. | no | no |  |  | `purpose`: `default_tax_registration` |
| `email` | email | Email | no | no |  |  | `purpose`: `email_address`; `effect`: `part_of_vendor_login` |
| `emailpreference` | select | Email Preference | no | yes |  |  | `purpose`: `email_format`; `special_values`: `Default`; `option_meanings`: `default:user_preference` |
| `emailtransactions` | checkbox | Email | no | yes |  |  | `purpose`: `preferred_transaction_delivery`; `effect`: `checks_matching_to_be_box_on_vendor_transactions`; `applies_to`: `created_copied_or_converted_transactions`; `effect`: `overrides_form_settings`; `configured_by`: `vendors_default_to_print_fax_email` [`new_vendors`]; `supported_in`: `mass_update`; `help_text_shared_with`: `vendor.faxtransactions`, `vendor.printtransactions` |
| `entityid` | text | Vendor ID | yes | yes |  |  | `purpose`: `supplier_id`; `displayed_on`: `lists`; `value_source`: `auto_generated_number` (when `auto_generated_numbering`) |
| `expenseaccount` | select | Default Expense Account | no | no |  |  | `purpose`: `default_expense_account`; `effect`: `default_on_vendor_transactions`; `can`: `change_per_transaction`; `ui_effect`: `new_option_creates_record` |
| `externalid` | text | ExternalId | no | no |  |  | `purpose`: `external_id` |
| `fax` | phone | Fax | no | yes |  |  | `purpose`: `fax_number`; `format`: `as_dialed_including_prefix`; `ui_effect`: `fills_to_be_faxed_on_transactions` |
| `faxtransactions` | checkbox | Fax | no | yes |  |  | `purpose`: `preferred_transaction_delivery`; `effect`: `checks_matching_to_be_box_on_vendor_transactions`; `applies_to`: `created_copied_or_converted_transactions`; `effect`: `overrides_form_settings`; `configured_by`: `vendors_default_to_print_fax_email` [`new_vendors`]; `supported_in`: `mass_update`; `help_text_shared_with`: `vendor.emailtransactions`, `vendor.printtransactions` |
| `firstname` | text | Name | no | no |  |  | `purpose`: `first_name` |
| `giveaccess` | checkbox | Give Access | no | no |  |  | `purpose`: `give_netsuite_access`; `requires`: `role_assignment`; `use_when`: `vendor_center_role_for_history` |
| `globalsubscriptionstatus` | select | Global Subscription Status | no | no |  |  | `purpose`: `global_subscription_status`; `allowed_values`: `Confirmed Opt-In`, `Soft Opt-In`, `Soft Opt-Out`, `Confirmed Opt-Out`; `constraint`: `only_recipient_sets_confirmed_statuses`; `caveat`: `re_opt_in_only_via_vendor_center_or_old_link` |
| `image` | select | Image | no | no |  |  | `purpose`: `record_image`; `value_source`: `file_cabinet_image`; `ui_effect`: `new_option_creates_record` |
| `incoterm` | select | Incoterm | no | no |  |  | `purpose`: `default_incoterm`; `value_kind`: `three_letter_trade_term` |
| `is1099eligible` | checkbox | 1099 Eligible | no | yes |  |  | `purpose`: `eligible_for_1099`; `applies_when`: `paid_600_or_more_per_year`; `constraint`: `us_dollar_vendors_only` |
| `isinactive` | checkbox | Inactive | no | yes |  |  | `purpose`: `inactivate_vendor`; `ui_effect`: `hidden_from_lists_unless_show_inactives` |
| `isjobresourcevend` | checkbox | Project Resource | no | yes |  |  | `purpose`: `project_resource`; `effect`: `assignable_to_tasks_and_projects`; `constraint`: `not_with_multi_subsidiary_sharing` (when `oneworld`) |
| `isperson` | radio | Company | no | yes |  |  | `purpose`: `company_or_individual`; `allowed_values`: `Company`, `Individual` |
| `laborcost` | currency | Labor Cost | no | yes |  |  | `purpose`: `labor_cost`; `effect`: `used_for_job_profitability` |
| `lastmodifieddate` | datetime | Last Modified Date | no | no |  |  | `purpose`: `date_last_modified` |
| `lastname` | text |  | no | no |  |  | `purpose`: `last_name` |
| `legalname` | text | Legal Name | no | no |  |  | `purpose`: `legal_name`; `default_behavior`: `company_name` |
| `middlename` | text |  | no | no |  |  | `purpose`: `middle_name` |
| `openingbalance` | currency | Opening Balance | no | no |  |  | `purpose`: `opening_balance` |
| `openingbalanceaccount` | select | Opening Balance Account | no | no |  |  | `purpose`: `opening_balance_account` |
| `openingbalancedate` | date | Opening Balance Date | no | no |  |  | `purpose`: `opening_balance_date` |
| `otherrelationships` | select | Other Relationships | no | no |  |  | `purpose`: `other_records_for_same_entity`; `value_source`: `system_displayed`; `ui_effect`: `add_new_creates_other_record_type` |
| `payablesaccount` | select | Default Payables Account | no | no |  |  | `purpose`: `default_payables_account` |
| `phone` | phone | Phone | no | yes |  |  | `purpose`: `phone`; `displayed_on`: `vendor_list_report`; `required_when`: `online_bill_pay_feature` |
| `phoneticname` | text | Furigana | no | no |  |  | `purpose`: `furigana_sort_name` |
| `predconfidence` | percent | Predicted Confidence Level | no | no |  |  | `purpose`: `delivery_confidence_percent` |
| `predicteddays` | integer | Predicted Days Late | no | no |  |  | `purpose`: `predicted_days_late`; `format`: `negative_for_early` |
| `prepaymentbalance` | currency | Prepayment Balance | no |  |  |  | `purpose`: `unapplied_prepayment_balance`; `value_source`: `system_displayed` |
| `printoncheckas` | text | Print on Check As | no | no |  |  | `purpose`: `print_on_check_as`; `displayed_on`: `check_pay_to_line` |
| `printtransactions` | checkbox | Print | no | yes |  |  | `purpose`: `preferred_transaction_delivery`; `effect`: `checks_matching_to_be_box_on_vendor_transactions`; `applies_to`: `created_copied_or_converted_transactions`; `effect`: `overrides_form_settings`; `configured_by`: `vendors_default_to_print_fax_email` [`new_vendors`]; `supported_in`: `mass_update`; `help_text_shared_with`: `vendor.emailtransactions`, `vendor.faxtransactions` |
| `purchaseorderamount` | posfloat | Vendor Bill - Purchase Order Amount Tolerance | no | no |  |  | `purpose`: `tolerance_bill_vs_po_amount` |
| `purchaseorderquantity` | posfloat | Vendor Bill - Purchase Order Quantity Tolerance | no | no |  |  | `purpose`: `tolerance_bill_vs_po_quantity` |
| `purchaseorderquantitydiff` | posfloat | Vendor Bill - Purchase Order Quantity Difference | no | no |  |  | `purpose`: `tolerance_bill_vs_po_quantity_difference` |
| `receiptamount` | posfloat | Vendor Bill - Item Receipt Amount Tolerance | no | no |  |  | `purpose`: `tolerance_bill_vs_receipt_amount` |
| `receiptquantity` | posfloat | Vendor Bill - Item Receipt Quantity Tolerance | no | no |  |  | `purpose`: `tolerance_bill_vs_receipt_quantity` |
| `receiptquantitydiff` | posfloat | Vendor Bill - Item Receipt Quantity Difference | no | no |  |  | `purpose`: `tolerance_bill_vs_receipt_quantity_difference` |
| `representingsubsidiary` | select | Represents Subsidiary | no | no |  |  | `purpose`: `represented_subsidiary`; `true_means`: `intercompany_vendor` |
| `requirepwdchange` | checkbox | Require Password Change On Next Login | no | no |  |  | `purpose`: `require_password_change`; `effect`: `forces_password_change_at_next_login`; `caveat`: `never_reads_back_as_checked`; `effect`: `save_sets_internal_flag_cleared_after_change` |
| `sendemail` | checkbox | Send Notification Email | no | no |  |  | `purpose`: `send_access_email`; `effect`: `emails_access_notice_with_password_link`; `false_means`: `password_must_be_assigned_manually` |
| `subsidiary` | select | Subsidiary | yes | no |  |  | `purpose`: `vendor_subsidiary`; `requires`: `set_before_entering_transactions`; `default_behavior`: `primary_subsidiary_base_currency` [`vendor_currency`]; `cannot`: `change_after_transaction_posted`; `cannot`: `change_primary_subsidiary_after_save` (when `oneworld`) |
| `taxidnum` | text | Tax ID | no | yes |  |  | `purpose`: `tax_id_number`; `accepts_forms`: `ssn_for_individuals`; `required_when`: `issuing_1099` |
| `taxitem` | select | Tax Code | no | no |  |  | `purpose`: `default_tax_code`; `can`: `change_per_transaction` |
| `terms` | select | Terms | no | yes |  |  | `purpose`: `default_terms`; `can`: `change_per_transaction`; `allowed_values_listed_in`: `accounting_list_terms` |
| `unbilledorders` | currency | Unbilled Orders | no | no |  |  | `purpose`: `unbilled_orders_total`; `effect`: `in_credit_limit` (when `vendor_credit_limit_includes_orders`) |
| `unbilledordersprimary` | currency | Unbilled Orders | no | no |  |  | `purpose`: `unbilled_orders_in_currency` |
| `unsubscribe` | select | Unsubscribe from Campaigns | no | no |  |  | `purpose`: `unsubscribed_from_marketing`; `true_means`: `no_campaign_email`; `configured_by`: `unsubscribed_to_marketing_by_default` (when `us_edition`) |
| `url` | url | Web Address | no | yes |  |  | `purpose`: `vendor_website`; `ui_effect`: `shown_as_link_in_view_mode` |
| `vatregnumber` | text | Tax Reg. Number | no | no |  |  | `purpose`: `vat_registration_number` |
| `workcalendar` | select | Work Calendar | yes | yes |  |  | `purpose`: `work_calendar` |
| `altemail` | email | Alt. Email | no | yes |  |  |  |
| `defaultvendorpaymentaccount` | select | Default Vendor Payment Account | no |  |  |  |  |
| `homephone` | phone | Home Phone | no | yes |  |  |  |
| `mobilephone` | phone | Mobile Phone | no | yes |  |  |  |
| `parent` | integer | Parent | no | no |  |  |  |
| `salutation` | text | Mr./Ms... | no | yes |  |  |  |
| `strength` | text | Password Strength | no | no |  |  |  |
| `subsidiaryedition` | text | Edition | no | no |  |  |  |
| `taxfractionunit` | select | Tax Rounding Precision | no | no |  |  |  |
| `taxrounding` | select | Tax Rounding Method | no | no |  |  |  |
| `tegatamaturity` | integer | Tegata Maturity Date | no | no |  |  |  |
| `title` | text | Job Title | no | no |  |  |  |
| `password` |  | Password |  |  |  |  |  |
| `password2` |  | Confirm Password |  |  |  |  |  |

## Sublist `addressbook` (Address Book)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `addressbookaddress` | summary | Edit | no |  |  |
| `addressid` | text |  | no |  |  |
| `defaultbilling` | checkbox | Default Billing | no |  |  |
| `defaultshipping` | checkbox | Default Shipping | no |  |  |
| `id` | integer | ID | no |  |  |
| `internalid` | integer |  | no |  |  |
| `label` | text | Label | no |  |  |

## Sublist `currency` (Currencies)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `balance` | currency | Balance | no |  |  |
| `currency` | select | Currency | yes |  |  |
| `unbilledorders` | currency | Unbilled Orders | no |  |  |

## Sublist `currencylist` (Currencies)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `prepaymentbalance` | currency | Prepayment Balance | no |  |  |

## Sublist `taxregistration` (Tax Registrations)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `address` | select | Address | no |  |  |
| `id` | text |  | no |  |  |
| `nexus` | select | Nexus | no |  |  |
| `nexuscountry` | select | Country | no |  |  |
| `taxregistrationnumber` | text | Tax Reg. Number | no |  |  |

## Tabs

| Tab ID | Label |
|---|---|
| `access` | Access |
| `address` | Address |
| `commission` | Commission |
| `custom` | Custom |
| `financial` | Financial |
| `general` | General |
| `info` | Info |
| `marketing` | Marketing |
| `workflow` | Workflow |

## Transform targets

| Target record | Default fields |
|---|---|
| `purchaseorder` | `customform` |
| `vendorbill` | `customform`, `entity`, `poids` |
| `vendorpayment` | `customform` |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `accountnumber` | text | Account |  |
| `address` | textarea | Address |  |
| `addressee` | text | Addressee |  |
| `addresslabel` | text | Address Label |  |
| `addressphone` | phone | Address Phone |  |
| `attention` | text | Attention |  |
| `balance` | currency | Balance |  |
| `category` | select | Category |  |
| `city` | text | City |  |
| `comments` | textarea | Comments |  |
| `contact` | text | Contact |  |
| `country` | select | Country |  |
| `county` | text | County/State |  |
| `creditlimit` | currency | Credit Limit |  |
| `currency` | select | Currency |  |
| `currentexchangerate` | currency | Current Currency Exchange Rate |  |
| `datecreated` | datetime | Date Created |  |
| `defaulttaxreg` | select | Default Tax Reg. |  |
| `defaulttaxregtext` | text | Default Tax Reg. (Text) |  |
| `defaultvendorpaymentaccount` | select | Default Vendor Payment Account |  |
| `eligibleforcommission` | checkbox | Eligible For Commission |  |
| `email` | text | Email |  |
| `emailpreference` | select | Email Preference |  |
| `emailtransactions` | checkbox | Email Transactions |  |
| `entityid` | text | Name/ID |  |
| `expenseaccount` | select | Default Expense Account |  |
| `externalid` | select | External ID |  |
| `externalidstring` | text | External ID (Text) |  |
| `fax` | phone | Fax |  |
| `faxtransactions` | checkbox | Fax Transactions |  |
| `firstname` | text | First Name |  |
| `formuladate` | date | Formula (Date) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulatext` | text | Formula (Text) |  |
| `fxbalance` | currency | Balance (Foreign Currency) |  |
| `fxunbilledorders` | currency | Unbilled Orders (Foreign Currency) |  |
| `giveaccess` | checkbox | Login Access |  |
| `globalsubscriptionstatus` | select | Global Subscription Status |  |
| `group` | multiselect | Group |  |
| `hasduplicates` | checkbox | Duplicate |  |
| `image` | image | Image |  |
| `incoterm` | select | Incoterm |  |
| `internalid` | select | Internal ID |  |
| `internalidnumber` | integer | Internal ID (Number) |  |
| `is1099eligible` | checkbox | 1099 Eligible |  |
| `isdefaultbilling` | checkbox | Default Billing Address |  |
| `isdefaultshipping` | checkbox | Default Shipping Address |  |
| `isinactive` | checkbox | Inactive |  |
| `isjobresourcevend` | checkbox | Is Job Resource |  |
| `isperson` | checkbox | Is Individual |  |
| `laborcost` | currency | Labor Cost |  |
| `language` | select | Language |  |
| `lastmodifieddate` | datetime | Last Modified |  |
| `lastname` | text | Last Name |  |
| `level` | select | Level |  |
| `middlename` | text | Middle Name |  |
| `otherrelationships` | multiselect | Other Relationships |  |
| `payablesaccount` | select | Default Payables Account |  |
| `pec` | text | PEC |  |
| `permission` | select | Permission |  |
| `phone` | phone | Phone |  |
| `phoneticname` | text | Phonetic Name |  |
| `predconfidence` | float | Predicted Confidence Level |  |
| `predicteddays` | float | Predicted Days Late |  |
| `prepaymentbalance` | currency | Prepayment Balance |  |
| `printtransactions` | checkbox | Print Transactions |  |
| `purchaseorderamount` | float | Vendor Bill - Purchase Order Amount Tolerance |  |
| `purchaseorderquantity` | float | Vendor Bill - Purchase Order Quantity Tolerance |  |
| `purchaseorderquantitydiff` | float | Vendor Bill - Purchase Order Quantity Difference |  |
| `receiptamount` | float | Vendor Bill - Item Receipt Amount Tolerance |  |
| `receiptquantity` | float | Vendor Bill - Item Receipt Quantity Tolerance |  |
| `receiptquantitydiff` | float | Vendor Bill - Item Receipt Quantity Difference |  |
| `representingsubsidiary` | select | Represents Subsidiary |  |
| `salutation` | text | Mr./Mrs... |  |
| `state` | select | State/Province |  |
| `subsidiary` | select | Subsidiary |  |
| `taxidnum` | text | Tax ID |  |
| `title` | text | Job Title |  |
| `type` | select | Entity Type |  |
| `unbilledorders` | currency | Unbilled Orders |  |
| `url` | url | Web Address |  |
| `vatregnumber` | text | Tax Number |  |
| `workcalendar` | select | Work Calendar |  |
| `zipcode` | text | Zip |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `accountnumber` | text | Account |  |
| `address` | text | Address |  |
| `addressee` | text | Addressee |  |
| `addressinternalid` | text | Address Internal ID |  |
| `addresslabel` | text | Address Label |  |
| `addressphone` | text | Address Phone |  |
| `altcontact` | text | Alt. Contact |  |
| `altemail` | email | Alt. Email |  |
| `altname` | text | Name |  |
| `altphone` | phone | Office Phone |  |
| `attention` | text | Attention |  |
| `balance` | currency | Balance |  |
| `billcountrycode` | text | Billing Country Code |  |
| `billzipcode` | text | Billing Zip |  |
| `category` | select | Category |  |
| `city` | text | City |  |
| `comments` | text | Comments |  |
| `companyname` | text | Company Name |  |
| `contact` | select | Primary Contact |  |
| `country` | select | Country |  |
| `countrycode` | text | Country Code |  |
| `creditlimit` | currency | Credit Limit |  |
| `currency` | select | Currency |  |
| `currentexchangerate` | float | Current Currency Exchange Rate |  |
| `datecreated` | datetime | Date Created |  |
| `defaulttaxreg` | text | Default Tax Reg. |  |
| `defaultvendorpaymentaccount` | select | Default Vendor Payment Account |  |
| `eligibleforcommission` | checkbox | Eligible for Commission |  |
| `email` | email | Email |  |
| `emailpreference` | select | Email Preference |  |
| `emailtransactions` | checkbox | Email Transactions |  |
| `entityid` | text | Name |  |
| `entitynumber` | integer | Number |  |
| `expenseaccount` | select | Default Expense Account |  |
| `externalid` | select | External ID |  |
| `fax` | text | Fax |  |
| `faxtransactions` | checkbox | Fax Transactions |  |
| `firstname` | text | First Name |  |
| `formulacurrency` | currency | Formula (Currency) |  |
| `formuladate` | date | Formula (Date) |  |
| `formuladatetime` | datetime | Formula (Date/Time) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulapercent` | percent | Formula (Percent) |  |
| `formulatext` | text | Formula (Text) |  |
| `fxbalance` | currency | Balance (Foreign Currency) |  |
| `fxunbilledorders` | currency | Unbilled Orders (Foreign Currency) |  |
| `giveaccess` | checkbox | Login Access |  |
| `globalsubscriptionstatus` | select | Global Subscription Status |  |
| `hasduplicates` | checkbox | Duplicate |  |
| `homephone` | phone | Home Phone |  |
| `image` | select | Image |  |
| `incoterm` | select | Incoterm |  |
| `internalid` | select | Internal ID |  |
| `is1099eligible` | checkbox | 1099 Eligible |  |
| `isdefaultbilling` | checkbox | Default Billing Address |  |
| `isdefaultshipping` | checkbox | Default Shipping Address |  |
| `isinactive` | checkbox | Inactive |  |
| `isjobresourcevend` | checkbox | Job Resource |  |
| `isperson` | checkbox | Is Individual |  |
| `laborcost` | currency | Labor Cost |  |
| `language` | select | Language |  |
| `lastmodifieddate` | datetime | Last Modified |  |
| `lastname` | text | Last Name |  |
| `lastviewed` | datetime | Last Viewed |  |
| `level` | select | Level |  |
| `middlename` | text | Middle Name |  |
| `mobilephone` | phone | Mobile Phone |  |
| `payablesaccount` | select | Default Payables Account |  |
| `pec` | text | PEC |  |
| `permission` | select | Permission |  |
| `phone` | phone | Phone |  |
| `phoneticname` | text | Phonetic Name |  |
| `predconfidence` | float | Predicted Confidence Level |  |
| `predicteddays` | float | Predicted Days Late |  |
| `prepaymentbalance` | currency | Prepayment Balance |  |
| `printoncheckas` | text | Print As |  |
| `printtransactions` | checkbox | Print Transactions |  |
| `purchaseorderamount` | float | Vendor Bill - Purchase Order Amount Tolerance |  |
| `purchaseorderquantity` | float | Vendor Bill - Purchase Order Quantity Tolerance |  |
| `purchaseorderquantitydiff` | float | Vendor Bill - Purchase Order Quantity Difference |  |
| `receiptamount` | float | Vendor Bill - Item Receipt Amount Tolerance |  |
| `receiptquantity` | float | Vendor Bill - Item Receipt Quantity Tolerance |  |
| `receiptquantitydiff` | float | Vendor Bill - Item Receipt Quantity Difference |  |
| `representingsubsidiary` | select | Represents Subsidiary |  |
| `salutation` | text | Mr./Mrs... |  |
| `shipcountrycode` | text | Shipping Country Code |  |
| `state` | select | State/Province |  |
| `statedisplayname` | select | State/Province Display Name |  |
| `subscription` | select | Subscription |  |
| `subscriptiondate` | datetime | Subscription Date |  |
| `subscriptionstatus` | checkbox | Subscription Status |  |
| `subsidiary` | select | Subsidiary |  |
| `subsidiarynohierarchy` | select | Subsidiary (no hierarchy) |  |
| `taxidnum` | text | Tax ID |  |
| `terms` | select | Terms |  |
| `title` | text | Job Title |  |
| `type` | select | Type |  |
| `unbilledorders` | currency | Unbilled Orders |  |
| `url` | text | Web Address |  |
| `vatregnumber` | text | Tax Number |  |
| `workcalendar` | select | Work Calendar |  |
| `zipcode` | text | Zip Code |  |

## Search joins

| Join ID | Label | Joined record |
|---|---|---|
| `account` | Payable Account | Account |
| `campaignresponse` | Campaign Response | Campaign |
| `contact` | Contact | Contact |
| `contactprimary` | Contact (Primary) | Contact |
| `expaccount` | Expense Account | Account |
| `messages` | Messages | Message |
| `messagesfrom` | Messages From | Message |
| `messagesto` | Messages To | Message |
| `msesubsidiary` | Subsidiary | Subsidiary |

## Suspected source errors

> [!SOURCE-ERROR] `unresolved_navigation_placeholders` (on `billpay`)
<!-- source-error: record:vendor | billpay | unresolved_navigation_placeholders -->
