# Contact (`contact`)
<!-- id: record:contact -->

| Fact | Value |
|---|---|
| Internal ID | `contact` |
| Category | entity |
| Client scripts | yes |
| Server scripts | yes |
| Searchable | yes |
| Tags | `custom_fields_supported`, `scripting_full` |

## Record facts

- `script_types`: `client`, `server`
- `script_functions`: `create`, `update`, `copy`, `delete`, `search`, `transform`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `altemail` | email | Alt. Email | no | yes |  | `purpose`: `alternate_email`; `effect`: `replies_attached_to_contact` (when `capture_email_replies`); `doc_inconsistency`: `stray_html_tag_in_help` |
| `assistant` | select | Assistant | no | no |  | `purpose`: `assistant_contact`; `allowed_values`: `existing_contacts` |
| `assistantphone` | phone | Assist. Phone | no | no |  | `purpose`: `assistant_phone`; `displayed_on`: `this_record_only` |
| `category` | select | Category | no | no |  | `purpose`: `contact_category`; `allowed_values_listed_in`: `crm_list_contact_category` |
| `comments` | textarea | Approach | no | yes |  | `purpose`: `contact_comments`; `displayed_on`: `this_record_only`; `max_length`: `999` characters; `ui_effect`: `text_enhance_button_may_appear` |
| `company` | select | Organization | no | yes |  | `purpose`: `contact_company`; `displayed_on`: `company_contact_lists` |
| `contactrole` | integer |  | no | no |  | `purpose`: `contact_role`; `allowed_values_listed_in`: `crm_list_contact_role` |
| `contactsource` | select | Lead Source | no | no |  | `purpose`: `lead_source`; `allowed_values_listed_in`: `campaigns` (when `marketing_automation`); `cannot`: `create_lead_sources` (when `marketing_automation`); `allowed_values_listed_in`: `crm_list_lead_source` (when `no_marketing_automation`); `help_text_shared_with`: `customer.leadsource`, `lead.leadsource`, `prospect.leadsource` |
| `customform` | select | Custom Form | yes | no |  | `purpose`: `entry_form`; `special_values`: `Standard Contact Form`; `ui_effect`: `new_option_creates_record` |
| `datecreated` | datetime | Date Created | no | no |  | `purpose`: `date_created`; `value_source`: `system_recorded` |
| `defaultaddress` | address | Address (Display Type DISABLED) | no | no |  | `purpose`: `default_billing_address`; `value_source`: `address_sublist_default_billing` |
| `email` | email | Email | no | no |  | `purpose`: `email_address`; `effect`: `receives_event_invitations` |
| `entityid` | text | Contact Name | yes | yes |  | `purpose`: `contact_name`; `default_behavior`: `built_from_first_middle_last_names`; `displayed_on`: `lists` |
| `fax` | phone | Fax | no | yes |  | `purpose`: `fax_number`; `format`: `as_dialed_including_prefix`; `ui_effect`: `fills_to_be_faxed_on_transactions`; `value_examples`: `leading_1_when_required`; `requires`: `fax_service_set_up` (when `faxing_forms`) |
| `firstname` | text | Full Name | no | yes |  | `purpose`: `first_name`; `effect`: `first_part_of_contact_name`; `required_when`: `online_bill_pay_feature` |
| `globalsubscriptionstatus` | select | Global Subscription Status | no | no |  | `purpose`: `global_subscription_status`; `allowed_values`: `Confirmed Opt-In`, `Soft Opt-In`, `Soft Opt-Out`, `Confirmed Opt-Out`; `constraint`: `only_recipient_sets_confirmed_statuses`; `can`: `set_soft_statuses_manually_or_mass_update`; `caveat`: `confirmed_opt_out_gets_no_campaign_or_opt_in`; `option_meanings`: `confirmed_opt_in:recipient_wants_campaigns`, `soft_opt_in:receives_opt_in_messages`, `soft_opt_out:no_campaigns_opt_in_messages_ok`, `confirmed_opt_out:no_campaigns_or_opt_in_messages`; `caveat`: `re_opt_in_only_via_customer_center_or_old_link` |
| `homephone` | phone | Home Phone | no | yes |  | `purpose`: `home_phone`; `displayed_on`: `this_record_only` |
| `image` | select | Image | no | no |  | `purpose`: `record_image`; `value_source`: `file_cabinet_image`; `ui_effect`: `new_option_creates_record` |
| `isinactive` | checkbox | Inactive | no | yes |  | `purpose`: `inactivate_contact`; `ui_effect`: `hidden_from_contacts_list_unless_show_inactives`; `cannot`: `select_inactive_contact_on_records` |
| `isprivate` | checkbox | Private | no | no |  | `purpose`: `private_contact`; `true_means`: `visible_only_to_creator`; `effect`: `excluded_from_duplicate_detection` |
| `lastname` | text |  | no | yes |  | `purpose`: `last_name`; `effect`: `last_part_of_contact_name`; `required_when`: `online_bill_pay_feature` |
| `middlename` | text |  | no | yes |  | `purpose`: `middle_name`; `effect`: `middle_part_of_contact_name` |
| `mobilephone` | phone | Mobile Phone | no | yes |  | `purpose`: `mobile_phone`; `displayed_on`: `this_record_only` |
| `officephone` | phone | Office Phone | no | no |  | `purpose`: `office_phone`; `displayed_on`: `this_record_only` |
| `otherrelationships` | select | Other Relationships | no | no |  | `purpose`: `other_records_for_same_entity`; `value_source`: `system_displayed` |
| `phone` | phone | Cell Phone | no | yes |  | `purpose`: `primary_phone`; `displayed_on`: `contacts_list`, `company_contact_section`; `required_when`: `online_bill_pay_feature` |
| `phoneticname` | text | blank | no | no |  | `purpose`: `furigana_sort_name` |
| `salutation` | text | Mr/Mrs/Ms | no | yes |  | `purpose`: `salutation`; `value_examples`: `Mr.`, `Mrs.`, `Ms.`, `Miss` |
| `subsidiary` | select | Subsidiary | yes | no |  | `purpose`: `contact_subsidiary`; `requires`: `set_before_entering_transactions`; `effect`: `transactions_associated_with_subsidiary`; `cannot`: `change_after_transaction_posted` |
| `supervisor` | select | Supervisor | no | no |  | `purpose`: `supervisor_contact`; `allowed_values`: `existing_contacts` |
| `supervisorphone` | phone | Sup. Phone | no | no |  | `purpose`: `supervisor_phone` |
| `title` | text | Job Title | no | no |  | `purpose`: `job_title`; `displayed_on`: `company_contacts_section` |
| `unsubscribe` | select | Unsubscribe from Campaigns | no | no |  | `purpose`: `unsubscribed_from_marketing`; `true_means`: `no_campaign_email`; `value_source`: `unsubscribe_link_in_campaign_email`; `configured_by`: `unsubscribed_to_marketing_by_default` (when `us_edition`); `caveat`: `resubscribe_only_via_customer_center_or_email_link` |
| `externalid` | text | ExternalId | no | no |  |  |
| `lastmodifieddate` | datetime | Last Modified Date | no | no |  |  |
| `owner` | integer |  | no | no |  |  |

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

