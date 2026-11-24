# Lead (`lead`)
<!-- id: record:lead -->

| Fact | Value |
|---|---|
| Internal ID | `lead` |
| Category | entity |
| Client scripts | yes |
| Server scripts | yes |
| Searchable | yes |
| Tags | `scripting_full` |

## Record facts

- `script_types`: `client`, `server`
- `script_functions`: `create`, `update`, `copy`, `delete`, `search`, `transform`
- `unreadable_in_before_submit_for_external_roles`: `password`, `password2`
- `unreadable_for_external_roles`: `ccnumber` [`search_filters_and_columns`]
- `system_generated_fields`: `datecreated`
- `purpose`: `potential_customer_first_sales_cycle_stage`
- `workaround`: `custom_field_defaulting_to_today` (when `own_creation_date_needed`)
- `sublist_requires_feature`: `partners:multi_partner_management`
- `value_kind`: `inline_editor_sublist` [`contactroles`, `access_subtab`]
- `value_kind`: `inline_editor_sublist` [`time_tracking`]
- `caveat`: `static_time_entry_lists_give_inaccurate_script_results`
- `doc_inconsistency`: `access_sublist_note_says_customer_record`

## Body fields

| Field ID | Type | Label | Required | submitField | Read-only | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `accessrole` | select | Role | no | no |  |  | `purpose`: `customer_center_role`; `allowed_values_listed_in`: `customer_center_roles` |
| `accountnumber` | text | Account | no | yes |  |  | `purpose`: `customer_account_number` |
| `altemail` | email | Alt. E-mail | no | yes |  |  | `purpose`: `alternate_email`; `effect`: `replies_attached_to_customer` (when `capture_email_replies`) |
| `altphone` | phone | Alt. Phone | no | yes |  |  | `purpose`: `alternate_phone` |
| `assignedwebsite` | select | Assigned Web Site | no | no |  |  | `purpose`: `assigned_web_site`; `effect`: `customer_restricted_to_site_account`; `default_behavior`: `access_to_all_sites` (when `blank`); `value_scope`: `sites_of_same_subsidiary` (when `oneworld`) |
| `autoname` | checkbox | Auto | no | no |  |  | `purpose`: `auto_name`; `true_means`: `name_from_auto_generated_numbers`; `false_means`: `enter_name_manually`; `configured_by`: `auto_generated_numbers_setup` |
| `balance` | currency | Balance | no | no |  |  | `purpose`: `ar_balance`; `value_unit`: `customer_currency` (when `multiple_currencies`) |
| `buyingreason` | select | Buying Reason | no | yes |  |  | `purpose`: `buying_reason`; `allowed_values_listed_in`: `crm_list_buying_reason` |
| `buyingtimeframe` | select | Buying Time Frame | no | yes |  |  | `purpose`: `buying_time_frame`; `allowed_values_listed_in`: `crm_list_buying_time_frame` |
| `campaigncategory` | select | Campaign Category | no | no |  |  | `purpose`: `campaign_category`; `allowed_values_listed_in`: `campaign_category_records` |
| `category` | select | Category | no | yes |  |  | `purpose`: `customer_category`; `allowed_values_listed_in`: `accounting_list_customer_category`; `value_examples`: `wholesale`, `retail`; `doc_inconsistency`: `unresolved_navigation_placeholders` |
| `clickstream` | text | Clickstream (1st Visit) | no | no |  |  | `purpose`: `first_visit_clickstream`; `value_source`: `web_site_visit_tracking` |
| `comments` | textarea | Comments | no | yes |  |  | `purpose`: `comments` |
| `companyname` | text | Company Name | no | yes |  |  | `purpose`: `legal_company_name`; `recommendation`: `enter_when_auto_numbering` |
| `consolbalance` | currency | Consolidated | no | no |  |  | `purpose`: `consolidated_ar_balance`; `value_scope`: `customer_subcustomer_hierarchy` |
| `consoldaysoverdue` | integer | Consolidated | no | no |  |  | `purpose`: `consolidated_days_overdue`; `value_scope`: `customer_subcustomer_hierarchy` |
| `consoldepositbalance` | currency | Consolidated | no | no |  |  | `purpose`: `consolidated_unapplied_deposits`; `value_scope`: `customer_subcustomer_hierarchy`; `caveat`: `deposits_liability_not_in_ar_balance` |
| `consoloverduebalance` | currency | Consolidated | no | no |  |  | `purpose`: `consolidated_overdue_balance`; `value_scope`: `customer_subcustomer_hierarchy`; `caveat`: `no_due_date_uses_transaction_date` |
| `consolunbilledorders` | currency | Consolidated | no | no |  |  | `purpose`: `consolidated_unbilled_orders`; `value_scope`: `customer_subcustomer_hierarchy`; `effect`: `in_credit_limit` (when `credit_limit_includes_orders`); `configured_by`: `customer_credit_limit_includes_orders` |
| `contact` | select | Primary Contact | no | no |  |  | `purpose`: `primary_contact`; `displayed_on`: `invoice_billing_address`; `allowed_values_listed_in`: `contact_records`; `ui_effect`: `new_option_creates_record`; `can`: `add_more_on_contacts_subtab` |
| `creditholdoverride` | select | Hold | no | no |  |  | `purpose`: `credit_hold_override`; `options`: `Auto`, `On`, `Off`; `option_meanings`: `auto:follows_accounting_preferences`, `on:manual_hold`, `off:manual_release` |
| `creditlimit` | poscurrency | Credit Limit | no | no |  |  | `purpose`: `credit_limit`; `effect`: `warns_or_blocks_transactions_at_limit`; `configured_by`: `customer_credit_limit_handling_preference`; `value_kind`: `max_outstanding_receivables` |
| `currency` | select | Currency | yes | yes |  |  | `purpose`: `customer_currency`; `cannot`: `change_after_customer_transactions_saved`; `effect`: `all_customer_transactions_in_this_currency`; `requires_feature`: `Multi-Currency Customers` (when `primary_currency`) [`real_name`]; `effect`: `default_transaction_and_balance_currency` (when `multi_currency_customers`); `value_source`: `parent_customer_currency` (when `subcustomer`); `constraint`: `changeable_only_without_other_name_records`; `requires`: `re_enter_credit_limit_after_change`; `doc_inconsistency`: `says_cannot_change_then_can_change`; `effect`: `converted_to_base_currency_in_registers_and_reports`; `can`: `add_currencies_on_currencies_subtab` (when `multi_currency_customers`); `allowed_values_listed_in`: `currency_records` |
| `customform` | select | Custom Form | yes | no |  |  | `purpose`: `entry_form`; `special_values`: `Standard Customer Form`; `allowed_values_listed_in`: `standard_and_custom_entry_forms`; `ui_effect`: `new_option_creates_record` |
| `datecreated` | datetime | Date Created | no | no |  |  | `purpose`: `date_created`; `value_source`: `system_recorded` |
| `daysoverdue` | integer | Days Overdue | no | no |  |  | `purpose`: `days_overdue` |
| `defaultaddress` | address | Address | no | no |  |  | `purpose`: `default_billing_address`; `value_source`: `address_sublist_default_billing` |
| `depositbalance` | currency | Deposit Balance | no | no |  |  | `purpose`: `unapplied_deposit_balance`; `caveat`: `deposits_liability_not_in_ar_balance`; `value_unit`: `customer_currency` (when `multiple_currencies`) |
| `draccount` | select | Deferred Revenue Reclassification Account | no | no |  |  | `purpose`: `default_revenue_reclass_deferred_account`; `can`: `override_on_sales_order` |
| `email` | email | E-Mail | no | no |  |  | `purpose`: `email_address`; `effect`: `part_of_customer_login` |
| `emailpreference` | select | E-Mail Preference | no | yes |  |  | `purpose`: `email_format_preference`; `default_behavior`: `user_email_preference`; `options`: `HTML`, `other_formats`; `requires`: `recipient_html_capable` (when `html`) |
| `emailtransactions` | checkbox | Email | no | yes |  |  | `purpose`: `preferred_transaction_delivery`; `effect`: `checks_matching_to_be_box_on_customer_transactions`; `applies_to`: `created_copied_or_converted_transactions`; `effect`: `overrides_form_settings`; `configured_by`: `customers_default_to_print_fax_email` [`new_customers`]; `supported_in`: `mass_update`; `allowed_values`: `regular_mail`, `email`, `fax`, `combination`; `help_text_shared_with`: `customer.faxtransactions`, `customer.printtransactions`, `lead.faxtransactions`, `lead.printtransactions`, `prospect.faxtransactions`, `prospect.printtransactions` |
| `entityid` | text | Customer ID | yes | yes |  |  | `purpose`: `customer_name_or_number`; `displayed_on`: `lists`, `checks`; `value_source`: `auto_generated_number` (when `auto_numbering`) |
| `entitystatus` | select | Status | yes | yes |  |  | `purpose`: `customer_status`; `effect`: `status_stage_sets_lead_prospect_or_customer`; `allowed_values`: `Awarded`, `Not Awarded`, `Closed`, `In Progress`, `Pending` (when `job_record`) |
| `estimatedbudget` | currency | Estimated Budget | no | yes |  |  | `purpose`: `estimated_budget` |
| `fax` | phone | Fax | no | yes |  |  | `purpose`: `fax_number`; `format`: `as_dialed_including_prefix`; `ui_effect`: `fills_to_be_faxed_on_transactions`; `requires`: `fax_service_set_up` (when `faxing_forms`) |
| `faxtransactions` | checkbox | Fax | no | yes |  |  | `purpose`: `preferred_transaction_delivery`; `effect`: `checks_matching_to_be_box_on_customer_transactions`; `applies_to`: `created_copied_or_converted_transactions`; `effect`: `overrides_form_settings`; `configured_by`: `customers_default_to_print_fax_email` [`new_customers`]; `supported_in`: `mass_update`; `allowed_values`: `regular_mail`, `email`, `fax`, `combination`; `help_text_shared_with`: `customer.emailtransactions`, `customer.printtransactions`, `lead.emailtransactions`, `lead.printtransactions`, `prospect.emailtransactions`, `prospect.printtransactions` |
| `firstname` | text | Name | no | no |  |  | `purpose`: `first_name` |
| `firstvisit` | datetime | Date of First Visit | no | no |  |  | `purpose`: `first_web_visit_date`; `value_source`: `web_site_visit_tracking` |
| `fxaccount` | select | Foreign Currency Adjustment Revenue Account | no | no |  |  | `purpose`: `default_fx_adjustment_revenue_account`; `constraint`: `income_account`; `can`: `override_on_sales_order` |
| `giveaccess` | checkbox | Give Access | no | no |  |  | `purpose`: `give_netsuite_access`; `effect`: `customer_center_view_access` |
| `globalsubscriptionstatus` | select | Global Subscription Status | no | no |  |  | `purpose`: `global_subscription_status`; `allowed_values`: `Confirmed Opt-In`, `Soft Opt-In`, `Soft Opt-Out`, `Confirmed Opt-Out`; `constraint`: `only_recipient_sets_confirmed_statuses`; `can`: `set_soft_statuses_manually_or_mass_update`; `caveat`: `confirmed_opt_out_gets_no_campaign_bulk_or_opt_in`; `option_meanings`: `confirmed_opt_in:recipient_wants_campaigns_and_bulk`, `soft_opt_in:receives_opt_in_messages`, `soft_opt_out:no_campaign_or_bulk_opt_in_ok`, `confirmed_opt_out:no_campaign_bulk_or_opt_in`; `caveat`: `re_opt_in_only_via_customer_center_or_old_link` |
| `homephone` | phone | Home Phone | no | yes |  |  | `purpose`: `home_phone` |
| `image` | select | Image | no | no |  |  | `purpose`: `record_image`; `value_source`: `file_cabinet_image`; `ui_effect`: `new_option_creates_record` |
| `isbudgetapproved` | checkbox | Budget Approved | no | yes |  |  | `purpose`: `budget_approved` |
| `isinactive` | checkbox | Inactive | no | yes |  |  | `purpose`: `inactivate_customer`; `ui_effect`: `hidden_from_lists_unless_show_inactives`; `effect`: `loses_netsuite_and_web_store_access` |
| `isperson` | radio | Company | no | yes |  |  | `purpose`: `company_or_individual`; `allowed_values`: `Company`, `Individual`; `ui_effect`: `changes_fields_and_subtabs` |
| `keywords` | text | Search Engine Keywords (1st Visit) | no | no |  |  | `purpose`: `referring_search_keywords`; `value_source`: `web_site_visit_tracking` |
| `language` | select | Language | no | no |  |  | `purpose`: `preferred_language`; `effect`: `printed_transaction_language`; `requires`: `language_enabled_for_translations`; `doc_inconsistency`: `unresolved_navigation_placeholders` |
| `lastname` | text |  | no | no |  |  | `purpose`: `last_name` |
| `lastpagevisited` | text | Last Page Visited | no | no |  |  | `purpose`: `last_page_visited`; `value_source`: `web_site_visit_tracking` |
| `lastvisit` | datetime | Date of Last Visit | no | no |  |  | `purpose`: `last_web_visit_date`; `value_source`: `web_site_visit_tracking` |
| `leadsource` | select | Lead Source | no | no |  |  | `purpose`: `lead_source`; `allowed_values_listed_in`: `campaigns` (when `marketing_automation`); `cannot`: `create_lead_sources` (when `marketing_automation`); `allowed_values_listed_in`: `crm_list_lead_source` (when `no_marketing_automation`); `help_text_shared_with`: `contact.contactsource` |
| `mobilephone` | phone | Mobile Phone | no | no |  |  | `purpose`: `mobile_phone` |
| `negativenumberformat` | select | Negative Number Format | no | no |  |  | `purpose`: `negative_number_format`; `options`: `minus_sign`, `parentheses` |
| `numberformat` | select | Number Format | no | no |  |  | `purpose`: `number_format`; `value_kind`: `thousands_separator_and_decimals` |
| `otherrelationships` | select | Other Relationships | no | no |  |  | `purpose`: `other_records_for_same_entity`; `value_source`: `system_displayed`; `ui_effect`: `add_new_creates_other_record_type` |
| `overduebalance` | currency | Overdue Balance | no | no |  |  | `purpose`: `overdue_balance`; `caveat`: `no_due_date_uses_transaction_date`; `value_unit`: `customer_currency` (when `multiple_currencies`) |
| `parent` | select | Child Of | no | no |  |  | `purpose`: `parent_customer`; `ui_effect`: `indented_under_parent_in_list` |
| `partner` | select | Partner | no | no |  |  | `purpose`: `referring_partner`; `constraint`: `customer_uses_partner_promotions_only` |
| `phone` | phone | Phone | no | yes |  |  | `purpose`: `phone`; `displayed_on`: `customer_list_report` |
| `phoneticname` | text | Furigana | no | no |  |  | `purpose`: `furigana_sort_name` |
| `prefccprocessor` | select | Pref. CC Processor | no | no |  |  | `purpose`: `preferred_card_processor`; `effect`: `used_for_web_orders_with_card`; `allowed_values_listed_in`: `payment_processing_profiles` |
| `pricelevel` | select | Price Level | no | no |  |  | `purpose`: `price_level`; `allowed_values_listed_in`: `price_level_records`; `can`: `items_have_multiple_price_levels` |
| `printoncheckas` | text | Print on Check As | no | no |  |  | `purpose`: `print_on_check_as`; `displayed_on`: `check_pay_to_line` |
| `printtransactions` | checkbox | Print | no | yes |  |  | `purpose`: `preferred_transaction_delivery`; `effect`: `checks_matching_to_be_box_on_customer_transactions`; `applies_to`: `created_copied_or_converted_transactions`; `effect`: `overrides_form_settings`; `configured_by`: `customers_default_to_print_fax_email` [`new_customers`]; `supported_in`: `mass_update`; `allowed_values`: `regular_mail`, `email`, `fax`, `combination`; `help_text_shared_with`: `customer.emailtransactions`, `customer.faxtransactions`, `lead.emailtransactions`, `lead.faxtransactions`, `prospect.emailtransactions`, `prospect.faxtransactions` |
| `receivablesaccount` | select | Default Receivables Account | no | no |  |  | `purpose`: `default_receivables_account`; `special_values`: `Use System Preference`; `default_behavior`: `default_receivables_account_preference` (when `use_system_preference`); `allowed_values_listed_in`: `accounts_receivable_accounts` |
| `referrer` | text | Referrer (1st Visit) | no | no |  |  | `purpose`: `referrer_url`; `value_source`: `web_site_visit_tracking`; `value_examples`: `search_engine_results_url` |
| `resalenumber` | text | Resale Number | no | no |  |  | `purpose`: `resale_tax_license_number`; `requires`: `not_taxable_tax_code` |
| `salesgroup` | select | Choose Team | no | no |  |  | `purpose`: `sales_team`; `allowed_values_listed_in`: `sales_team_groups` |
| `salesreadiness` | select | Sales Readiness | no | yes |  |  | `purpose`: `sales_readiness`; `allowed_values_listed_in`: `crm_list_sales_readiness` |
| `salesrep` | integer | Sales Rep | no | no |  |  | `purpose`: `sales_rep`; `can`: `reassign_using_territories`; `value_source`: `team_primary_rep` (when `team_selling`); `default_behavior`: `creating_user_if_sales_rep` |
| `salutation` | text | Mr./Ms... | no | yes |  |  | `purpose`: `salutation`; `value_examples`: `Mr.`, `Mrs.`, `Ms.`, `Dr.`, `Rev.` |
| `sendemail` | checkbox | Send Notification E-mail | no | no |  |  | `purpose`: `send_access_email`; `effect`: `emails_access_notice_with_password_link`; `false_means`: `password_must_be_assigned_manually` |
| `sourcewebsite` | select | Source Web Site | no | no | yes |  | `purpose`: `registration_web_site` |
| `subsidiary` | select | Subsidiary | yes | no |  |  | `purpose`: `entity_subsidiary`; `requires`: `set_before_entering_transactions`; `effect`: `transactions_associated_with_subsidiary`; `effect`: `entity_access_limited_to_subsidiary`; `cannot`: `change_after_transaction_posted` |
| `syncpartnerteams` | checkbox | Update Transactions | no | no |  |  | `purpose`: `sync_partner_team_to_transactions` |
| `taxable` | checkbox | Taxable | no | no |  |  | `purpose`: `taxable_customer`; `caveat`: `tax_item_applies_even_if_unchecked`; `false_means`: `not_taxable_only_if_tax_item_empty`; `configured_by`: `customers_default_to_taxable`; `true_means`: `customer_pays_sales_tax` |
| `taxitem` | select | Tax Item | no | no |  |  | `purpose`: `default_tax_code`; `can`: `change_per_sale`; `allowed_values_listed_in`: `tax_code_records`; `value_kind`: `single_rate_single_agency_tax` |
| `terms` | select | Terms | no | no |  |  | `purpose`: `default_terms`; `can`: `change_per_sale`; `allowed_values_listed_in`: `accounting_list_terms` |
| `territory` | select | Territory | no | no |  |  | `purpose`: `sales_territory`; `allowed_values_listed_in`: `sales_territory_rules` |
| `title` | text | Job Title | no | no |  |  | `purpose`: `job_title` |
| `unbilledorders` | currency | Unbilled Orders | no | no |  |  | `purpose`: `unbilled_orders_total`; `effect`: `in_credit_limit` (when `credit_limit_includes_orders`); `value_unit`: `customer_currency` (when `multiple_currencies`); `configured_by`: `customer_credit_limit_includes_orders` |
| `unsubscribe` | select | Unsubscribe from Campaigns | no | no |  |  | `purpose`: `unsubscribed_from_marketing`; `true_means`: `no_campaign_email`; `value_source`: `unsubscribe_link_in_campaign_email`; `configured_by`: `unsubscribed_to_marketing_by_default` (when `us_edition`); `caveat`: `resubscribe_only_via_customer_center_or_email_link` |
| `url` | url | Web Address | no | yes |  |  | `purpose`: `customer_url` |
| `vatregnumber` | text | Tax Reg. Number | no | no |  |  | `purpose`: `tax_registration_number` |
| `visits` | integer | Number of Visits | no | no |  |  | `purpose`: `web_visit_count`; `value_source`: `web_site_visit_tracking`; `caveat`: `new_visit_after_leaving_and_returning` |
| `weblead` | text | Web Lead | no | no |  |  | `purpose`: `created_by_web_registration` |
| `currencyprecision` | text | Currency Precision | no | no |  |  |  |
| `defaultbankaccount` | select | Bank Account | no | no |  |  |  |
| `externalid` | text | ExternalId | no | no |  |  |  |
| `isjob` | text |  | no | no |  |  |  |
| `lastmodifieddate` | datetime | Last Modified Date | no | no |  |  |  |
| `middlename` | text |  | no | no |  |  |  |
| `monthlyclosing` | select | Monthly Closing Date | yes | yes |  |  |  |
| `stage` | text |  | no | no |  |  |  |
| `strength` | text | Password Strength | no | no |  |  |  |
| `syncsalesteams` | checkbox | Update Transactions | no | no |  |  |  |
| `taxexempt` | checkbox | PST Exempt | no | no |  |  |  |
| `taxfractionunit` | select | Tax Rounding Precision | no | no |  |  |  |
| `taxrounding` | select | Tax Rounding Method | no | no |  |  |  |
| `password` |  | Password |  |  |  |  |  |
| `password2` |  | Confirm Password |  |  |  |  |  |
| `ccnumber` |  | Credit Card Number |  |  |  |  |  |

