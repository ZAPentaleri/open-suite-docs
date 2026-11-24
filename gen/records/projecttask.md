# Project Task (`projecttask`)
<!-- id: record:projecttask -->

| Fact | Value |
|---|---|
| Internal ID | `projecttask` |
| Category | event |
| Client scripts | yes |
| Server scripts | yes |
| Searchable | yes |
| Tags | `custom_fields_supported`, `scripting_full` |

## Record facts

- `requires_feature`: `Project Management` [`real_name`]
- `standalone`: `false`
- `parent_record` (refs `record:job`)
- `required_fields`: `company:project_id`
- `task_kinds`: `project_task`, `milestone_task`
- `rule`: `saved_as_milestone_without_estimated_work`
- `rule`: `estimatedwork_body_overwritten_by_assignee_sum`
- `doc_inconsistency`: `script_type_section_names_project_record`
- `ui_effect`: `project_new_project_task_or_new_milestone_buttons`
- `configured_by`: `enable_features_company_subtab` [`project_management`]
- `workflow`: `clear_estimatedwork`, `clear_assignees` (when `converting_task_to_milestone`)
- `workflow`: `set_estimatedwork_or_add_assignee_with_positive_work` (when `converting_milestone_to_task`)
- `can`: `set_estimatedwork_body_when_no_assignees`
- `script_types`: `server`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `actualwork` | float | Actual Work | no | no |  | `purpose`: `actual_work`; `value_source`: `system_displayed` |
| `bbudgetshowcalculatedlines` | checkbox | Show Calculated Lines | no | no |  | `purpose`: `show_calculated_budget_lines`; `help_text_shared_with`: `projecttask.cbudgetshowcalculatedlines` |
| `bbudgetusecalculatedvalues` | checkbox | Use Calculated Values for all Labor Billing Budgets | no | no |  | `purpose`: `use_calculated_billing_budgets`; `cannot`: `enter_amounts_manually` (when `checked`) |
| `cbudgetshowcalculatedlines` | checkbox | Show Calculated Lines | no | no |  | `purpose`: `show_calculated_budget_lines`; `help_text_shared_with`: `projecttask.bbudgetshowcalculatedlines` |
| `cbudgetusecalculatedvalues` | checkbox | Use Calculated Values for all Labor Cost Budgets | no | no |  | `purpose`: `use_calculated_cost_budgets`; `cannot`: `enter_amounts_manually` (when `checked`) |
| `company` | select | Project Name | yes | no |  | `purpose`: `task_project`; `cannot`: `move_task_between_projects` |
| `constrainttype` | select | Constraint Type | no | no |  | `purpose`: `task_constraint_type`; `options`: `As Soon As Possible`, `Fixed Date`; `option_meanings`: `asap:earliest_after_predecessors`, `fixed_date:given_start_predecessors_ignored` |
| `contact` | select | Contact | no | no |  | `purpose`: `task_contact` |
| `customform` | select | Custom Form | yes | no |  | `purpose`: `entry_form`; `special_values`: `Standard Project Task Form`; `ui_effect`: `new_option_creates_record` |
| `enddate` | date | End Date | no | no |  | `purpose`: `estimated_end_date`; `value_source`: `estimated_work_and_dependencies`; `value_source`: `last_time_entry_date` (when `completed`) |
| `enddatebaseline` | datetime | End Date Baseline | no | no |  | `purpose`: `baseline_end_date` |
| `estimatedwork` | float | Estimated Work | no | no |  | `purpose`: `estimated_work`; `value_source`: `sum_of_resource_estimates`; `effect`: `entered_value_split_equally_among_resources`; `effect`: `set_to_actual_work_when_completed` |
| `estimatedworkbaseline` | float | Estimated Work Baseline | no | no |  | `purpose`: `baseline_estimated_work`; `value_source`: `value_when_baseline_set`; `help_text_shared_with`: `job.estimatedtimeoverridebaseline` |
| `finishbydate` | date | Finish No Later Than | no | no |  | `purpose`: `finish_by_date`; `effect`: `overrides_relationships_and_adjusts_start` |
| `message` | textarea |  | no | no |  | `purpose`: `task_comments` |
| `nonbillabletask` | checkbox | Non-billable | no | no |  | `purpose`: `non_billable_task`; `effect`: `time_entries_forced_non_billable` |
| `order` | select | Insert Before | no | no |  | `purpose`: `insert_before_task` |
| `owner` | select | Created By | no | no |  | `purpose`: `task_creator`; `value_source`: `system_displayed` |
| `parent` | select | Parent Task | no | no |  | `purpose`: `parent_task`; `effect`: `parent_summarizes_children`; `cannot`: `assign_resources_to_parent_task` |
| `percenttimecomplete` | percent | Percent Complete | no | no |  | `purpose`: `percent_complete`; `value_source`: `actual_divided_by_estimated_work` |
| `plannedwork` | float | Planned Work | yes | no |  | `purpose`: `planned_work`; `caveat`: `unplanned_time_not_added` |
| `priority` | select | Priority | yes | yes |  | `purpose`: `task_priority`; `caveat`: `does_not_affect_schedule` |
| `remainingwork` | float | Remaining Work | no | no |  | `purpose`: `remaining_work`; `value_source`: `estimated_minus_actual_work`; `default_value`: `0` (when `completed`) |
| `startdate` | date | Start Date | no | no |  | `purpose`: `task_start_date`; `value_source`: `schedule` (when `as_soon_as_possible`) |
| `startdatebaseline` | datetime | Start Date Baseline | no | no |  | `purpose`: `baseline_start_date` |
| `starttime` | timeofday | Start Time | no | no |  | `purpose`: `task_start_time`; `default_value`: `12:00 am` (when `blank`); `value_unit`: `creator_time_zone` |
| `status` | select | Status | yes | yes |  | `purpose`: `task_status`; `allowed_values`: `Completed`, `In Progress`, `Not Started`; `requires_permission`: `edit_project_task` |
| `title` | text | Project Task Name | yes | no |  | `purpose`: `task_name` |
| `eventid` | integer | Project Task ID | yes | no |  |  |
| `externalid` | text | ExternalId | no | no |  |  |
| `fxrate` | currency2 | Exchange Rate | no | no |  |  |
| `ismilestone` | checkbox |  | no | no |  |  |
| `isoncriticalpath` | text |  | no | no |  |  |
| `lateend` | date |  | no | no |  |  |
| `latestart` | date |  | no | no |  |  |
| `plannedworkbaseline` | float | Planned Work Baseline | no | no |  |  |
| `slackminutes` | float |  | no | no |  |  |

