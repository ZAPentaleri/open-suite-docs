# Other Name (`othername`)
<!-- id: record:othername -->

| Fact | Value |
|---|---|
| Internal ID | `othername` |
| Category | entity |
| Client scripts | no |
| Server scripts | yes |
| Searchable | yes |
| Tags | `custom_fields_supported`, `scripting_full` |

## Record facts

- `script_types`: `server`
- `script_functions`: `create`, `update`, `copy`, `delete`, `search`
- `doc_inconsistency`: `functions_sentence_names_lead_record`
- `purpose`: `people_or_companies_not_vendor_customer_or_employee`
- `value_examples`: `charity_receiving_donations`, `owners_contributing_equity`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `accountnumber` | text | Account ID | no | yes |  | `purpose`: `customer_account_number` |
| `autoname` | checkbox | Auto | no | no |  | `purpose`: `auto_name`; `true_means`: `name_from_auto_generated_numbers`; `false_means`: `enter_name_manually`; `configured_by`: `auto_generated_numbers_setup` |
| `bcn` | text | Business Number | no | no |  | `purpose`: `business_number` |
| `category` | select | Category | no | yes |  | `purpose`: `other_name_category` |
| `comments` | textarea | Comments | no | yes |  | `purpose`: `comments` |
| `companyname` | text | Company Name | no | yes |  | `purpose`: `legal_name`; `required_when`: `online_bill_pay_feature` |
| `contact` | select | Primary Contact | no | no |  | `purpose`: `check_contact_name`; `displayed_on`: `checks_below_mailing_address` |
| `creditlimit` | currency | Credit Limit | no | no |  | `purpose`: `credit_limit`; `effect`: `warns_or_blocks_transactions_at_limit`; `configured_by`: `customer_credit_limit_handling_preference` |
| `currency` | select | Currency | yes | yes |  | `purpose`: `currency`; `cannot`: `change_after_transactions_saved`; `effect`: `all_transactions_in_this_currency`; `effect`: `converted_to_base_currency_in_registers_and_reports`; `allowed_values_listed_in`: `currency_records` |
| `defaultaddress` | address | Address | no | no |  | `purpose`: `default_billing_address`; `value_source`: `address_sublist_default_billing` |
| `email` | email | Other Name Email | no | no |  | `purpose`: `email_address` |
| `emailpreference` | select | Email Preference | no | no |  | `purpose`: `email_format_preference`; `options`: `HTML`, `other_formats`; `requires`: `recipient_html_capable` (when `html`); `default_behavior`: `user_email_preference` |
| `entityid` | text | Other Name | yes | yes |  | `purpose`: `entity_name`; `displayed_on`: `lists`; `required_when`: `online_bill_pay_feature` |
| `fax` | phone | Fax | no | yes |  | `purpose`: `fax_number`; `format`: `as_dialed_including_prefix`; `ui_effect`: `fills_to_be_faxed_on_transactions`; `requires`: `fax_service_set_up` (when `faxing_forms`) |
| `firstname` | text | First Name | no | no |  | `purpose`: `first_name` |
| `globalsubscriptionstatus` | select | Global Subscription Status | no | no |  | `purpose`: `global_subscription_status`; `allowed_values`: `Confirmed Opt-In`, `Soft Opt-In`, `Soft Opt-Out`, `Confirmed Opt-Out`; `constraint`: `only_recipient_sets_confirmed_statuses`; `can`: `set_soft_statuses_manually_or_mass_update`; `caveat`: `re_opt_in_only_via_old_campaign_link` |
| `image` | select | Image | no | no |  | `purpose`: `record_image`; `value_source`: `file_cabinet_image`; `ui_effect`: `new_option_creates_record` |
| `isinactive` | checkbox | Other Name Is Inactive | no | yes |  | `purpose`: `inactivate_record`; `ui_effect`: `hidden_from_lists_unless_show_inactives`; `doc_inconsistency`: `help_says_item` |
| `isperson` | radio | Company | no | no |  | `purpose`: `company_or_individual`; `allowed_values`: `Company`, `Individual`; `ui_effect`: `changes_fields_and_subtabs` |
| `openingbalance` | currency | Opening Balance | no | no |  | `purpose`: `opening_balance` |
| `openingbalanceaccount` | select | Opening Balance Account | no | no |  | `purpose`: `opening_balance_account` |
| `openingbalancedate` | date | Opening Balance Date | no | no |  | `purpose`: `opening_balance_date` |
| `otherrelationships` | select | Other Relationships | no | no |  | `purpose`: `other_records_for_same_entity`; `value_source`: `system_displayed`; `ui_effect`: `add_new_creates_other_record_type` |
| `phone` | phone | Phone | no | yes |  | `purpose`: `phone`; `required_when`: `online_bill_pay_feature` |
| `phoneticname` | text | Furigana | no | no |  | `purpose`: `furigana_sort_name` |
| `printoncheckas` | text | Print on Check As | no | no |  | `purpose`: `print_on_check_as`; `displayed_on`: `checks` |
| `subsidiary` | select | Subsidiary | yes | no |  | `purpose`: `subsidiary`; `requires`: `set_before_entering_transactions`; `effect`: `transactions_associated_with_subsidiary`; `cannot`: `change_after_transaction_posted` |
| `taxfractionunit` | select | Consumption Tax Fraction Unit | no | no |  | `purpose`: `tax_calculation_precision` |
| `taxidnum` | text | Tax ID | no | no |  | `purpose`: `tax_id_number`; `accepts_forms`: `ssn_for_individuals`; `required_when`: `issuing_1099` |
| `taxrounding` | select | Tax Rounding | no | no |  | `purpose`: `tax_rounding_method` |
| `terms` | select | Terms | no | no |  | `purpose`: `default_terms`; `can`: `change_per_sale`; `allowed_values_listed_in`: `accounting_list_terms`; `doc_inconsistency`: `help_path_says_crm_lists` |
| `url` | url | URL | no | no |  | `purpose`: `web_address`; `ui_effect`: `shown_as_link_in_view_mode` |
| `vatregnumber` | text | VAT Registration No. | no | no |  | `purpose`: `vat_registration_number` |
| `altemail` | email | Alt. Email | no | yes |  |  |
| `balance` | currency | Balance | no | no |  |  |
| `datecreated` | datetime | Date Created | no | no |  |  |
| `entitystatus` | integer | Status | no | no |  |  |
| `externalid` | text | ExternalId | no | no |  |  |
| `homephone` | phone | Home Phone | no | no |  |  |
| `lastmodifieddate` | datetime | Last Modified Date | no | no |  |  |
| `lastname` | text |  | no | no |  |  |
| `middlename` | text |  | no | no |  |  |
| `mobilephone` | phone | Mobile Phone | no | no |  |  |
| `parent` | integer | Parent | no | no |  |  |
| `salesrep` | integer | Sales Rep | no | yes |  |  |
| `salutation` | text | Mr./Ms... | no | no |  |  |
| `title` | text | Job Title | no | no |  |  |
| `unsubscribe` | select | Unsubscribe from Campaigns | no | no |  |  |

