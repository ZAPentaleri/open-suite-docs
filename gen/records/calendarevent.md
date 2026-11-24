# Event (`calendarevent`)
<!-- id: record:calendarevent -->

| Fact | Value |
|---|---|
| Internal ID | `calendarevent` |
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
- `not_supported`: `setting_recurring_events`
- `purpose`: `scheduled_activities_added_to_calendar`
- `value_kind`: `timeitem_sublist_is_inline_editor`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `accesslevel` | select | Event Access | yes | no |  | `purpose`: `event_access_level`; `options`: `Public`, `Private`, `Show As Busy`; `option_meanings`: `public:visible_to_all_with_calendar_access`, `private:visible_only_to_owner`, `show_as_busy:busy_slot_to_others` |
| `alldayevent` | checkbox | All Day | no | no |  | `purpose`: `all_day_event`; `ui_effect`: `shown_8am_to_6pm` |
| `company` | select | Company | no | no |  | `purpose`: `related_company`; `effect`: `event_attached_to_company_record` |
| `contact` | select | Contact | no | no |  | `purpose`: `contact_of_company` |
| `customform` | select | Custom Form | yes | no |  | `purpose`: `event_form`; `special_values`: `Standard Event Form`; `ui_effect`: `new_option_creates_record` |
| `endbydate` | date | End By | no | no |  | `purpose`: `recurrence_end_date`; `ui_effect`: `stops_showing_after_date` |
| `endtime` | timeofday | End Time | yes | no |  | `purpose`: `event_end_time`; `displayed_on`: `calendar_daily_and_weekly_views` (when `public_event`) |
| `location` | text | Location | no | yes |  | `purpose`: `event_location`; `help_text_shared_with`: `calendarevent.resource.location` |
| `message` | textarea |  | no | no |  | `purpose`: `event_message`; `displayed_on`: `invitation_email` |
| `noenddate` | checkbox | No End Date | no | no |  | `purpose`: `no_end_date`; `true_means`: `event_recurs_indefinitely`; `can`: `set_endbydate_later` |
| `organizer` | select | Organizer | yes | no |  | `purpose`: `event_organizer`; `effect`: `organizer_added_as_accepted_attendee`; `caveat`: `new_organizer_not_added_on_change` |
| `reminderminutes` | select | Reminder | no | no |  | `purpose`: `reminder_lead_time`; `requires`: `remindertype` |
| `remindertype` | select | Reminder Type | no | no |  | `purpose`: `reminder_type`; `options`: `Email`, `Popup Window`, `None` |
| `seriesstartdate` | date | Series Start Date | no | no |  | `purpose`: `recurrence_start_date` |
| `startdate` | date | Date | yes | yes |  | `purpose`: `event_date`; `displayed_on`: `calendar_views`, `invitation_email` |
| `starttime` | timeofday | Start Time | yes | no |  | `purpose`: `event_start_time`; `displayed_on`: `calendar_daily_and_weekly_views` (when `public_event`) |
| `status` | select | Status | yes | yes |  | `purpose`: `event_status`; `allowed_values`: `Confirmed`, `Tentative`, `Canceled`; `option_meanings`: `confirmed:will_occur`, `tentative:may_occur`, `canceled:will_not_occur`; `constraint`: `organizer_changes_status` |
| `supportcase` | select | Support Case | no | no |  | `purpose`: `related_support_case`; `effect`: `event_attached_to_case` |
| `title` | text | Title | yes | yes |  | `purpose`: `event_title`; `displayed_on`: `invitation_email_subject`, `calendars` |
| `transaction` | select | Transaction | no | no |  | `purpose`: `related_transaction`; `displayed_on`: `transaction_events_subtab` |
| `createddate` | datetime |  | no | no |  |  |
| `enddate` | text |  | no | no |  |  |
| `externalid` | text | ExternalId | no | no |  |  |
| `frequency` | text |  | no | no |  |  |
| `group` | text |  | no | no |  |  |
| `lastmodifieddate` | datetime |  | no | no |  |  |
| `owner` | text |  | no | no |  |  |
| `period` | text |  | no | no |  |  |
| `timezone` | text |  | no | no |  |  |