## Sublist `assignee` (Assignees)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `billingclass` | select | Billing Class | no |  |  |
| `cost` | currency | Cost | no |  |  |
| `estimatedwork` | float | Estimated Work | yes |  |  |
| `id` | integer |  | no |  |  |
| `plannedwork` | float | Planned Work | yes |  |  |
| `price` | currency | Price | no |  |  |
| `resource` | select | Resource | yes |  |  |
| `serviceitem` | select | Service Item | no |  |  |
| `unitcost` | currency | Unit Cost | yes |  |  |
| `unitprice` | currency | Unit Price | no |  |  |
| `units` | percent | Units | no |  |  |

## Sublist `bbudget` (Billing Budget)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `costcategoryref` | integer |  | no |  |  |
| `costcategorysubtype` | text |  | no |  |  |
| `costcategorytype` | text |  | no |  |  |
| `selectline` | checkbox | Select | no |  |  |
| `totalamount` | currency | Task Total | no |  |  |

## Sublist `cbudget` (Cost Budget)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `costcategoryref` | integer |  | no |  |  |
| `costcategorysubtype` | text |  | no |  |  |
| `costcategorytype` | text |  | no |  |  |
| `selectline` | checkbox | Select | no |  |  |
| `totalamount` | currency | Task Total | no |  |  |

## Sublist `predecessor` (Predecessors)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `enddate` | datetime | End Date | no |  |  |
| `startdate` | datetime | Start Date | no |  |  |
| `task` | select | Task | yes |  |  |
| `type` | select | Type | yes |  |  |

## Tabs