## Sublist `addressbook` (Address)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `addressbookaddress` | summary | Edit | no |  |  |
| `addressid` | text |  | no |  |  |
| `defaultbilling` | checkbox | Default Billing | no |  |  |
| `defaultshipping` | checkbox | Default Shipping | no |  |  |
| `id` | integer | ID | no |  |  |
| `internalid` | integer |  | no |  |  |
| `isresidential` | checkbox | Residential Address | no |  |  |
| `label` | text | Label | no |  |  |

## Sublist `contactroles` (Contacts)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `contact` | select | Contact | no |  |  |
| `email` | email | Email | no |  |  |
| `giveaccess` | checkbox | Access | no |  |  |
| `passwordconfirm` | password | Confirm Password | no |  |  |
| `role` | select | Role | no |  |  |
| `sendemail` | checkbox | Notify | no |  |  |
| `strength` | text | Password Strength | no |  |  |

## Sublist `currency` (Currencies)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `balance` | currency | Balance | no |  |  |
| `consolbalance` | currency | Consolidated | no |  |  |
| `consoldepositbalance` | currency | Consolidated | no |  |  |
| `consoloverduebalance` | currency | Consolidated | no |  |  |
| `consolunbilledorders` | currency | Consolidated | no |  |  |
| `currency` | select | Currency | yes |  |  |
| `currencyformatsample` | text | Format Sample | no |  |  |
| `depositbalance` | currency | Deposit Balance | no |  |  |
| `displaysymbol` | text | Symbol | no |  |  |
| `overduebalance` | currency | Overdue Balance | no |  |  |
| `overridecurrencyformat` | checkbox | Override Currency Format | no |  |  |
| `symbolplacement` | select | Symbol Placement | no |  |  |
| `unbilledorders` | currency | Unbilled Orders | no |  |  |