## Sublist `addressbook` (Address)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `addressbookaddress` | summary | Edit | no |  |  |
| `addressid` | text |  | no |  |  |
| `defaultbilling` | checkbox | Default Billing | no |  |  |
| `defaultshipping` | checkbox | Default Shipping | no |  |  |
| `id` | integer | ID | no |  |  |
| `internalid` | integer |  | no |  |  |
| `label` | text | Label | no |  |  |

## Tabs

| Tab ID | Label |
|---|---|
| `access` | System Access |
| `address` | Address |
| `custom` | Custom |
| `financial` | Financial |
| `general` | General |
| `info` | Info |
| `marketing` | Marketing |
| `s_comm` | Communications |
| `s_relation` | Relationships |
| `s_sysinfo` | System Information |
| `sales` | Sales |
| `support` | Support |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `accountnumber` | text | Account |  |
| `address` | textarea | Address |  |
| `addressee` | text | Addressee |  |
| `addresslabel` | text | Address Label |  |
| `addressphone` | phone | Address Phone |  |
| `attention` | text | Attention |  |
| `category` | select | Category |  |
| `city` | text | City |  |
| `comments` | textarea | Comments |  |
| `country` | select | Country |  |
| `county` | text | County/State |  |
| `currency` | select | Currency |  |
| `datecreated` | datetime | Date Created |  |
| `email` | text | Email |  |
| `entityid` | text | Name/ID |  |
| `externalid` | select | External ID |  |
| `externalidstring` | text | External ID (Text) |  |
| `fax` | phone | Fax |  |
| `formuladate` | date | Formula (Date) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulatext` | text | Formula (Text) |  |
| `giveaccess` | checkbox | Login Access |  |
| `globalsubscriptionstatus` | select | Global Subscription Status |  |
| `image` | image | Image |  |
| `internalid` | select | Internal ID |  |
| `internalidnumber` | integer | Internal ID (Number) |  |
| `isdefaultbilling` | checkbox | Default Billing Address |  |
| `isdefaultshipping` | checkbox | Default Shipping Address |  |
| `isinactive` | checkbox | Inactive |  |
| `language` | select | Language |  |
| `lastmodifieddate` | datetime | Last Modified |  |
| `level` | select | Level |  |
| `permission` | select | Permission |  |
| `phone` | phone | Phone |  |
| `phoneticname` | text | Phonetic Name |  |
| `representingsubsidiary` | select | Represents Subsidiary |  |
| `salesrep` | select | Sales Rep |  |
| `state` | select | State/Province |  |
| `subsidiary` | select | Subsidiary |  |
| `type` | select | Type |  |
| `url` | url | Web Address |  |
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
| `altemail` | email | Alt. Email |  |
| `altname` | text | Name |  |
| `altphone` | phone | Office Phone |  |
| `attention` | text | Attention |  |
| `billcountrycode` | text | Billing Country Code |  |
| `billzipcode` | text | Billing Zip |  |
| `category` | text | Category |  |
| `city` | text | City |  |
| `comments` | text | Comments |  |
| `companyname` | text | Company Name |  |
| `country` | select | Country |  |
| `countrycode` | text | Country Code |  |
| `currency` | text | Currency |  |
| `datecreated` | datetime | Date Created |  |
| `email` | email | Email |  |
| `entityid` | text | Name |  |
| `entitynumber` | integer | Number |  |
| `entitystatus` | text | Status |  |
| `externalid` | select | External ID |  |
| `fax` | text | Fax |  |
| `formulacurrency` | currency | Formula (Currency) |  |
| `formuladate` | date | Formula (Date) |  |
| `formuladatetime` | datetime | Formula (Date/Time) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulapercent` | percent | Formula (Percent) |  |
| `formulatext` | text | Formula (Text) |  |
| `globalsubscriptionstatus` | select | Global Subscription Status |  |
| `image` | select | Image |  |
| `internalid` | select | Internal ID |  |
| `isdefaultbilling` | checkbox | Default Billing Address |  |
| `isdefaultshipping` | checkbox | Default Shipping Address |  |
| `isinactive` | checkbox | Inactive |  |
| `language` | select | Language |  |
| `lastmodifieddate` | datetime | Last Modified |  |
| `lastviewed` | datetime | Last Viewed |  |
| `level` | select | Level |  |
| `notsfastatus` | text | Status |  |
| `permission` | select | Permission |  |
| `phone` | phone | Phone |  |
| `phoneticname` | text | Phonetic Name |  |
| `representingsubsidiary` | select | Represents Subsidiary |  |
| `salesrep` | select | Sales Rep |  |
| `shipcountrycode` | text | Shipping Country Code |  |
| `state` | select | State/Province |  |
| `statedisplayname` | select | State/Province Display Name |  |
| `subscription` | select | Subscription |  |
| `subscriptiondate` | datetime | Subscription Date |  |
| `subscriptionstatus` | checkbox | Subscription Status |  |
| `subsidiary` | select | Subsidiary |  |
| `subsidiarynohierarchy` | select | Subsidiary (no hierarchy) |  |
| `type` | text | Type |  |
| `url` | text | Web Address |  |
| `zipcode` | text | Zip Code |  |

## Search joins

| Join ID | Label | Joined record |
|---|---|---|
| `campaignresponse` | Campaign Response | Campaign |
| `contact` | Contact | Contact |
| `messages` | Messages | Message |
| `messagesfrom` | Messages From | Message |
| `messagesto` | Messages To | Message |

## Suspected source errors

> [!SOURCE-ERROR] `functions_sentence_names_lead_record`
<!-- source-error: record:othername | - | functions_sentence_names_lead_record -->

> [!SOURCE-ERROR] `help_says_item` (on `isinactive`)
<!-- source-error: record:othername | isinactive | help_says_item -->

> [!SOURCE-ERROR] `help_path_says_crm_lists` (on `terms`)
<!-- source-error: record:othername | terms | help_path_says_crm_lists -->