## Sublist `resource` (Resources)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `location` | text | Location | no |  | `purpose`: `event_location`; `help_text_shared_with`: `calendarevent.location` |
| `availability` | text | Availability | no |  |  |
| `id` | text |  | no |  |  |
| `resource` | select | Resource | yes |  |  |

## Sublist `attendee` (Attendees)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `attendance` | select | Attendance | no |  |  |
| `attendee` | select | Send Invitation to | yes |  |  |
| `attendeetype` | text |  | no |  |  |
| `availability` | text | Availability | no |  |  |
| `id` | text |  | no |  |  |
| `response` | select | Response | yes |  |  |

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
| `custom` | Custom |
| `history` | History |
| `icalendar` | Availability |
| `istatus` | Attendees |
| `media` | File Cabinet |
| `recurrence` | Recurrence |
| `relatedinfo` | Related Info |
| `rstatus` | Res. Availability |
| `time` | Time |
| `workflow` | Workflow |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `accesslevel` | select | Event Access |  |
| `attendee` | select | Attendee |  |
| `calendar` | select | Calendar |  |
| `completeddate` | date | Date Completed |  |
| `createddate` | datetime | Date Created |  |
| `externalid` | select | External ID |  |
| `externalidstring` | text | External ID (Text) |  |
| `formuladate` | date | Formula (Date) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulatext` | text | Formula (Text) |  |
| `instancestart` | datetime | Instance Start |  |
| `internalid` | select | Internal ID |  |
| `internalidnumber` | integer | Internal ID (Number) |  |
| `isupcomingevent` | checkbox | When |  |
| `lastmodifieddate` | datetime | Date Last Modified |  |
| `location` | text | Location |  |
| `message` | textarea | Message |  |
| `organizer` | select | Organizer |  |
| `resource` | select | Resource |  |
| `response` | select | Response |  |
| `startdate` | date | Date |  |
| `status` | select | Status |  |
| `title` | text | Title |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `accesslevel` | text | Event Access |  |
| `attendee` | select | Attendee |  |
| `company` | select | Company |  |
| `completeddate` | date | Date Completed |  |
| `contact` | select | Contact |  |
| `createddate` | datetime | Date Created |  |
| `endtime` | timeofday | End Time |  |
| `externalid` | select | External ID |  |
| `formulacurrency` | currency | Formula (Currency) |  |
| `formuladate` | date | Formula (Date) |  |
| `formuladatetime` | datetime | Formula (Date/Time) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulapercent` | percent | Formula (Percent) |  |
| `formulatext` | text | Formula (Text) |  |
| `instanceend` | datetime | Instance End |  |
| `instancestart` | datetime | Instance Start |  |
| `internalid` | select | Internal ID |  |
| `lastmodifieddate` | datetime | Last Modified |  |
| `lastviewed` | datetime | Last Viewed |  |
| `location` | text | Location |  |
| `markdone` | text | Mark |  |
| `message` | text | Message |  |
| `organizer` | select | Organizer |  |
| `owner` | select | Owner |  |
| `recurrence` | text | Event Date |  |
| `resource` | select | Resource |  |
| `response` | select | Response |  |
| `startdate` | date | Start Date |  |
| `starttime` | timeofday | Start Time |  |
| `status` | select | Status |  |
| `title` | text | Event |  |
| `transaction` | select | Transaction |  |

## Search joins

| Join ID | Label | Joined record |
|---|---|---|
| `attendeecontact` | Attendee (Contact) | Contact |
| `attendeecustomer` | Attendee (Customer) | Customer |
| `originatinglead` | Originating Lead | Lead |
| `time` | Time | Time |