## Sublist `download` (Downloads)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `expiration` | date | Expiration Date | no |  |  |
| `file` | select | Attach File | no |  |  |
| `licensecode` | textarea | License Code | no |  |  |
| `remainingdownloads` | integer | Remaining Downloads | no |  |  |

## Sublist `grouppricing` (Group Pricing)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `group` | select | Pricing Group | yes |  |  |
| `level` | select | Price Level | yes |  |  |

## Sublist `itempricing` (Item Pricing)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `currency` | select | Currency | no |  |  |
| `item` | select | Item | yes |  |  |
| `level` | select | Price Level | yes |  |  |
| `price` | currency2 | Unit Price | no |  |  |

## Sublist `partners` (Partners)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `contribution` | percent | Contribution % | yes |  |  |
| `customer` | text |  | no |  |  |
| `id` | text |  | no |  |  |
| `isprimary` | checkbox | Primary | no |  |  |
| `partner` | select | Partner | yes |  |  |
| `partnerrole` | select | Partner Role | no |  |  |

## Sublist `salesteam` (Sales Team)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `contribution` | percent | Contribution % | yes |  |  |
| `customer` | text |  | no |  |  |
| `employee` | select | Employee | yes |  |  |
| `id` | text |  | no |  |  |
| `isprimary` | checkbox | Primary | no |  |  |
| `issalesrep` | text |  | no |  |  |
| `salesrole` | select | Sales Role | yes |  |  |

