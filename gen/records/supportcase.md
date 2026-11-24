# Case (`supportcase`)
<!-- id: record:supportcase -->

| Fact | Value |
|---|---|
| Internal ID | `supportcase` |
| Category | support |
| Client scripts | yes |
| Server scripts | yes |
| Searchable | yes |
| Tags | `custom_fields_supported`, `scripting_full` |

## Record facts

- `script_types`: `client`, `server`
- `script_functions`: `create`, `update`, `copy`, `delete`, `search`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `assigned` | select | Assigned To | no | yes |  | `purpose`: `case_assignee`; `allowed_values_listed_in`: `employees_marked_support_rep` |
| `autoname` | checkbox | Auto | no | no |  | `purpose`: `auto_case_number`; `shown_only_when`: `override_auto_numbers_allowed`; `false_means`: `number_editable` |
| `casenumber` | identifieranycase | Number | no | no |  | `purpose`: `case_number`; `value_source`: `auto_generated` (when `auto_generated_numbering`); `supported_in`: `case_search`, `global_search` |
| `category` | select | Type | no | yes |  | `purpose`: `case_type`; `allowed_values`: `problem`, `question`, `concern` [`defaults`]; `allowed_values_listed_in`: `case_type_records` |
| `company` | select | Company | yes | no |  | `purpose`: `case_company`; `allowed_values`: `customers`, `leads`, `prospects`, `employees`, `partners` |
| `contact` | select | Contact | no | no |  | `purpose`: `reporting_contact`; `allowed_values_listed_in`: `contacts_of_selected_company` |
| `createddate` | datetime | Created Date | no | no |  | `purpose`: `date_time_created`; `value_source`: `system_displayed` |
| `customform` | select | Custom Form | yes | no |  | `purpose`: `case_form`; `ui_effect`: `page_switches_to_form`; `caveat`: `customizing_saves_separate_form` |
| `email` | emails | Email(s) | no | yes |  | `purpose`: `contact_email`; `format`: `comma_or_semicolon_separated`; `effect`: `edit_expires_external_case_response_link` (when `external_case_response_page_feature`) |
| `emailemployees` | select | Copy Employees | no | no |  | `purpose`: `email_employees`; `value_kind`: `multi_select`; `effect`: `notified_with_case_details_on_save` |
| `emailform` | checkbox | Send to Customer | no | no |  | `purpose`: `email_reply_to_customer` |
| `enddate` | datetime | Date Closed | no | no |  | `purpose`: `date_time_closed` |
| `escalationmessage` | textarea | Escalation Message | no | no |  | `purpose`: `escalation_message`; `caveat`: `not_sent_to_customer_not_stored`; `ui_effect`: `text_enhance_button_may_appear` |
| `externalid` | text | ExternalId | no | no |  | `purpose`: `external_id` |
| `helpdesk` | checkbox | Help Desk | no | yes |  | `purpose`: `help_desk_case`; `caveat`: `excluded_from_customer_service_reports` |
| `inboundemail` | email | Inbound Email Address | no | no |  | `purpose`: `inbound_support_address`; `value_source`: `system_displayed`; `applies_when`: `submitted_by_email` |
| `incomingmessage` | textarea | Message | no | no |  | `purpose`: `latest_customer_message`; `caveat`: `kept_when_company_changed`; `ui_effect`: `text_enhance_button_may_appear` |
| `insertsolution` | select | Insert Solution | no | no |  | `purpose`: `insert_solution`; `options`: `type_and_tab`, `List`, `Search`, `New` |
| `internalonly` | checkbox | Internal Only | no | no |  | `purpose`: `internal_only_reply`; `true_means`: `hidden_from_customers` |
| `isinactive` | checkbox | Inactive | no | yes |  | `purpose`: `inactivate_case`; `ui_effect`: `hidden_from_cases_list_unless_show_inactives` |
| `issue` | select | Case Issue | no | yes |  | `purpose`: `case_issue`; `allowed_values_listed_in`: `case_issue_records` |
| `item` | select | Item | no | no |  | `purpose`: `case_item`; `allowed_values_listed_in`: `items_offering_support` |
| `lastmessagedate` | datetime | Last Message Date | no | no |  | `purpose`: `last_message_date`; `value_source`: `system_displayed` |
| `lastmodifieddate` | datetime | Last Modified Date | no | no |  | `purpose`: `last_modified_date`; `effect`: `updated_on_message_received_or_sent` |
| `lastreopeneddate` | datetime | Date Last Reopened | no | no |  | `purpose`: `last_reopened_date`; `value_source`: `system_displayed` |
| `messagenew` | checkbox | New Message | no | no |  | `purpose`: `add_new_customer_message`; `ui_effect`: `clears_message_field` |
| `module` | select | Module | no | no |  | `purpose`: `product_module` |
| `origin` | select | Origin | no | yes |  | `purpose`: `case_origin`; `allowed_values_listed_in`: `case_origin_records` |
| `outgoingmessage` | textarea | Reply | no | no |  | `purpose`: `reply`; `effect`: `emailed_when_send_email_checked`; `effect`: `internal_only_goes_to_selected_employees`; `ui_effect`: `text_enhance_button_may_appear` |
| `phone` | phone | Phone | no | no |  | `purpose`: `contact_phone`; `help_text_shared_with`: `supportcase.escalateto.phone` |
| `priority` | select | Priority | no | yes |  | `purpose`: `case_priority` |
| `product` | select | Product | no | no |  | `purpose`: `product` |
| `profile` | select | Profile | yes | no |  | `purpose`: `case_profile`; `effect`: `sets_notification_from_and_templates`; `allowed_values_listed_in`: `case_profile_records` |
| `serialnumber` | select | Serial/Lot Number | no | no |  | `purpose`: `item_serial_number` |
| `startdate` | date | Incident Date | yes | no |  | `purpose`: `report_date`; `displayed_on`: `cases_list`, `cases_subtab` |
| `starttime` | timeofday | Incident Time | yes | no |  | `purpose`: `report_time`; `displayed_on`: `cases_list`, `cases_subtab` |
| `status` | select | Status | yes | yes |  | `purpose`: `case_status`; `allowed_values_listed_in`: `case_status_records`; `recommendation`: `set_assigned_when_in_progress`; `recommendation`: `set_escalate_to_when_escalated` |
| `subsidiary` | select | Subsidiary | no | no |  | `purpose`: `subsidiary`; `value_source`: `customer_subsidiary` |
| `title` | text | Subject | yes | yes |  | `purpose`: `case_subject`; `max_length`: `256` characters; `displayed_on`: `cases_list`, `cases_subtab` |
| `companyname` | text |  | no | no |  |  |
| `eventnumber` | text |  | no | no |  |  |
| `newsolutionfrommsg` | textarea |  | no | no |  |  |
| `searchsolution` | textarea |  | no | no |  |  |
| `stage` | text |  | no | no |  |  |

