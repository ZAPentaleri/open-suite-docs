# Timesheet (`timesheet`)
<!-- id: record:timesheet -->

| Fact | Value |
|---|---|
| Internal ID | `timesheet` |
| Category | transaction |
| Client scripts | yes |
| Server scripts | yes |
| Searchable | yes |
| Tags | `custom_fields_supported`, `scripting_full` |

## Record facts

- `requires_feature`: `Time Tracking`, `Weekly Timesheets` [`real_name`]
- `sublist_line_record` (refs `record:timebill`)
- `record_actions`: `approve`, `reject`, `submit`
- `record_macros`: `checkTimeLimits`, `copyFromWeek`
- `purpose`: `one_week_of_time_entries_one_line_per_day`
- `difference`: `old_version_timeentry_subrecord_new_version_timebill`
- `caveat`: `records_browser_may_list_deprecated_field_ids`
- `configured_by`: `enable_features_employees_subtab` [`time_tracking`, `weekly_timesheets`, `new_weekly_timesheets_interface`]
- `requires_permission`: `administrator` (when `enabling_features`)
- `effect`: `new_interface_applies_to_all_timesheet_users` (when `new_weekly_timesheets_interface`)
- `available_only_in`: `2018_1_and_later` [`new_weekly_timesheets_interface`]
- `risk`: `existing_timesheet_scripts_may_break` (when `new_weekly_timesheets_interface`)
- `ui_effect`: `some_fields_moved_to_per_day_popup` (when `new_weekly_timesheets_interface`)
- `effect`: `popup_field_id_sets_value_for_all_days`
- `format`: `append_day_index_0_to_6_for_single_day` [`popup_fields`]
- `value_examples`: `memo0:memo_for_first_day`
- `risk`: `multi_form_client_scripts_fail_if_field_locations_differ`
- `recommendation`: `user_event_scripts_over_client_scripts_for_single_timebills`
- `effect`: `approve_reject_submit_all_arrive_as_edit_event` (when `new_weekly_timesheets_interface`) (refs `script:UserEventScript`)
- `workaround`: `check_approval_status_change_instead_of_event_type`
- `difference`: `time_limits_checked_only_on_ui_submit_without_new_interface`
- `effect`: `time_limits_validated_on_scripted_time_entry_add` (when `new_weekly_timesheets_interface`)
- `supported_script_types`: `client`, `server`
- `record_actions`: `approve:all_pending`, `reject:all_pending_optional_note`, `retract`, `submit`
- `governance_note`: `approve:5`, `reject:5`
- `requires_preference`: `advanced_approvals_on_time` [`reject_action`]
- `effect`: `retract_moves_pending_timebills_to_open`
- `contains`: `id`, `action`, `success`, `recordCount`, `platformResponses` [`retract_returns`]
- `governance_note`: `5` usage_units [`submit_action`]
- `requires_preference`: `advanced_approvals_on_time` [`submit_action`]
- `ui_location`: `approve_reject_all_pending_retract_and_submit_buttons`
- `caveat`: `acts_only_on_timebills_user_may_approve_or_reject`
- `record_macros`: `checkTimeLimits:server_limit_check`, `copyFromWeek:copies_lines_no_replace`
- `macro_params`: `checkTimeLimits.action:approve_save_submit`, `checkTimeLimits.limitcheck`
- `ui_location`: `check_time_limits_and_copy_from_week_buttons`
- `caveat`: `approve_limits_may_be_skipped_by_preferences`
- `caveat`: `save_not_checked_when_submit_available`
- `contains`: `weekOf`, `copyExact` [`copyfromweek_params`]
- `caveat`: `copyfromweek_keeps_lines_hours_memos_may_not_copy`
- `effect`: `checktimelimits_sends_timeitem_machine_to_service`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `approvalstatus` | select |  | no | no |  | `purpose`: `approval_status` |
| `customform` | select | Custom Form | yes | no |  | `purpose`: `custom_form` |
| `employee` | select | Employee | no | no |  | `purpose`: `employee`; `ui_effect`: `new_option_creates_record` |
| `startdate` | date | Start Date | yes | no |  | `purpose`: `timesheet_start_date`; `value_source`: `system_displayed` |
| `subsidiary` | select | Subsidiary | no | no |  | `purpose`: `subsidiary`; `value_source`: `employee_subsidiary` |
| `totalhours` | time | Total Hours | no | no |  | `purpose`: `total_hours`; `value_source`: `updated_in_real_time` |
| `trandate` | date | Week Of | yes | no |  | `purpose`: `transaction_date`; `default_behavior`: `current_date` |
| `allocatedhours` | time | Allocated Hours | no | no |  |  |
| `enddate` | date | End Date | yes | no |  |  |
| `externalid` | text | ExternalId | no | no |  |  |
| `id` | integer | Internal ID | no |  |  |  |
| `iscomplete` | checkbox |  | no | no |  |  |
| `plannedhours` | time | Planned Hours | no | no |  |  |
| `rejectedhours` | time | Rejected Hours | no | no |  |  |
| `submittedhours` | time | Submitted Hours | no | no |  |  |
| `templatestored` | text |  | no | no |  |  |
| `workcalendarhours` | time | Work Calendar Hours | no | no |  |  |

## Sublist `timegrid` (Time)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `friday` | summary |  | no |  |  |
| `monday` | summary |  | no |  |  |
| `saturday` | summary |  | no |  |  |
| `sunday` | summary |  | no |  |  |
| `thursday` | summary |  | no |  |  |
| `tuesday` | summary |  | no |  |  |
| `wednesday` | summary |  | no |  |  |

