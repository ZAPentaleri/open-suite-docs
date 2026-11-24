# Partner (`partner`)
<!-- id: record:partner -->

| Fact | Value |
|---|---|
| Internal ID | `partner` |
| Category | entity |
| Client scripts | yes |
| Server scripts | yes |
| Searchable | yes |
| Tags | `custom_fields_supported`, `scripting_full` |

## Record facts

- `script_types`: `client`, `server`
- `script_functions`: `create`, `update`, `copy`, `delete`, `search`, `transform`
- `unreadable_in_before_submit_for_external_roles`: `password`, `password2`
- `workflow`: `add_entity`, `add_contacts`, `attach_contacts`, `update_entity_access` [`contact_center_access`]
- `access_sublist`: `contactaccessroles`
- `purpose`: `business_partner_not_customer_or_vendor`
- `displayed_on`: `access_subtab` [`contactaccessroles_fields`]
- `effect`: `notify_true_emails_contact_on_access_change`
- `requires`: `contact_exists_and_attached_before_granting_access`
- `doc_inconsistency`: `access_sublist_note_says_customer_record`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `accessrole` | select | Role | no | no |  | `purpose`: `partner_center_role`; `options`: `Partner Center`, `Advanced Partner Center`; `option_meanings`: `partner_center:subpartners_promotions_reports`, `advanced_partner_center:wider_transactions_and_records` |
| `assigntasks` | checkbox | Assign Tasks | no | yes |  | `purpose`: `assignable_tasks`; `displayed_on`: `advanced_partner_center_tasks_and_calendar` |
| `autoname` | checkbox | Auto | no | no |  | `purpose`: `auto_name`; `true_means`: `name_from_auto_generated_numbers`; `false_means`: `enter_name_manually` |
| `bcn` | text | Business Number | no | no |  | `purpose`: `business_number`; `applies_when`: `partner_in_canada` |
| `category` | select | Category | no | no |  | `purpose`: `partner_category`; `allowed_values_listed_in`: `crm_list_partner_category_role` |
| `class` | select | Class | no | yes |  | `purpose`: `class`; `allowed_values_listed_in`: `class_records`; `ui_effect`: `new_option_creates_record` |
| `comments` | textarea | Comments | no | yes |  | `purpose`: `comments` |
| `companyname` | text | Company Name | no | yes |  | `purpose`: `legal_name`; `recommendation`: `enter_when_auto_numbering` |
| `contact` | select | Primary Contact | no | no |  | `purpose`: `primary_contact`; `displayed_on`: `invoice_billing_address`; `allowed_values_listed_in`: `contact_records`; `ui_effect`: `new_option_creates_record`; `can`: `add_more_on_contacts_subtab` |
| `customform` | select | Template | yes | no |  | `purpose`: `entry_form`; `allowed_values_listed_in`: `custom_entry_forms` |
| `defaultaddress` | address | Address | no | no |  | `purpose`: `default_billing_address`; `value_source`: `address_sublist_default_billing` |
| `defaulttaxreg` | select | Default Tax Reg. | no | no |  | `purpose`: `default_tax_registration` |
| `department` | select | Department | no | yes |  | `purpose`: `department`; `allowed_values_listed_in`: `department_records`; `ui_effect`: `new_option_creates_record` |
| `eligibleforcommission` | checkbox | Eligible for Commission | no | no |  | `purpose`: `eligible_for_commission`; `requires`: `partner_commission_plan` |
| `email` | email | E-Mail | no | no |  | `purpose`: `email_address`; `ui_effect`: `emailable_from_partners_list` |
| `emailpreference` | select | E-Mail Preference | no | yes |  | `purpose`: `email_format_preference`; `options`: `HTML`, `other_formats`; `requires`: `recipient_html_capable` (when `html`); `default_behavior`: `user_email_preference` |
| `entityid` | text | Partner ID | yes | yes |  | `purpose`: `partner_name`; `displayed_on`: `lists`; `value_source`: `auto_generated_number` (when `auto_generated_numbering`); `doc_inconsistency`: `help_says_vendor` |
| `fax` | phone | Fax | no | yes |  | `purpose`: `fax_number`; `format`: `as_dialed_including_prefix`; `ui_effect`: `fills_to_be_faxed_on_transactions`; `requires`: `fax_service_set_up` (when `faxing_forms`) |
| `giveaccess` | checkbox | Give Access | no | no |  | `purpose`: `give_netsuite_access` |
| `globalsubscriptionstatus` | select | Global Subscription Status | no | no |  | `purpose`: `global_subscription_status`; `allowed_values`: `Confirmed Opt-In`, `Soft Opt-In`, `Soft Opt-Out`, `Confirmed Opt-Out`; `constraint`: `only_recipient_sets_confirmed_statuses`; `can`: `set_soft_statuses_manually_or_mass_update`; `caveat`: `re_opt_in_only_via_partner_center_or_old_link` |
| `image` | select | Image | no | no |  | `purpose`: `record_image`; `value_source`: `file_cabinet_image`; `ui_effect`: `new_option_creates_record` |
| `isinactive` | checkbox | Inactive | no | yes |  | `purpose`: `inactivate_partner`; `ui_effect`: `hidden_from_partners_list_unless_show_inactives`; `cannot`: `select_inactive_partner_on_records` |
| `isperson` | radio | Company | no | yes |  | `purpose`: `company_or_individual`; `allowed_values`: `Company`, `Individual`; `ui_effect`: `changes_fields_and_subtabs` |
| `location` | select | Location | no | yes |  | `purpose`: `location`; `allowed_values_listed_in`: `location_records`; `ui_effect`: `new_option_creates_record` |
| `middlename` | text |  | no | no |  | `purpose`: `middle_name` |
| `otherrelationships` | select | Other Relationships | no | no |  | `purpose`: `other_records_for_same_entity`; `value_source`: `system_displayed`; `ui_effect`: `add_new_creates_other_record_type` |
| `parent` | select | Subpartner of | no | no |  | `purpose`: `parent_partner` |
| `partnercode` | text | Partner Code | yes | yes |  | `purpose`: `partner_code`; `default_behavior`: `copied_from_partner_name`; `can`: `override` |
| `phone` | phone | Phone | no | yes |  | `purpose`: `phone`; `displayed_on`: `partners_list`; `required_when`: `online_bill_pay_feature`; `accepts_forms`: `nnn-nnn-nnnn`, `1-nnn-nnn-nnnn`, `(nnn) nnn-nnnn`, `1(nnn) nnn-nnnn`, `nnn-nnn-nnnn ext nnnn` |
| `phoneticname` | text | Furigana | no | no |  | `purpose`: `furigana_sort_name` |
| `printoncheckas` | text | Print on Check As | no | no |  | `purpose`: `print_on_check_as`; `displayed_on`: `check_pay_to_line` |
| `referringurl` | url | Referring URL | no | no |  | `purpose`: `partner_referral_url`; `effect`: `registering_customers_linked_to_partner` |
| `requirepwdchange` | checkbox | Require Password Change On Next Login | no | no |  | `purpose`: `require_password_change`; `effect`: `forces_password_change_at_next_login`; `caveat`: `never_reads_back_as_checked`; `effect`: `save_sets_internal_flag_cleared_after_change` |
| `salutation` | text | Mr./Ms... | no | yes |  | `purpose`: `salutation` |
| `sendemail` | checkbox | Send Notification E-mail | no | no |  | `purpose`: `send_access_email`; `effect`: `emails_access_notice_with_password_link`; `false_means`: `password_must_be_assigned_manually` |
| `subpartnerlogin` | checkbox | Can give login access To Subpartners | no | no |  | `purpose`: `allow_subpartner_creation`; `constraint`: `subpartners_see_parent_data_only` |
| `subsidiary` | select | Subsidiary | yes | no |  | `purpose`: `subsidiary`; `requires`: `set_before_entering_transactions`; `effect`: `transactions_associated_with_subsidiary`; `cannot`: `change_after_transaction_posted` |
| `taxfractionunit` | select | Tax Fraction Unit | no | no |  | `purpose`: `tax_calculation_precision` |
| `taxidnum` | text | Tax ID | no | no |  | `purpose`: `tax_id_number`; `required_when`: `issuing_1099` |
| `taxrounding` | select | Tax Rounding | no | no |  | `purpose`: `tax_rounding_method` |
| `unsubscribe` | select | Unsubscribe from Campaigns | no | no |  | `purpose`: `unsubscribed_from_marketing`; `true_means`: `no_campaign_email`; `value_source`: `unsubscribe_link_in_campaign_email`; `configured_by`: `unsubscribed_to_marketing_by_default` (when `us_edition`) |
| `url` | url | Web Address | no | yes |  | `purpose`: `partner_website`; `format`: `must_start_with_http_or_https_scheme` |
| `vatregnumber` | text | VAT Registration No. | no | no |  | `purpose`: `vat_registration_number`; `applies_when`: `partner_liable_for_vat` |
| `altemail` | email | Alt. E-mail | no | yes |  |  |
| `centertype` | text | Entity Type | no | no |  |  |
| `datecreated` | datetime | Date Created | no | no |  |  |
| `externalid` | text | ExternalId | no | no |  |  |
| `faqty` | text | FAQty | no | no |  |  |
| `faqtymax` | text | FAQtyMax | no | no |  |  |
| `firstname` | text | Name | no | no |  |  |
| `homephone` | phone | Home Phone | no | yes |  |  |
| `isfaqtyenforced` | text | isFAQtyEnforced | no | no |  |  |
| `lastmodifieddate` | datetime | Last Modified Date | no | no |  |  |
| `lastname` | text |  | no | no |  |  |
| `mobilephone` | phone | Mobile Phone | no | yes |  |  |
| `strength` | text | Password Strength | no | no |  |  |
| `title` | text | Job Title | no | no |  |  |
| `washasaccess` | text | washasaccess | no | no |  |  |
| `wasinactive` | text | wasinactive | no | no |  |  |
| `password` |  | Password |  |  |  |  |
| `password2` |  | Confirm Password |  |  |  |  |

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
| `promocodes` | Promotion Codes |
| `support` | Support |
| `workflow` | Workflow |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `address` | textarea | Address |  |
| `addressee` | text | Addressee |  |
| `addresslabel` | text | Address Label |  |
| `addressphone` | phone | Address Phone |  |
| `assigntasks` | checkbox | Assign Tasks |  |
| `attention` | text | Attention |  |
| `category` | select | Category |  |
| `city` | text | City |  |
| `class` | select | Class |  |
| `comments` | textarea | Comments |  |
| `commissionplan` | select | Commission Plan |  |
| `country` | select | Country |  |
| `county` | text | County/State |  |
| `datecreated` | datetime | Date Created |  |
| `defaulttaxreg` | select | Default Tax Reg. |  |
| `defaulttaxregtext` | text | Default Tax Reg. (Text) |  |
| `department` | select | Department |  |
| `eligibleforcommission` | checkbox | Eligible For Commission |  |
| `email` | text | Email |  |
| `emailpreference` | select | Email Preference |  |
| `entityid` | text | Name/ID |  |
| `externalid` | select | External ID |  |
| `externalidstring` | text | External ID (Text) |  |
| `fax` | phone | Fax |  |
| `firstname` | text | First Name |  |
| `formuladate` | date | Formula (Date) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulatext` | text | Formula (Text) |  |
| `giveaccess` | checkbox | Login Access |  |
| `globalsubscriptionstatus` | select | Global Subscription Status |  |
| `group` | multiselect | Group |  |
| `hasduplicates` | checkbox | Duplicate |  |
| `image` | image | Image |  |
| `internalid` | select | Internal ID |  |
| `internalidnumber` | integer | Internal ID (Number) |  |
| `isdefaultbilling` | checkbox | Default Billing Address |  |
| `isdefaultshipping` | checkbox | Default Shipping Address |  |
| `isinactive` | checkbox | Inactive |  |
| `isperson` | checkbox | Is Individual |  |
| `language` | select | Language |  |
| `lastmodifieddate` | datetime | Last Modified |  |
| `lastname` | text | Last Name |  |
| `level` | select | Level |  |
| `location` | select | Location |  |
| `middlename` | text | Middle Name |  |
| `otherrelationships` | multiselect | Other Relationships |  |
| `parent` | select | Sub of |  |
| `partnercode` | text | Code |  |
| `permission` | select | Permission |  |
| `phone` | phone | Phone |  |
| `phoneticname` | text | Phonetic Name |  |
| `promocode` | select | Promotion |  |
| `representingsubsidiary` | select | Represents Subsidiary |  |
| `salutation` | text | Mr./Mrs... |  |
| `state` | select | State/Province |  |
| `subsidiary` | select | Subsidiary |  |
| `title` | text | Job Title |  |
| `type` | select | Entity Type |  |
| `url` | url | Web Address |  |
| `zipcode` | text | Zip |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `address` | text | Address |  |
| `addressee` | text | Addressee |  |
| `addressinternalid` | text | Address Internal ID |  |
| `addresslabel` | text | Address Label |  |
| `addressphone` | text | Address Phone |  |
| `altemail` | email | Alt. Email |  |
| `altname` | text | Name |  |
| `altphone` | phone | Office Phone |  |
| `assigntasks` | checkbox | Assign Tasks |  |
| `attention` | text | Attention |  |
| `billcountrycode` | text | Billing Country Code |  |
| `billzipcode` | text | Billing Zip |  |
| `category` | text | Category |  |
| `city` | text | City |  |
| `class` | select | Class |  |
| `classnohierarchy` | select | Class (no hierarchy) |  |
| `comments` | text | Comments |  |
| `companyname` | text | Company Name |  |
| `country` | select | Country |  |
| `countrycode` | text | Country Code |  |
| `datecreated` | datetime | Date Created |  |
| `defaulttaxreg` | text | Default Tax Reg. |  |
| `department` | select | Department |  |
| `departmentnohierarchy` | select | Department (no hierarchy) |  |
| `eligibleforcommission` | checkbox | Eligible for Commission |  |
| `email` | email | Email |  |
| `emailpreference` | select | Email Preference |  |
| `entityid` | text | Name |  |
| `entitynumber` | integer | Number |  |
| `externalid` | select | External ID |  |
| `fax` | text | Fax |  |
| `firstname` | text | First Name |  |
| `formulacurrency` | currency | Formula (Currency) |  |
| `formuladate` | date | Formula (Date) |  |
| `formuladatetime` | datetime | Formula (Date/Time) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulapercent` | percent | Formula (Percent) |  |
| `formulatext` | text | Formula (Text) |  |
| `giveaccess` | checkbox | Login Access |  |
| `globalsubscriptionstatus` | select | Global Subscription Status |  |
| `hasduplicates` | checkbox | Duplicate |  |
| `homephone` | phone | Home Phone |  |
| `image` | select | Image |  |
| `internalid` | select | Internal ID |  |
| `isdefaultbilling` | checkbox | Default Billing Address |  |
| `isdefaultshipping` | checkbox | Default Shipping Address |  |
| `isinactive` | checkbox | Inactive |  |
| `isperson` | checkbox | Is Individual |  |
| `language` | select | Language |  |
| `lastmodifieddate` | datetime | Last Modified |  |
| `lastname` | text | Last Name |  |
| `lastviewed` | datetime | Last Viewed |  |
| `level` | select | Level |  |
| `location` | select | Location |  |
| `locationnohierarchy` | select | Location (no hierarchy) |  |
| `middlename` | text | Middle Name |  |
| `mobilephone` | phone | Mobile Phone |  |
| `parent` | select | Sub of |  |
| `partnercode` | text | Code |  |
| `permission` | select | Permission |  |
| `phone` | phone | Phone |  |
| `phoneticname` | text | Phonetic Name |  |
| `promocode` | text | Promotion |  |
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
| `title` | text | Job Title |  |
| `type` | select | Type |  |
| `url` | text | Web Address |  |
| `zipcode` | text | Zip Code |  |

## Search joins

| Join ID | Label | Joined record |
|---|---|---|
| `campaignresponse` | Campaign Response | Campaign |
| `contact` | Contact | Contact |
| `contactprimary` | Contact (Primary) | Contact |
| `customer` | Customer | Customer |
| `messages` | Messages | Message |
| `messagesfrom` | Messages From | Message |
| `messagesto` | Messages To | Message |

## Suspected source errors

> [!SOURCE-ERROR] `access_sublist_note_says_customer_record`
<!-- source-error: record:partner | - | access_sublist_note_says_customer_record -->

> [!SOURCE-ERROR] `help_says_vendor` (on `entityid`)
<!-- source-error: record:partner | entityid | help_says_vendor -->
