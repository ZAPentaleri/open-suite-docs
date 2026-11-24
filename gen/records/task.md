# Task (`task`)
<!-- id: record:task -->

| Fact | Value |
|---|---|
| Internal ID | `task` |
| Category | activity |
| Client scripts | yes |
| Server scripts | yes |
| Searchable | yes |
| Tags | `custom_fields_supported`, `scripting_full` |

## Record facts

- `script_functions`: `create`, `update`, `copy`, `delete`, `search`
- `sublist_requires_feature`: `timeitem:time_tracking`
- `not_scriptable_when`: `over_9500_time_entries_static_list`
- `script_types`: `client`, `server`
- `script_functions`: `create`, `update`, `copy`, `delete`, `search`
- `doc_inconsistency`: `functions_sentence_names_resource_allocation`
- `purpose`: `activities_to_complete_for_people_or_companies`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `accesslevel` | checkbox | Private Task | no | no |  | `purpose`: `private_task`; `true_means`: `visible_to_creator_and_assignee_only` |
| `actualtime` | time | Actual Time | no | no |  | `purpose`: `actual_time`; `value_source`: `system_displayed` |
| `assigned` | select | Assigned To | yes | yes |  | `purpose`: `assignee`; `default_behavior`: `current_user` |
| `company` | select | Company | no | no |  | `purpose`: `task_company`; `effect`: `task_attached_to_company_record` |
| `contact` | select | Contact | no | no |  | `purpose`: `task_contact`; `allowed_values_listed_in`: `contacts_of_selected_company` |
| `customform` | select | Custom Form | yes | no |  | `purpose`: `task_form`; `special_values`: `Standard Task Form`; `ui_effect`: `new_option_creates_record` |
| `duedate` | date | Due Date | yes | yes |  | `purpose`: `due_date` |
| `enddate` | text |  | no | no |  | `purpose`: `task_end_date`; `displayed_on`: `assignee_email` |
| `endtime` | timeofday | End Time | yes | no |  | `purpose`: `calendar_end_time`; `requires`: `block_out_time_checked` |
| `estimatedtime` | time | Initial Time Budget | no | yes |  | `purpose`: `initial_time_budget`; `default_behavior`: `task_template_estimate` |
| `estimatedtimeoverride` | time | Current Time Budget | no | yes |  | `purpose`: `current_time_budget` |
| `message` | textarea |  | no | no |  | `purpose`: `task_comments`; `displayed_on`: `assignee_email` |
| `milestone` | select | Milestone | no | no |  | `purpose`: `milestone` |
| `owner` | text | Created By | yes | no |  | `purpose`: `task_creator`; `value_source`: `creating_user` |
| `parent` | select | Parent Task | no | no |  | `purpose`: `parent_task`; `effect`: `parent_becomes_summary_task`; `cannot`: `assign_resources_to_summary_task` |
| `percentcomplete` | percent | Percent Complete | no | no |  | `purpose`: `percent_complete`; `value_examples`: `50%`; `effect`: `overrides_percent_time_complete` |
| `percenttimecomplete` | percent | Percent Time Complete | no | no |  | `purpose`: `percent_time_complete`; `value_source`: `actual_divided_by_current_or_initial_budget` |
| `priority` | select | Priority | yes | yes |  | `purpose`: `task_priority`; `allowed_values`: `High`, `Medium`, `Low` |
| `reminderminutes` | select | Reminder | no | no |  | `purpose`: `reminder_lead_time`; `requires`: `block_out_time_checked`; `requires_together`: `reminderminutes`, `remindertype` |
| `remindertype` | select | Reminder Type | no | no |  | `purpose`: `reminder_type`; `requires`: `block_out_time_checked`; `requires_together`: `reminderminutes`, `remindertype`; `caveat`: `email_to_assignee_employee_address`; `caveat`: `popup_only_when_logged_in` |
| `sendemail` | checkbox | Notify Assignee by Email | no | no |  | `purpose`: `email_assignee`; `effect`: `assignee_emailed_on_update`; `caveat`: `no_email_without_address` |
| `startdate` | date | Start Date | yes | yes |  | `purpose`: `task_start_date`; `displayed_on`: `assignee_email` |
| `starttime` | timeofday | Start Time | yes | no |  | `purpose`: `calendar_start_time`; `requires`: `block_out_time_checked` |
| `status` | select | Status | yes | yes |  | `purpose`: `task_status`; `allowed_values`: `Complete`, `In Progress`, `Not Started`; `requires_permission`: `edit_task` |
| `supportcase` | select | Support Case | no | no |  | `purpose`: `related_support_case`; `effect`: `task_attached_to_case` |
| `timedevent` | checkbox | Reserve Time | no | no |  | `purpose`: `schedule_on_calendar`; `requires_together`: `starttime`, `endtime` |
| `timeremaining` | time | Time Remaining | no | no |  | `purpose`: `time_remaining`; `value_source`: `budget_minus_actual_time` |
| `title` | text | Title | yes | yes |  | `purpose`: `task_title`; `displayed_on`: `assignee_email_subject`, `tasks_list` |
| `transaction` | select | Transaction | no | no |  | `purpose`: `related_transaction`; `displayed_on`: `transaction_history_subtab` |
| `createddate` | datetime |  | no | no |  |  |
| `endtimepicker` | select |  | no | no |  |  |
| `externalid` | text | ExternalId | no | no |  |  |
| `group` | text |  | no | no |  |  |
| `lastmodifieddate` | datetime |  | no | no |  |  |
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
| `actualtime` | float | Actual Work |  |
| `assigned` | select | Assigned To |  |
| `company` | select | Company |  |
| `completeddate` | date | Date Completed |  |
| `contact` | select | Contact |  |
| `createddate` | datetime | Date Created |  |
| `enddate` | date | Due date |  |
| `estimatedtime` | float | Initial Time Budget |  |
| `estimatedtimeoverride` | float | Current Time Budget |  |
| `externalid` | select | External ID |  |
| `externalidstring` | text | External ID (Text) |  |
| `formuladate` | date | Formula (Date) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulatext` | text | Formula (Text) |  |
| `internalid` | select | Internal ID |  |
| `internalidnumber` | integer | Internal ID (Number) |  |
| `isjobsummarytask` | checkbox | Is Job Summary Task |  |
| `isjobtask` | checkbox | Is Job Task |  |
| `isprivate` | checkbox | Is Private |  |
| `lastmodifieddate` | datetime | Last Modified Date |  |
| `milestone` | integer | Milestone |  |
| `owner` | select | Created By |  |
| `percentcomplete` | integer | Percent Complete |  |
| `percenttimecomplete` | integer | Percent Time Complete |  |
| `priority` | select | Priority |  |
| `startdate` | date | Start date |  |
| `status` | select | Status |  |
| `timeremaining` | float | Remaining Work |  |
| `title` | text | Title |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `accesslevel` | text | Private |  |
| `actualtime` | float | Actual Work |  |
| `assigned` | select | Assigned To |  |
| `company` | select | Company |  |
| `completeddate` | date | Date Completed |  |
| `contact` | select | Contact |  |
| `createddate` | datetime | Date Created |  |
| `duedate` | date | Due Date |  |
| `estimatedtime` | float | Initial Time Budget |  |
| `estimatedtimeoverride` | float | Current Time Budget |  |
| `externalid` | select | External ID |  |
| `formulacurrency` | currency | Formula (Currency) |  |
| `formuladate` | date | Formula (Date) |  |
| `formuladatetime` | datetime | Formula (Date/Time) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulapercent` | percent | Formula (Percent) |  |
| `formulatext` | text | Formula (Text) |  |
| `internalid` | select | Internal ID |  |
| `isjobsummarytask` | checkbox | Is Job Summary Task |  |
| `isjobtask` | checkbox | Is Job Task |  |
| `lastmodifieddate` | datetime | Last Modified |  |
| `lastviewed` | datetime | Last Viewed |  |
| `markdone` | text | Mark |  |
| `message` | text | Comment |  |
| `milestone` | select | Milestone |  |
| `order` | integer | Insert |  |
| `owner` | select | Created By |  |
| `percentcomplete` | percent | Pct Complete |  |
| `percenttimecomplete` | percent | Percent Time Complete |  |
| `priority` | select | Priority |  |
| `startdate` | date | Start Date |  |
| `starttime` | timeofday | Start Time |  |
| `status` | select | Status |  |
| `timeremaining` | float | Remaining Work |  |
| `title` | text | Task Title |  |
| `transaction` | select | Transaction |  |

## Search joins

| Join ID | Label | Joined record |
|---|---|---|
| `companycustomer` | Company (Customer) | Customer |
| `contact` | Contact | Contact |
| `employee` | Employee | Employee |
| `job` | Job | Job |
| `originatinglead` | Originating Lead | Lead |

## Suspected source errors

> [!SOURCE-ERROR] `functions_sentence_names_resource_allocation`
<!-- source-error: record:task | - | functions_sentence_names_resource_allocation -->