## Sublist `timeitem` (Enter Time)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `billingclass` | select | Billing Class | no |  |  |
| `billingsubsidiary` | select | Billing Subsidiary | no |  |  |
| `casetaskevent` | select | Case/Task/Event | no |  |  |
| `class` | select | Class | no |  |  |
| `customer` | select | Customer : Job | no |  |  |
| `department` | select | Department | no |  |  |
| `hours0` | time | Sun | no |  |  |
| `hours1` | time | Mon | no |  |  |
| `hours2` | time | Tue | no |  |  |
| `hours3` | time | Wed | no |  |  |
| `hours4` | time | Thu | no |  |  |
| `hours5` | time | Fri | no |  |  |
| `hours6` | time | Sat | no |  |  |
| `isbillable` | checkbox | Billable | no |  |  |
| `isexempt` | checkbox | Exempt | no |  |  |
| `isproductive` | checkbox | Productive | no |  |  |
| `isutilized` | checkbox | Utilized | no |  |  |
| `item` | select | Service Item | no |  |  |
| `location` | select | Location | no |  |  |
| `memo` | textarea | Memo | no |  |  |
| `overriderate` | checkbox | Lock this Rate | no |  |  |
| `payrollitem` | select | Payroll Item | no |  |  |
| `payrollworkplace` | select | Payroll Item Workplace | no |  |  |
| `price` | select | Price Level | no |  |  |
| `rate` | currency2 | Rate | no |  |  |
| `rejectionnote` | textarea | Rejection Note | no |  |  |
| `statushours0` | integer |  | no |  |  |
| `statushours1` | integer |  | no |  |  |
| `statushours2` | integer |  | no |  |  |
| `statushours3` | integer |  | no |  |  |
| `statushours4` | integer |  | no |  |  |
| `statushours5` | integer |  | no |  |  |
| `statushours6` | integer |  | no |  |  |
| `timebill0` | integer |  | no |  |  |
| `timebill1` | integer |  | no |  |  |
| `timebill2` | integer |  | no |  |  |
| `timebill3` | integer |  | no |  |  |
| `timebill4` | integer |  | no |  |  |
| `timebill5` | integer |  | no |  |  |
| `timebill6` | integer |  | no |  |  |
| `timemodified` | checkbox | Time Modified | no |  |  |
| `timeofftype` | select | Time-Off Type | no |  |  |
| `timetype` | select | Type | no |  |  |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `allocatedhours` | time | Allocated Hours |  |
| `allocatedhoursdecimal` | float | Allocated Hours (DECIMAL) |  |
| `approvalstatus` | select | Approval Status (Text) |  |
| `employee` | select | Employee |  |
| `externalid` | select | External ID |  |
| `externalidstring` | text | External ID (Text) |  |
| `formuladate` | date | Formula (Date) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulatext` | text | Formula (Text) |  |
| `id` | integer | ID |  |
| `internalid` | select | Internal ID |  |
| `internalidnumber` | integer | Internal ID (Number) |  |
| `plannedhours` | time | Planned Hours |  |
| `plannedhoursdecimal` | float | Planned Hours (DECIMAL) |  |
| `rejectedhours` | time | Rejected Hours |  |
| `rejectedhoursdecimal` | float | Rejected Hours (DECIMAL) |  |
| `submittedhours` | time | Submitted Hours |  |
| `submittedhoursdecimal` | float | Submitted Hours (DECIMAL) |  |
| `timesheetdate` | date | Timesheet Date |  |
| `totalhours` | float | Total Hours |  |
| `workcalendarhours` | time | Work Calendar Hours |  |
| `workcalendarhoursdecimal` | float | Work Calendar Hours (DECIMAL) |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `allocatedhours` | time | Allocated Hours |  |
| `allocatedhoursdecimal` | float | Allocated Hours (DECIMAL) |  |
| `approvalstatus` | select | Approval Status (Text) |  |
| `employee` | select | Employee |  |
| `enddate` | date | End Date |  |
| `externalid` | select | External ID |  |
| `formulacurrency` | currency | Formula (Currency) |  |
| `formuladate` | date | Formula (Date) |  |
| `formuladatetime` | datetime | Formula (Date/Time) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulapercent` | percent | Formula (Percent) |  |
| `formulatext` | text | Formula (Text) |  |
| `id` | integer | ID |  |
| `internalid` | select | Internal ID |  |
| `plannedhours` | time | Planned Hours |  |
| `plannedhoursdecimal` | float | Planned Hours (DECIMAL) |  |
| `rejectedhours` | time | Rejected Hours |  |
| `rejectedhoursdecimal` | float | Rejected Hours (DECIMAL) |  |
| `startdate` | date | Start Date |  |
| `submittedhours` | time | Submitted Hours |  |
| `submittedhoursdecimal` | float | Submitted Hours (DECIMAL) |  |
| `totalhours` | text | Total Hours |  |
| `totalhoursdecimal` | float | Total Hours (DECIMAL) |  |
| `workcalendarhours` | time | Work Calendar Hours |  |
| `workcalendarhoursdecimal` | float | Work Calendar Hours (DECIMAL) |  |

## Search joins

| Join ID | Label | Joined record |
|---|---|---|
| `employee` | Employee | Employee |
| `timebill` | Time | Time |
| `timeentry` | Time Entry | Time Entry |