## Sublist `escalateto` (Escalate To)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `email` | email | Email | no |  | `purpose`: `escalation_email`; `format`: `comma_or_semicolon_separated` |
| `phone` | phone | Phone | no |  | `purpose`: `contact_phone`; `help_text_shared_with`: `supportcase.phone` |
| `escalatee` | select | Escalatee | yes |  |  |

## Sublist `timeitem` (Time Tracking)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `class` | select | Class | no |  |  |
| `department` | select | Department | no |  |  |
| `employee` | select | Employee | yes |  |  |
| `hours` | timetrack | Duration | yes |  |  |
| `id` | integer |  | no |  |  |
| `isbillable` | checkbox | Billable | no |  |  |
| `isexempt` | text |  | no |  |  |
| `isproductive` | text |  | no |  |  |
| `isutilized` | text |  | no |  |  |
| `item` | select | Service Item | yes |  |  |
| `jobbillingtype` | text |  | no |  |  |
| `location` | select | Location | yes |  |  |
| `memo` | textarea | Memo | no |  |  |
| `payrollitem` | select | Payroll Item | no |  |  |
| `timetype` | select | Type | no |  |  |
| `trandate` | date | Date | yes |  |  |

## Tabs

| Tab ID | Label |
|---|---|
| `communication` | Communication |
| `custom` | Custom |
| `escalation` | Escalations |
| `general` | General |
| `interactions` | Interactions |
| `time` | Time |
| `workflow` | Workflow |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `assigned` | select | Assigned To |  |
| `awaitingreply` | checkbox | Awaiting Support Reply |  |
| `casenumber` | text | Number/ID |  |
| `category` | select | Type |  |
| `closeddate` | date | Date Closed |  |
| `company` | text | Company |  |
| `contact` | text | Contact |  |
| `createddate` | datetime | Date Created |  |
| `email` | text | Email |  |
| `escalateto` | select | Escalated To |  |
| `externalid` | select | External ID |  |
| `externalidstring` | text | External ID (Text) |  |
| `formuladate` | date | Formula (Date) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulatext` | text | Formula (Text) |  |
| `helpdesk` | checkbox | Help Desk |  |
| `inboundemail` | text | Inbound Email |  |
| `internalid` | select | Internal ID |  |
| `internalidnumber` | integer | Internal ID (Number) |  |
| `isinactive` | checkbox | Inactive |  |
| `issue` | select | Issue |  |
| `issuefixedin` | select | Issue Fixed In |  |
| `issuenumber` | text | Issue Number |  |
| `issuestatus` | select | Issue Status |  |
| `issuetype` | select | Issue Type |  |
| `item` | select | Item |  |
| `lastmessage` | checkbox | Most Recent Message |  |
| `lastmodifieddate` | datetime | Date Last Modified |  |
| `lastreopeneddate` | datetime | Date Last Reopened |  |
| `locked` | checkbox | Locked |  |
| `message` | clobtext | Message Text |  |
| `messageauthor` | select | Message Author |  |
| `messagedate` | date | Message Date |  |
| `messagetype` | checkbox | Message Type |  |
| `module` | select | Module |  |
| `number` | integer | Number |  |
| `origin` | select | Origin |  |
| `priority` | select | Priority |  |
| `product` | select | Product |  |
| `profile` | select | Profile |  |
| `serialnumber` | text | Serial/Lot # |  |
| `stage` | select | Stage |  |
| `startdate` | datetime | Incident Date |  |
| `status` | select | Status |  |
| `subsidiary` | select | Subsidiary |  |
| `title` | text | Subject |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `assigned` | select | Assigned To |  |
| `awaitingreply` | checkbox | Awaiting Support Reply |  |
| `casenumber` | text | Number |  |
| `category` | select | Type |  |
| `company` | select | Company |  |
| `contact` | select | Contact |  |
| `createddate` | datetime | Date Created |  |
| `customerstage` | select | Customer Stage |  |
| `email` | email | Email |  |
| `enddate` | datetime | Date Closed |  |
| `escalatee` | select | Escalated To |  |
| `externalid` | select | External ID |  |
| `formulacurrency` | currency | Formula (Currency) |  |
| `formuladate` | date | Formula (Date) |  |
| `formuladatetime` | datetime | Formula (Date/Time) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulapercent` | percent | Formula (Percent) |  |
| `formulatext` | text | Formula (Text) |  |
| `grabref` | text | Grab |  |
| `helpdesk` | checkbox | Help Desk |  |
| `inboundemail` | text | Inbound Email |  |
| `internalid` | select | Internal ID |  |
| `isinactive` | checkbox | Inactive |  |
| `issue` | select | Issue |  |
| `issuenumber` | text | Issue Number |  |
| `item` | select | Item |  |
| `lastmessagedate` | datetime | Last Msg. Date |  |
| `lastmodifieddate` | datetime | Last Modified |  |
| `lastreopeneddate` | datetime | Last Reopened |  |
| `lastviewed` | datetime | Last Viewed |  |
| `locked` | checkbox | Locked |  |
| `module` | select | Module |  |
| `origin` | select | Origin |  |
| `priority` | select | Priority |  |
| `product` | select | Product |  |
| `profile` | select | Profile |  |
| `serialnumber` | text | Serial/Lot # |  |
| `stage` | select | Stage |  |
| `startdate` | datetime | Incident Date |  |
| `status` | select | Status |  |
| `subsidiary` | select | Subsidiary |  |
| `title` | text | Subject |  |

## Search joins

| Join ID | Label | Joined record |
|---|---|---|
| `contact` | Contact | Contact |
| `customer` | Customer | Customer |
| `employee` | Employee | Employee |
| `issue` | Issue | Issue |
| `messages` | Messages | Message |
| `transaction` | Transaction | Transaction |