## Tabs

| Tab ID | Label |
|---|---|
| `access` | Access |
| `address` | Address |
| `custom` | Custom |
| `financial` | Financial |
| `general` | General |
| `info` | Info |
| `marketing` | Marketing |
| `qualification` | Qualification |
| `sales` | Sales |
| `support` | Support |
| `workflow` | Workflow |

## Transform targets

| Target record | Default fields |
|---|---|
| `opportunity` | `customform` |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `accountnumber` | text | Account |  |
| `address` | textarea | Address |  |
| `addressee` | text | Addressee |  |
| `addresslabel` | text | Address Label |  |
| `addressphone` | phone | Address Phone |  |
| `assignedsite` | select | Assigned Web Site |  |
| `attention` | text | Attention |  |
| `availableoffline` | checkbox | Always Available Offline |  |
| `balance` | currency | Balance |  |
| `boughtamount` | currency | Total Amount Purchased |  |
| `boughtdate` | date | Purchase Dates |  |
| `buyingreason` | select | Buying Reason |  |
| `buyingtimeframe` | select | Buying Time Frame |  |
| `category` | select | Category |  |
| `cccustomercode` | text | CC Customer Code |  |
| `ccdefault` | checkbox | Default Credit Card |  |
| `ccexpdate` | date | CC Expire Date |  |
| `ccholdername` | text | Credit Cardholder Name |  |
| `ccnumber` | ccnumber | Credit Card Number |  |
| `cctype` | select | Credit Card Type |  |
| `city` | text | City |  |
| `classbought` | multiselect | Class |  |
| `comments` | textarea | Comments |  |
| `companyname` | text | Company Name |  |
| `consolbalance` | currency | Consolidated Balance |  |
| `consoldaysoverdue` | integer | Consolidated Days Overdue |  |
| `consoldepositbalance` | currency | Consolidated Deposit Balance |  |
| `consoloverduebalance` | currency | Consolidated Overdue Balance |  |
| `consolunbilledorders` | currency | Consolidated Unbilled Orders |  |
| `contact` | text | Contact |  |
| `contribution` | integer | Contribution % |  |
| `conversiondate` | date | Conversion Date |  |
| `country` | select | Country |  |
| `county` | text | County/State |  |
| `credithold` | select | Credit Hold |  |
| `creditholdoverride` | checkbox | Override Credit Hold Off |  |
| `creditlimit` | currency | Credit Limit |  |
| `currency` | select | Currency |  |
| `custstage` | select | Parent Stage |  |
| `custstatus` | select | Parent Status |  |
| `dateclosed` | datetime | Date Closed |  |
| `datecreated` | datetime | Date Created |  |
| `daysoverdue` | integer | Days Overdue |  |
| `depositbalance` | currency | Deposit Balance |  |
| `deptbought` | multiselect | Department |  |
| `draccount` | select | Deferred Revenue Reclassification Account |  |
| `email` | text | Email |  |
| `emailpreference` | select | Email Preference |  |
| `emailtransactions` | checkbox | Email Transactions |  |
| `enddate` | date | End Date |  |
| `entityid` | text | Name/ID |  |
| `entitystatus` | select | Status |  |
| `estimatedbudget` | currency | Estimated Budget |  |
| `explicitconversion` | checkbox | Explicit Conversion |  |
| `externalid` | select | External ID |  |
| `externalidstring` | text | External ID (Text) |  |
| `fax` | phone | Fax |  |
| `faxtransactions` | checkbox | Fax Transactions |  |
| `firstname` | text | First Name |  |
| `firstorderdate` | date | Date of First Order |  |
| `firstsaledate` | date | Date of First Sale |  |
| `formuladate` | date | Formula (Date) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulatext` | text | Formula (Text) |  |
| `fxaccount` | select | Foreign Currency Adjustment Revenue Account |  |
| `fxbalance` | currency | Balance (Foreign Currency) |  |
| `fxconsolbalance` | currency | Consolidated Balance (Foreign Currency) |  |
| `fxconsolunbilledorders` | currency | Consolidated Unbilled Orders (Foreign Currency) |  |
| `fxunbilledorders` | currency | Unbilled Orders (Foreign Currency) |  |
| `giveaccess` | checkbox | Login Access |  |
| `globalsubscriptionstatus` | select | Global Subscription Status |  |
| `group` | multiselect | Group |  |
| `grouppricinglevel` | select | Group Pricing Level |  |
| `hasduplicates` | checkbox | Duplicate |  |
| `image` | image | Image |  |
| `internalid` | select | Internal ID |  |
| `internalidnumber` | integer | Internal ID (Number) |  |
| `isbudgetapproved` | checkbox | Budget Approved |  |
| `isdefaultbilling` | checkbox | Default Billing Address |  |
| `isdefaultshipping` | checkbox | Default Shipping Address |  |
| `isinactive` | checkbox | Inactive |  |
| `isjob` | checkbox | Is Job |  |
| `isperson` | checkbox | Is Individual |  |
| `isreportedlead` | checkbox | Included in Lead Reports |  |
| `isshipaddress` | checkbox | Residential Address |  |
| `itempricinglevel` | select | Item Pricing Level |  |
| `itempricingunitprice` | currency | Item Pricing Unit Price |  |
| `itemsbought` | multiselect | Items Purchased |  |
| `itemsordered` | multiselect | Items Ordered |  |
| `jobenddate` | date | Actual End Date |  |
| `jobestcost` | currency | Estimated Cost |  |
| `jobestenddate` | date | Projected End Date |  |
| `jobestrevenue` | currency | Estimated Revenue |  |
| `jobpctcomplete` | integer | Percent Complete |  |
| `jobstartdate` | date | Start Date |  |
| `jobstatus` | select | Job Status |  |
| `jobtype` | select | Job Type |  |
| `language` | select | Language |  |
| `lastmodifieddate` | datetime | Last Modified |  |
| `lastname` | text | Last Name |  |
| `lastorderdate` | date | Date of Last Order |  |
| `lastsaledate` | date | Date of Last Sale |  |
| `leaddate` | datetime | Lead Date |  |
| `leadsource` | select | Lead Source |  |
| `level` | select | Level |  |
| `locationbought` | multiselect | Location |  |
| `manualcredithold` | checkbox | Override Credit Hold On |  |
| `merchantaccount` | select | Credit Card Processor |  |
| `middlename` | text | Middle Name |  |
| `monthlyclosing` | select | Monthly Closing Date |  |
| `oncredithold` | checkbox | On Credit Hold |  |
| `onlineformdate` | date | Online Form History - Form Date |  |
| `onlineformleadsource` | select | Online Form History - Lead Source |  |
| `onlineformname` | select | Online Form History - Form Name |  |
| `orderedamount` | currency | Total Amount Ordered |  |
| `ordereddate` | date | Order Dates |  |
| `otherrelationships` | multiselect | Other Relationships |  |
| `overduebalance` | currency | Overdue Balance |  |
| `parent` | select | Sub of |  |
| `parentitemsbought` | multiselect | Items or Sub-Items Purchased |  |
| `parentitemsordered` | multiselect | Items or Sub-Items Ordered |  |
| `partner` | select | Partner |  |
| `partnercontribution` | integer | Partner Contribution % |  |
| `partnerrole` | select | Partner Role |  |
| `partnerteammember` | select | Partner Team Member |  |
| `pec` | text | PEC |  |
| `permission` | select | Permission |  |
| `phone` | phone | Phone |  |
| `phoneticname` | text | Phonetic Name |  |
| `pricelevel` | select | Price Level |  |
| `pricinggroup` | select | Pricing Group |  |
| `pricingitem` | select | Pricing Item |  |
| `printtransactions` | checkbox | Print Transactions |  |
| `prospectdate` | datetime | Prospect Date |  |
| `pstexempt` | checkbox | PST Exempt |  |
| `receivablesaccount` | select | Default Receivables Account |  |
| `reminderdate` | date | Renewal Reminder Date |  |
| `representingsubsidiary` | select | Represents Subsidiary |  |
| `resalenumber` | text | Resale Number |  |
| `role` | select | Role |  |
| `salesreadiness` | select | Sales Readiness |  |
| `salesrep` | select | Sales Rep |  |
| `salesteammember` | select | Sales Team Member |  |
| `salesteamrole` | select | Sales Team Role |  |
| `salutation` | text | Mr./Mrs... |  |
| `shipcomplete` | checkbox | Ship Complete |  |
| `shippingcarrier` | select | Shipping Carrier |  |
| `shippingitem` | select | Shipping Item |  |
| `sourcesite` | select | Source Website |  |
| `stage` | select | Stage |  |
| `startdate` | date | Start Date |  |
| `state` | select | State/Province |  |
| `subsidbought` | multiselect | Subsidiaries Bought |  |
| `subsidiary` | select | Subsidiary |  |
| `taxable` | checkbox | Taxable |  |
| `terms` | select | Terms |  |
| `territory` | select | Territory |  |
| `title` | text | Job Title |  |
| `type` | select | Entity Type |  |
| `unbilledorders` | currency | Unbilled Orders |  |
| `url` | url | Web Address |  |
| `vatregnumber` | text | Tax Number |  |
| `weblead` | checkbox | Web Lead |  |
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
| `assignedsite` | select | Assigned Web Site |  |
| `attention` | text | Attention |  |
| `availableoffline` | checkbox | Always Available Offline |  |
| `balance` | currency | Balance |  |
| `billcountrycode` | text | Billing Country Code |  |
| `billzipcode` | text | Billing Zip |  |
| `buyingreason` | text | Buying Reason |  |
| `buyingtimeframe` | text | Buying Time Frame |  |
| `category` | select | Category |  |
| `cccustomercode` | text | CC Customer Code |  |
| `ccdefault` | checkbox | Default Credit Card |  |
| `ccexpdate` | mmyydate | CC Expire Date |  |
| `ccholdername` | text | Credit Cardholder Name |  |
| `ccinternalid` | text | Credit Card Internal ID |  |
| `ccnumber` | ccnumber | Credit Card Number |  |
| `cctype` | select | Credit Card Type |  |
| `city` | text | City |  |
| `comments` | text | Comments |  |
| `companyname` | text | Company Name |  |
| `consolbalance` | currency | Consolidated Balance |  |
| `consoldaysoverdue` | integer | Consolidated Days Overdue |  |
| `consoldepositbalance` | currency | Consolidated Deposit Balance |  |
| `consoloverduebalance` | currency | Consolidated Overdue Balance |  |
| `consolunbilledorders` | currency | Consolidated Unbilled Orders |  |
| `contact` | select | Primary Contact |  |
| `contribution` | percent | Contribution % |  |
| `contributionprimary` | percent | Primary Sales Rep Contribution % |  |
| `conversiondate` | date | Conversion Date |  |
| `country` | select | Country |  |
| `countrycode` | text | Country Code |  |
| `credithold` | select | Credit Hold |  |
| `creditholdoverride` | checkbox | Override Credit Hold Off |  |
| `creditlimit` | currency | Credit Limit |  |
| `currency` | select | Currency |  |
| `dateclosed` | datetime | Date Closed |  |
| `datecreated` | datetime | Date Created |  |
| `daysoverdue` | integer | Days Overdue |  |
| `depositbalance` | currency | Deposit Balance |  |
| `draccount` | text | Deferred Revenue Reclassification Account |  |
| `email` | email | Email |  |
| `emailpreference` | select | Email Preference |  |
| `emailtransactions` | checkbox | Email Transactions |  |
| `enddate` | date | End Date |  |
| `entityid` | text | Name |  |
| `entitynumber` | integer | Number |  |
| `entitystatus` | select | Status |  |
| `estimatedbudget` | currency | Estimated Budget |  |
| `explicitconversion` | checkbox | Explicit Conversion |  |
| `externalid` | select | External ID |  |
| `fax` | text | Fax |  |
| `faxtransactions` | checkbox | Fax Transactions |  |
| `firstname` | text | First Name |  |
| `firstorderdate` | date | Date of First Order |  |
| `firstsaledate` | date | Date of First Sale |  |
| `formulacurrency` | currency | Formula (Currency) |  |
| `formuladate` | date | Formula (Date) |  |
| `formuladatetime` | datetime | Formula (Date/Time) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulapercent` | percent | Formula (Percent) |  |
| `formulatext` | text | Formula (Text) |  |
| `fxaccount` | text | Foreign Currency Adjustment Revenue Account |  |
| `fxbalance` | currency | Balance (Foreign Currency) |  |
| `fxconsolbalance` | currency | Consolidated Balance (Foreign Currency) |  |
| `fxconsolunbilledorders` | currency | Consolidated Unbilled Orders (Foreign Currency) |  |
| `fxunbilledorders` | currency | Unbilled Orders (Foreign Currency) |  |
| `giveaccess` | checkbox | Login Access |  |
| `globalsubscriptionstatus` | select | Global Subscription Status |  |
| `grouppricinglevel` | text | Group Pricing Level |  |
| `hasduplicates` | checkbox | Duplicate |  |
| `homephone` | phone | Home Phone |  |
| `image` | select | Image |  |
| `internalid` | select | Internal ID |  |
| `isbudgetapproved` | checkbox | Budget Approved |  |
| `isdefaultbilling` | checkbox | Default Billing Address |  |
| `isdefaultshipping` | checkbox | Default Shipping Address |  |
| `isinactive` | checkbox | Inactive |  |
| `isperson` | checkbox | Is Individual |  |
| `isshipaddress` | checkbox | Residential Address |  |
| `itempricinglevel` | text | Item Pricing Level |  |
| `itempricingunitprice` | currency | Item Pricing Unit Price |  |
| `jobenddate` | date | Job Actual End |  |
| `jobprojectedend` | date | Job Projected End |  |
| `jobstartdate` | date | Job Start |  |
| `jobtype` | select | Job Type |  |
| `language` | select | Language |  |
| `lastmodifieddate` | datetime | Last Modified |  |
| `lastname` | text | Last Name |  |
| `lastorderdate` | date | Date of Last Order |  |
| `lastsaledate` | date | Date of Last Sale |  |
| `lastviewed` | datetime | Last Viewed |  |
| `leaddate` | datetime | Lead Date |  |
| `leadsource` | select | Lead Source |  |
| `level` | select | Level |  |
| `manualcredithold` | checkbox | Override Credit Hold On |  |
| `middlename` | text | Middle Name |  |
| `mobilephone` | phone | Mobile Phone |  |
| `monthlyclosing` | select | Monthly Closing Date |  |
| `oncredithold` | checkbox | On Credit Hold |  |
| `onlineformdate` | text | Online Form History - Date |  |
| `onlineformleadsource` | text | Online Form History - Lead Source |  |
| `onlineformname` | text | Online Form History - Form Name |  |
| `overduebalance` | currency | Overdue Balance |  |
| `parent` | select | Top Level Parent |  |
| `partner` | select | Partner |  |
| `partnercontribution` | percent | Partner Contribution % |  |
| `partnerrole` | text | Partner Role |  |
| `partnerteammember` | select | Partner Team Member |  |
| `pec` | text | PEC |  |
| `permission` | select | Permission |  |
| `phone` | phone | Phone |  |
| `phoneticname` | text | Phonetic Name |  |
| `prefccprocessor` | select | Credit Card Processor |  |
| `pricelevel` | select | Price Level |  |
| `pricinggroup` | text | Pricing Group |  |
| `pricingitem` | text | Pricing Item |  |
| `printtransactions` | checkbox | Print Transactions |  |
| `prospectdate` | datetime | Prospect Date |  |
| `receivablesaccount` | text | Default Receivables Account |  |
| `reminderdays` | integer | Reminder Days |  |
| `representingsubsidiary` | select | Represents Subsidiary |  |
| `resalenumber` | text | Resale Number |  |
| `role` | text | Role |  |
| `salesreadiness` | text | Sales Readiness |  |
| `salesrep` | select | Sales Rep |  |
| `salesteammember` | select | Sales Team Member |  |
| `salesteamrole` | select | Sales Team Role |  |
| `salutation` | text | Mr./Mrs... |  |
| `shipcomplete` | checkbox | Ship Complete |  |
| `shipcountrycode` | text | Shipping Country Code |  |
| `shippingcarrier` | select | Shipping Carrier |  |
| `shippingitem` | select | Shipping Item |  |
| `sourcesite` | select | Source Website |  |
| `stage` | select | Stage |  |
| `startdate` | date | Start Date |  |
| `state` | select | State/Province |  |
| `statedisplayname` | select | State/Province Display Name |  |
| `subscription` | select | Subscription |  |
| `subscriptiondate` | datetime | Subscription Date |  |
| `subscriptionstatus` | checkbox | Subscription Status |  |
| `subsidiary` | select | Subsidiary |  |
| `subsidiarynohierarchy` | select | Subsidiary (no hierarchy) |  |
| `taxable` | checkbox | Taxable |  |
| `taxitem` | select | Tax Item |  |
| `terms` | select | Terms |  |
| `territory` | select | Territory |  |
| `title` | text | Job Title |  |
| `type` | select | Type |  |
| `unbilledorders` | currency | Unbilled Orders |  |
| `url` | text | Web Address |  |
| `vatregnumber` | text | Tax Number |  |
| `weblead` | checkbox | Web Lead |  |
| `zipcode` | text | Zip Code |  |