## Tabs

| Tab ID | Label |
|---|---|
| `address` | Address |
| `custom` | Custom |
| `general` | General |
| `info` | Info |
| `marketing` | Marketing |
| `support` | Support |
| `workflow` | Workflow |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `address` | textarea | Address |  |
| `addressee` | text | Addressee |  |
| `addresslabel` | text | Address Label |  |
| `addressphone` | phone | Address Phone |  |
| `assistant` | select | Assistant |  |
| `assistantphone` | phone | Assist. Phone |  |
| `attention` | text | Attention |  |
| `availableoffline` | checkbox | Always Available Offline |  |
| `category` | select | Category |  |
| `city` | text | City |  |
| `comments` | textarea | Comments |  |
| `company` | select | Company |  |
| `contactrole` | select | Role |  |
| `country` | select | Country |  |
| `county` | text | County/State |  |
| `datecreated` | datetime | Date Created |  |
| `email` | text | Email |  |
| `employer` | text | Employer |  |
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
| `isprivate` | checkbox | Is Private |  |
| `language` | select | Language |  |
| `lastmodifieddate` | datetime | Last Modified |  |
| `lastname` | text | Last Name |  |
| `level` | select | Level |  |
| `middlename` | text | Middle Name |  |
| `onlineformdate` | date | Online Form History - Form Date |  |
| `onlineformleadsource` | select | Online Form History - Lead Source |  |
| `onlineformname` | select | Online Form History - Form Name |  |
| `owner` | select | Owner |  |
| `permission` | select | Permission |  |
| `phone` | phone | Phone |  |
| `phoneticname` | text | Phonetic Name |  |
| `representingsubsidiary` | select | Represents Subsidiary |  |
| `salutation` | text | Mr./Mrs... |  |
| `state` | select | State/Province |  |
| `subsidiary` | select | Subsidiary |  |
| `supervisor` | select | Supervisor |  |
| `supervisorphone` | phone | Sup. Phone |  |
| `title` | text | Job Title |  |
| `type` | select | Entity Type |  |
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
| `assistant` | select | Assistant |  |
| `assistantphone` | phone | Assist. Phone |  |
| `attention` | text | Attention |  |
| `availableoffline` | checkbox | Always Available Offline |  |
| `billcountrycode` | text | Billing Country Code |  |
| `billzipcode` | text | Billing Zip |  |
| `category` | select | Category |  |
| `city` | text | City |  |
| `comments` | text | Comments |  |
| `company` | select | Company |  |
| `contactrole` | select | Role |  |
| `country` | select | Country |  |
| `countrycode` | text | Country Code |  |
| `datecreated` | datetime | Date Created |  |
| `email` | email | Email |  |
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
| `language` | select | Language |  |
| `lastmodifieddate` | datetime | Last Modified |  |
| `lastname` | text | Last Name |  |
| `lastviewed` | datetime | Last Viewed |  |
| `level` | select | Level |  |
| `middlename` | text | Middle Name |  |
| `mobilephone` | phone | Mobile Phone |  |
| `onlineformdate` | text | Online Form History - Date |  |
| `onlineformleadsource` | text | Online Form History - Lead Source |  |
| `onlineformname` | text | Online Form History - Form Name |  |
| `owner` | select | Owner |  |
| `permission` | select | Permission |  |
| `phone` | phone | Phone |  |
| `phoneticname` | text | Phonetic Name |  |
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
| `supervisor` | select | Supervisor |  |
| `supervisorphone` | phone | Sup. Phone |  |
| `title` | text | Job Title |  |
| `type` | select | Type |  |
| `zipcode` | text | Zip Code |  |

## Search joins

| Join ID | Label | Joined record |
|---|---|---|
| `campaignresponse` | Campaign Response | Campaign |
| `customer` | Customer | Customer |
| `customerprimary` | Customer (Primary) | Customer |
| `job` | Job | Job |
| `jobprimary` | Job (Primary) | Job |
| `messages` | Messages | Message |
| `messagesfrom` | Messages From | Message |
| `messagesto` | Messages To | Message |
| `parentcustomer` | Parent Customer | Customer |
| `parentjob` | Parent Job | Job |
| `parentpartner` | Parent Partner | Partner |
| `parentvendor` | Parent Vendor | Vendor |
| `partner` | Partner | Partner |
| `partnerprimary` | Partner (Primary) | Partner |
| `task` | Task | Task |
| `vendor` | Vendor | Vendor |
| `vendorprimary` | Vendor (Primary) | Vendor |

## Suspected source errors

> [!SOURCE-ERROR] `stray_html_tag_in_help` (on `altemail`)
<!-- source-error: record:contact | altemail | stray_html_tag_in_help -->