| Tab ID | Label |
|---|---|
| `assignees` | Assignees |
| `budget` | Budget |
| `communication` | Communication |
| `custom` | Custom |
| `history` | History |
| `media` | File Cabinet |
| `predecessors` | Predecessors |
| `time` | Time |
| `workflow` | Workflow |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `actualwork` | float | Actual Work |  |
| `assignee` | select | Assignee |  |
| `company` | select | Project/Project Template |  |
| `constrainttype` | select | Constraint Type |  |
| `contact` | select | Contact |  |
| `cost` | currency | Estimated Cost |  |
| `costbase` | currency | Estimated Cost (Base Currency) |  |
| `costbasebaseline` | currency | Estimated Cost Baseline (Base Currency) |  |
| `costbaseline` | currency | Estimated Cost Baseline |  |
| `costbasevariance` | currency | Estimated Cost Variance (Base Currency) |  |
| `costvariance` | currency | Estimated Cost Variance |  |
| `costvariancepercent` | float | Estimated Cost Variance Percent |  |
| `createddate` | datetime | Date Created |  |
| `enddate` | date | End Date |  |
| `enddatebaseline` | date | End Date Baseline |  |
| `enddatevariance` | float | End Date Variance |  |
| `estimatedwork` | float | Estimated Work |  |
| `estimatedworkbaseline` | float | Estimated Work Baseline |  |
| `estimatedworkvariance` | float | Estimated Work Variance |  |
| `estimatedworkvariancepercent` | float | Estimated Work Variance Percent |  |
| `externalid` | select | External ID |  |
| `externalidstring` | text | External ID (Text) |  |
| `finishbydate` | date | Finish No Later Than Date |  |
| `formuladate` | date | Formula (Date) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulatext` | text | Formula (Text) |  |
| `id` | integer | ID |  |
| `internalid` | select | Internal ID |  |
| `internalidnumber` | integer | Internal ID (Number) |  |
| `ismilestone` | checkbox | Milestone |  |
| `issummarytask` | checkbox | Is Job Summary Task |  |
| `lastmodifieddate` | datetime | Date Last Modified |  |
| `nonbillabletask` | checkbox | Non-billable |  |
| `owner` | select | Created By |  |
| `parent` | select | Parent Task |  |
| `percentworkcomplete` | percent | Percent Work Complete |  |
| `plannedwork` | float | Planned Work |  |
| `plannedworkbaseline` | float | Planned Work Baseline |  |
| `predecessor` | select | Predecessor |  |
| `predecessors` | text | Predecessors |  |
| `priority` | select | Priority |  |
| `remainingwork` | float | Remaining Work |  |
| `startdate` | date | Start Date |  |
| `startdatebaseline` | date | Start Date Baseline |  |
| `startdatevariance` | float | Start Date Variance |  |
| `status` | select | Status |  |
| `successor` | select | Successor |  |
| `title` | text | Task Name |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `actualwork` | float | Actual Work |  |
| `bbudgetusecalculatedvalues` | checkbox | Use Calculated Values for all Labor Billing Budgets |  |
| `cbudgetusecalculatedvalues` | checkbox | Use Calculated Values for all Labor Cost Budgets |  |
| `company` | select | Project |  |
| `constrainttype` | select | Constraint Type |  |
| `contact` | select | Contact |  |
| `cost` | currency | Estimated Cost |  |
| `costbase` | currency | Estimated Cost (Base Currency) |  |
| `costbasebaseline` | currency | Estimated Cost Baseline (Base Currency) |  |
| `costbaseline` | currency | Estimated Cost Baseline |  |
| `costbasevariance` | currency | Estimated Cost Variance (Base Currency) |  |
| `costvariance` | currency | Estimated Cost Variance |  |
| `costvariancepercent` | float | Estimated Cost Variance Percent |  |
| `createddate` | datetime | Date Created |  |
| `enddate` | date | End Date |  |
| `enddatebaseline` | date | End Date Baseline |  |
| `enddatevariance` | float | End Date Variance |  |
| `estimatedwork` | float | Estimated Work |  |
| `estimatedworkbaseline` | float | Estimated Work Baseline |  |
| `estimatedworkvariance` | float | Estimated Work Variance |  |
| `estimatedworkvariancepercent` | float | Estimated Work Variance Percent |  |
| `externalid` | select | External ID |  |
| `finishbydate` | date | Finish No Later Than Date |  |
| `formulacurrency` | currency | Formula (Currency) |  |
| `formuladate` | date | Formula (Date) |  |
| `formuladatetime` | datetime | Formula (Date/Time) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulapercent` | percent | Formula (Percent) |  |
| `formulatext` | text | Formula (Text) |  |
| `id` | integer | ID |  |
| `internalid` | select | Internal ID |  |
| `ismilestone` | checkbox | Milestone |  |
| `issummarytask` | checkbox | Is Summary Task |  |
| `lastmodifieddate` | datetime | Last Modified |  |
| `message` | text | Notes |  |
| `nonbillabletask` | checkbox | Non-billable |  |
| `owner` | select | Created By |  |
| `parent` | select | Parent Task |  |
| `percentworkcomplete` | percent | Percent Work Complete |  |
| `plannedwork` | float | Planned Work |  |
| `plannedworkbaseline` | float | Planned Work Baseline |  |
| `predecessor` | select | Predecessor |  |
| `predecessors` | text | Predecessors |  |
| `predecessortype` | text | Predecessor Type |  |
| `priority` | select | Priority |  |
| `remainingwork` | float | Remaining Work |  |
| `startdate` | date | Start Date |  |
| `startdatebaseline` | date | Start Date Baseline |  |
| `startdatevariance` | float | Start Date Variance |  |
| `status` | select | Status |  |
| `successor` | select | Successor |  |
| `successortype` | text | Successor Type |  |
| `title` | text | Task Name |  |

## Search joins

| Join ID | Label | Joined record |
|---|---|---|
| `job` | Job | Job |
| `predecessor` | Predecessor | ProjectTask |
| `projecttaskassignment` | Project Task Assignment | Project Task Assignment |
| `successor` | Successor | ProjectTask |
| `transaction` | Transaction | Transaction |

## Suspected source errors

> [!SOURCE-ERROR] `script_type_section_names_project_record`
<!-- source-error: record:projecttask | - | script_type_section_names_project_record -->
