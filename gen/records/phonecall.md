# Phone Call (`phonecall`)
<!-- id: record:phonecall -->

| Fact | Value |
|---|---|
| Internal ID | `phonecall` |
| Category | activity |
| Client scripts | yes |
| Server scripts | yes |
| Searchable | yes |
| Tags | `custom_fields_supported`, `scripting_full` |

## Record facts

- `script_types`: `client`, `server`
- `script_functions`: `create`, `update`, `copy`, `delete`, `search`, `transform`
- `sublist_requires_feature`: `timeitem:time_tracking`
- `not_scriptable_when`: `over_9500_time_entries_static_list`
- `purpose`: `document_phone_call_activity`
- `effect`: `stored_on_caller_customer_and_listed_contacts`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `accesslevel` | checkbox | Private Phone Call | no | no |  | `purpose`: `private_call`; `true_means`: `only_in_own_phone_calls_list` |
| `assigned` | select | Organizer | yes | yes |  | `purpose`: `call_assignee` |
| `company` | select | Company | no | no |  | `purpose`: `call_company`; `effect`: `call_attached_to_company_record` |
| `completeddate` | date | Date Completed | no | no |  | `purpose`: `completed_date`; `use_when`: `follow_up_after_call_date` |
| `contact` | select | Contact | no | no |  | `purpose`: `call_contact`; `allowed_values_listed_in`: `contacts_of_selected_company` |
| `customform` | select | Custom Form | yes | no |  | `purpose`: `custom_form`; `ui_effect`: `page_switches_to_form` |
| `enddate` | text |  | no | yes |  | `purpose`: `call_deadline` |
| `endtime` | timeofday | End Time | yes | no |  | `purpose`: `calendar_end_time`; `requires`: `reserve_time_checked` |
| `message` | textarea |  | no | no |  | `purpose`: `call_notes`; `max_length`: `999` characters |
| `phone` | phone | Phone Number | no | yes |  | `purpose`: `call_phone_number`; `accepts_forms`: `nnn-nnn-nnnn`, `1-nnn-nnn-nnnn`, `(nnn) nnn-nnnn`, `1(nnn) nnn-nnnn`, `nnn-nnn-nnnn ext nnnn` |
| `reminderminutes` | select | Reminder | no | no |  | `purpose`: `reminder_lead_time`; `requires`: `block_out_time_checked`; `requires_together`: `reminderminutes`, `remindertype` |
| `remindertype` | select | Reminder Type | no | no |  | `purpose`: `reminder_type`; `requires`: `block_out_time_checked`; `requires_together`: `reminderminutes`, `remindertype`; `caveat`: `email_to_organizer_employee_address`; `caveat`: `popup_only_when_logged_in` |
| `startdate` | date | Date | yes | yes |  | `purpose`: `call_date` |
| `starttime` | timeofday | Start Time | yes | no |  | `purpose`: `calendar_start_time`; `requires`: `reserve_time_checked` |
| `status` | select | Status | yes | yes |  | `purpose`: `call_status`; `allowed_values`: `Scheduled`, `Completed`; `ui_effect`: `completed_hidden_from_list_by_default` |
| `supportcase` | select | Support Case | no | no |  | `purpose`: `related_support_case`; `effect`: `call_attached_to_case` |
| `timedevent` | checkbox | Reserve Time | no | no |  | `purpose`: `schedule_on_calendar`; `requires_together`: `starttime`, `endtime` |
| `title` | text | Subject | yes | yes |  | `purpose`: `call_subject`; `max_length`: `100` characters |
| `transaction` | select | Transaction | no | no |  | `purpose`: `related_transaction`; `displayed_on`: `transaction_history_subtab` |
| `createddate` | datetime |  | no | no |  |  |
| `endtimepicker` | select |  | no | no |  |  |
| `externalid` | text | ExternalId | no | no |  |  |
| `group` | text |  | no | no |  |  |
| `lastmodifieddate` | datetime |  | no | no |  |  |
| `owner` | text | Created By | yes | no |  |  |
| `priority` | select | Priority | yes | yes |  |  |
| `sendemail` | checkbox | Send Email | no | no |  |  |
| `starttimepicker` | select |  | no | no |  |  |
| `timezone` | text |  | no | no |  |  |

