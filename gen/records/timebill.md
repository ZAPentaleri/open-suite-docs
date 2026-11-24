# Time (`timebill`)
<!-- id: record:timebill -->

| Fact | Value |
|---|---|
| Internal ID | `timebill` |
| Category | transaction |
| Client scripts | yes |
| Server scripts | yes |
| Searchable | yes |
| Tags | `custom_fields_supported`, `scripting_full` |

## Record facts

- `record_actions`: `approve`, `reject`, `submit`
- `governance_note`: `approve:5`, `reject:5`
- `action_params`: `reject:note_optional`
- `requires_preference`: `advanced_approvals_on_time` [`reject_and_submit_actions`]
- `governance_note`: `5` usage_units [`submit_action`]
- `ui_location`: `approve_reject_reject_with_note_submit_buttons`
- `doc_inconsistency`: `submit_parameters_text_says_approve`
- `script_types`: `client`, `server`
- `script_functions`: `create`, `update`, `copy`, `delete`, `search`
- `requires_feature`: `Time Tracking` [`real_name`]
- `record_actions`: `retract`
- `returns_detail`: `retract:id_action_success_record_count`
- `purpose`: `hours_worked_for_billing_and_invoicing`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `approvalstatus` | select |  | no | no |  | `purpose`: `approval_status` |
| `billingclass` | text | Billing Class | no | no | marked_new | `purpose`: `billing_class`; `default_behavior`: `employee_billing_class` |
| `casetaskevent` | select | Case/Task/Event | no | no |  | `purpose`: `related_activity`; `effect`: `time_added_to_selected_activity` |
| `class` | select | Class | no | yes |  | `purpose`: `class`; `ui_effect`: `new_option_creates_record` |
| `customer` | select | Customer:Project | no | no |  | `purpose`: `customer_or_project`; `requires`: `service_item` (when `customer_selected`); `ui_effect`: `new_option_creates_record` |
| `customform` | select | Custom Form | no | no |  | `purpose`: `custom_form` |
| `department` | select | Department | no | yes |  | `purpose`: `department`; `ui_effect`: `new_option_creates_record` |
| `employee` | select | Employee | yes | no |  | `purpose`: `employee`; `ui_effect`: `new_option_creates_record` |
| `externalid` | text | ExternalId | no | no |  | `purpose`: `external_id` |
| `hours` | timetrack | Duration | yes | no |  | `purpose`: `duration`; `value_source`: `start_end_and_break_times`; `ui_effect`: `timer_link_times_work` |
| `isbillable` | checkbox | Billable | no | no |  | `purpose`: `billable` |
| `isexempt` | checkbox | Exempt | no | yes |  | `purpose`: `exempt_time`; `value_source`: `system_displayed`; `doc_inconsistency`: `help_says_tax_exempt` |
| `isproductive` | checkbox | Productive | no | yes |  | `purpose`: `productive_time`; `value_source`: `system_displayed`; `caveat`: `not_billed_not_in_revenue` |
| `isutilized` | checkbox | Utilized | no | no |  | `purpose`: `utilized_time`; `value_source`: `system_displayed` |
| `item` | select | Service Item | no | no |  | `purpose`: `service_item`; `requires`: `customer_or_project` (when `item_selected`); `ui_effect`: `new_option_creates_record` |
| `location` | select | Location | yes | yes |  | `purpose`: `location`; `ui_effect`: `new_option_creates_record` |
| `memo` | textarea | Memo | no | yes |  | `purpose`: `memo`; `displayed_on`: `invoice_line_description` (when `billed_to_customer`) |
| `overriderate` | text | Lock this Rate | no | no |  | `purpose`: `lock_rate`; `effect`: `rate_unaffected_by_later_rate_changes`; `caveat`: `charge_rules_discard_entered_rates`; `recommendation`: `leave_blank_for_payroll_items` |
| `paidexternally` | checkbox | Paid Externally | no | no |  | `purpose`: `paid_externally`; `effect`: `removed_from_payroll_but_billable` |
| `payrollitem` | select | Payroll Item | no | no |  | `purpose`: `payroll_item`; `effect`: `selects_pay_rate` |
| `price` | text | Price Level | no | no |  | `purpose`: `price_level`; `default_behavior`: `customer_custom_price_level_or_sales_price_1`; `special_values`: `Custom`; `caveat`: `charge_rules_override_on_charge_based_projects`; `recommendation`: `leave_blank_for_payroll_items` |
| `rate` | text | Rate | no | no |  | `purpose`: `rate`; `value_source`: `price_level`; `caveat`: `charge_rules_override_on_charge_based_projects`; `recommendation`: `leave_blank_for_payroll_items` |
| `rejectionnote` | textarea | Rejection Note | no | yes |  | `purpose`: `rejection_note`; `applies_when`: `rejecting_time` |
| `subsidiary` | select | Subsidiary | no | no |  | `purpose`: `subsidiary`; `value_source`: `employee_subsidiary` |
| `supervisorapproval` | checkbox | Supervisor Approval | no | no |  | `purpose`: `supervisor_approval` |
| `timemodified` | checkbox | Time Modified | no |  |  | `purpose`: `time_modified`; `value_source`: `system_displayed` |
| `timesheet` | select | Timesheet | no | no |  | `purpose`: `timesheet`; `value_source`: `system_displayed` |
| `timetype` | select | Type | no | no |  | `purpose`: `time_type`; `allowed_values`: `Actual`, `Planned`, `Allocated`; `option_meanings`: `actual:worked`, `planned:project_planning`, `allocated:resource_allocation` |
| `trandate` | date | Date | yes | no |  | `purpose`: `transaction_date`; `default_behavior`: `current_date` |
| `projecttaskassignment` | select |  | no | no |  |  |
| `serviceitem` | select | Service Item | no | no |  |  |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `approvalstatus` | select | Approval Status |  |
| `approved` | checkbox | Approved |  |
| `billable` | checkbox | Billable |  |
| `billingclass` | select | Billing Class | marked_new |
| `class` | select | Class |  |
| `customer` | select | Customer:Job |  |
| `date` | date | Date |  |
| `datecreated` | datetime | Date Created |  |
| `department` | select | Department |  |
| `duration` | float | Duration |  |
| `employee` | select | Employee |  |
| `exempt` | checkbox | Exempt |  |
| `externalid` | select | External ID |  |
| `externalidstring` | text | External ID (Text) |  |
| `formuladate` | date | Formula (Date) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulatext` | text | Formula (Text) |  |
| `internalid` | select | Internal ID |  |
| `internalidnumber` | integer | Internal ID (Number) |  |
| `item` | select | Item |  |
| `lastmodified` | datetime | Last Modified |  |
| `location` | select | Location |  |
| `memo` | text | Memo |  |
| `paidbypayroll` | checkbox | Paid by Payroll |  |
| `paidexternally` | checkbox | Paid Externally |  |
| `payitem` | select | Payroll Item |  |
| `productive` | checkbox | Productive |  |
| `rejectionnote` | text | Rejection Note |  |
| `status` | checkbox | Status |  |
| `subsidiary` | select | Subsidiary |  |
| `temporarylocaljurisdiction` | select | Temporary Local Jurisdiction |  |
| `temporarystatejurisdiction` | select | Temporary State Jurisdiction |  |
| `timemodified` | checkbox | Time Modified |  |
| `timesheet` | select | Weekly Timesheet |  |
| `type` | select | Type |  |
| `utilized` | checkbox | Utilized |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `approvalstatus` | select | Approval Status |  |
| `billingclass` | select | Billing Class | marked_new |
| `break` | text | Break |  |
| `class` | select | Class |  |
| `classnohierarchy` | select | Class (no hierarchy) |  |
| `customer` | select | Customer:Job |  |
| `date` | date | Date |  |
| `datecreated` | date | Date Created |  |
| `department` | select | Department |  |
| `departmentnohierarchy` | select | Department (no hierarchy) |  |
| `durationdecimal` | float | Duration (Decimal) |  |
| `employee` | select | Employee |  |
| `endtime` | timeofday | End Time |  |
| `externalid` | select | External ID |  |
| `formulacurrency` | currency | Formula (Currency) |  |
| `formuladate` | date | Formula (Date) |  |
| `formuladatetime` | datetime | Formula (Date/Time) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulapercent` | percent | Formula (Percent) |  |
| `formulatext` | text | Formula (Text) |  |
| `hours` | text | Duration |  |
| `internalid` | select | Internal ID |  |
| `isbillable` | checkbox | Billable |  |
| `isexempt` | checkbox | Exempt |  |
| `isproductive` | checkbox | Productive |  |
| `isutilized` | checkbox | Utilized |  |
| `item` | text | Item |  |
| `lastmodified` | date | Last Modified |  |
| `location` | select | Location |  |
| `locationnohierarchy` | select | Location (no hierarchy) |  |
| `memo` | text | Note |  |
| `paidexternally` | checkbox | Paid Externally |  |
| `payitem` | select | Payroll Item |  |
| `payrolldate` | date | Payroll Date |  |
| `rate` | currency | Rate |  |
| `rejectionnote` | text | Rejection Note |  |
| `starttime` | timeofday | Start Time |  |
| `status` | text | Status |  |
| `subsidiary` | text | Subsidiary |  |
| `subsidiarynohierarchy` | text | Subsidiary (no hierarchy) |  |
| `supervisorapproval` | checkbox | Approved |  |
| `temporarylocaljurisdiction` | select | Temporary Local Jurisdiction |  |
| `temporarystatejurisdiction` | select | Temporary State Jurisdiction |  |
| `timemodified` | checkbox | Time Modified |  |
| `timesheet` | select | Timesheet |  |
| `type` | select | Type |  |

## Search joins

| Join ID | Label | Joined record |
|---|---|---|
| `charge` | Charge | Charge |
| `customer` | Customer | Customer |
| `employee` | Employee | Employee |
| `job` | Job | Job |
| `projecttask` | Project Task | ProjectTask |
| `projecttaskassignment` | Project Task Assignment | Project Task Assignment |
| `task` | Task | Task |
| `timesheet` | Weekly Timesheet | Timesheet |

## Suspected source errors

> [!SOURCE-ERROR] `submit_parameters_text_says_approve`
<!-- source-error: record:timebill | - | submit_parameters_text_says_approve -->

> [!SOURCE-ERROR] `help_says_tax_exempt` (on `isexempt`)
<!-- source-error: record:timebill | isexempt | help_says_tax_exempt -->