## Search joins

| Join ID | Label | Joined record |
|---|---|---|
| `campaignresponse` | Campaign Response | Campaign |
| `contact` | Contact | Contact |
| `contactprimary` | Contact (Primary) | Contact |
| `job` | Job | Job |
| `leadsource` | Lead Source | Campaign |
| `messages` | Messages | Message |
| `messagesfrom` | Messages From | Message |
| `messagesto` | Messages To | Message |
| `msesubsidiary` | Subsidiary | Subsidiary |
| `originatinglead` | Originating Lead | Lead |
| `parentcustomer` | Parent Customer | Customer |
| `partner` | Partner | Partner |
| `pricing` | Pricing | Pricing |
| `salesrep` | Sales Rep | Employee |
| `subcustomer` | Sub Customer | Customer |
| `task` | Task | Task |
| `toplevelparent` | Top Level Parent | Customer |
| `websitecategory` | Web Site Category | Presentation Category |

## Suspected source errors

> [!SOURCE-ERROR] `access_sublist_note_says_customer_record`
<!-- source-error: record:lead | - | access_sublist_note_says_customer_record -->

> [!SOURCE-ERROR] `unresolved_navigation_placeholders` (on `category`)
<!-- source-error: record:lead | category | unresolved_navigation_placeholders -->

> [!SOURCE-ERROR] `says_cannot_change_then_can_change` (on `currency`)
<!-- source-error: record:lead | currency | says_cannot_change_then_can_change -->

> [!SOURCE-ERROR] `unresolved_navigation_placeholders` (on `language`)
<!-- source-error: record:lead | language | unresolved_navigation_placeholders -->