## Sublist `timeitem` (Time Tracking)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `class` | select | Class | no |  |  |
| `customer` | select | Customer:Project | no |  |  |
| `department` | select | Department | no |  |  |
| `employee` | select | Employee | yes |  |  |
| `hours` | timetrack | Duration | yes |  |  |
| `id` | integer |  | no |  |  |
| `isbillable` | checkbox | Billable | no |  |  |
| `isexempt` | text |  | no |  |  |
| `isproductive` | text |  | no |  |  |
| `isutilized` | text |  | no |  |  |
| `item` | select | Service Item | no |  |  |
| `jobbillingtype` | text |  | no |  |  |
| `location` | select | Location | yes |  |  |
| `memo` | textarea | Memo | no |  |  |
| `overriderate` | checkbox | Lock this Rate | no |  |  |
| `payrollitem` | select | Payroll Item | no |  |  |
| `price` | select | Price Level | no |  |  |
| `rate` | currency2 | Rate | no |  |  |
| `timetype` | select | Type | no |  |  |
| `trandate` | date | Date | yes |  |  |

## Tabs

| Tab ID | Label |
|---|---|
| `communication` | Communication |
| `contacts` | Contacts |
| `custom` | Custom |
| `history` | History |
| `icalendar` | Availability |
| `media` | File Cabinet |
| `relatedinfo` | Related Info |
| `time` | Time |
| `workflow` | Workflow |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `assigned` | select | Organizer |  |
| `company` | select | Company |  |
| `completeddate` | date | Date Completed |  |
| `contact` | select | Contact |  |
| `createdby` | select | Created By |  |
| `createddate` | datetime | Date Created |  |
| `externalid` | select | External ID |  |
| `externalidstring` | text | External ID (Text) |  |
| `formuladate` | date | Formula (Date) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulatext` | text | Formula (Text) |  |
| `internalid` | select | Internal ID |  |
| `internalidnumber` | integer | Internal ID (Number) |  |
| `isprivate` | checkbox | Is Private |  |
| `lastmodifieddate` | datetime | Last Modified Date |  |
| `owner` | checkbox | Owner |  |
| `phone` | phone | Phone Number |  |
| `priority` | select | Priority |  |
| `startdate` | date | Phone Call Date |  |
| `status` | select | Status |  |
| `title` | text | Subject |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `accesslevel` | text | Private |  |
| `assigned` | select | Organizer |  |
| `company` | select | Company |  |
| `completeddate` | date | Date Completed |  |
| `contact` | select | Contact |  |
| `createddate` | datetime | Date Created |  |
| `enddate` | date | End Date |  |
| `externalid` | select | External ID |  |
| `formulacurrency` | currency | Formula (Currency) |  |
| `formuladate` | date | Formula (Date) |  |
| `formuladatetime` | datetime | Formula (Date/Time) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulapercent` | percent | Formula (Percent) |  |
| `formulatext` | text | Formula (Text) |  |
| `internalid` | select | Internal ID |  |
| `lastmodifieddate` | datetime | Last Modified |  |
| `lastviewed` | datetime | Last Viewed |  |
| `markdone` | text | Mark |  |
| `message` | text | Comment |  |
| `owner` | select | Created By |  |
| `phone` | phone | Phone Number |  |
| `priority` | select | Priority |  |
| `startdate` | date | Phone Call Date |  |
| `starttime` | timeofday | Start Time |  |
| `status` | select | Status |  |
| `title` | text | Subject |  |
| `transaction` | select | Transaction |  |

## Search joins

| Join ID | Label | Joined record |
|---|---|---|
| `companycustomer` | Company (Customer) | Customer |
| `contact` | Contact | Contact |
| `employee` | Employee | Employee |
| `originatinglead` | Originating Lead | Lead |
