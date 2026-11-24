# Manufacturing Operation Task (`manufacturingoperationtask`)
<!-- id: record:manufacturingoperationtask -->

| Fact | Value |
|---|---|
| Internal ID | `manufacturingoperationtask` |
| Category | transaction |
| Client scripts | yes |
| Server scripts | yes |
| Tags | `custom_fields_supported`, `no_copy` |

## Record facts

- `script_types`: `client`, `server`
- `not_supported`: `copy`
- `requires_feature`: `Manufacturing Routing and Work Center` [`real_name`]
- `not_supported`: `copy`
- `purpose`: `routing_step_job_for_employee_group_on_wip_work_order`
- `effect`: `created_automatically_from_routing_on_wip_work_order_save`
- `displayed_on`: `work_order_operations_subtab`
- `ui_effect`: `new_operation_task_button_on_operations_subtab`
- `ui_location`: `transactions_manufacturing_manufacturing_operation_tasks`
- `configured_by`: `enable_features_items_and_inventory_subtab` [`manufacturing_routing_and_work_center`]
- `script_functions`: `create`, `update`, `delete`, `search`
- `user_events`: `beforeLoad`, `beforeSubmit`, `afterSubmit`
- `requires`: `initvalues_reference_existing_wip_work_order` (when `creating`)

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `actualwork` | float | Actual Hours | no | no |  | `purpose`: `actual_hours`; `value_source`: `system_displayed` |
| `autocalculatelag` | checkbox | Auto-calculate Lag | no | no |  | `purpose`: `auto_calculate_lag` |
| `completedquantity` | float | Completed Quantity | no | no |  | `purpose`: `completed_quantity` |
| `customform` | select | Custom Form | no | no |  | `purpose`: `custom_form` |
| `enddate` | date | End Date | no | no |  | `purpose`: `production_end_date`; `default_behavior`: `today_plus_lead_time`; `effect`: `demand_planning_supply_date` |
| `estimatedwork` | float | Estimated Hours | no | no |  | `purpose`: `estimated_hours`; `value_source`: `setup_time_plus_quantity_times_run_rate` |
| `inputquantity` | float | Input Quantity | no | no |  | `purpose`: `input_quantity` |
| `laborresources` | double | Labor Resources | yes | no |  | `purpose`: `labor_resources`; `accepts_forms`: `decimal_fraction`; `doc_inconsistency`: `example_value_off_by_factor_of_ten` |
| `machineresources` | double | Machine Resources | yes | no |  | `purpose`: `machine_resources`; `accepts_forms`: `decimal_fraction`; `doc_inconsistency`: `example_value_off_by_factor_of_ten` |
| `manufacturingcosttemplate` | select | Manufacturing Cost Template | yes | no |  | `purpose`: `manufacturing_cost_template`; `effect`: `cost_updated_from_template_on_time_entry` |
| `manufacturingworkcenter` | select | Manufacturing Work Center | yes | no |  | `purpose`: `work_center`; `value_kind`: `group_of_people_and_machines` |
| `message` | textarea | Comments | no | no |  | `purpose`: `task_comments` |
| `operationsequence` | integer | Operation Sequence | yes | no |  | `purpose`: `operation_sequence`; `value_examples`: `setup_is_sequence_1` |
| `order` | select | Insert Before | no | no |  | `purpose`: `insert_before_step` |
| `remainingwork` | float | Remaining Hours | no | no |  | `purpose`: `remaining_hours`; `value_source`: `system_displayed` |
| `runrate` | float | Run Rate (Min/Unit) | yes | no |  | `purpose`: `run_rate_minutes_per_unit`; `value_unit`: `minutes`; `doc_inconsistency`: `help_sentence_repeated` |
| `setuptime` | float | Setup Time (Min) | yes | no |  | `purpose`: `setup_time`; `value_unit`: `minutes` |
| `startdate` | date | Start Date | no | no |  | `purpose`: `production_start_date`; `default_behavior`: `current_date`; `effect`: `demand_planning_component_demand_date` |
| `status` | select | Status | no | no |  | `purpose`: `operation_status`; `value_examples`: `Not Started`, `Started`, `Completed` |
| `title` | text | Operation Name | yes | no |  | `purpose`: `operation_name` |
| `workorder` | select | Work Order | no | no |  | `purpose`: `related_work_order` |
| `actualruntime` | float | Actual Run Time (Min) | no | no |  |  |
| `actualsetuptime` | float | Actual Setup Time (Min) | no | no |  |  |
| `company` | select | Project | yes | no |  |  |
| `constrainttype` | select | Type | no | no |  |  |
| `contact` | select | Contact | no | no |  |  |
| `enddatebaseline` | datetime | End Date | no | no |  |  |
| `estimatedworkbaseline` | float | Hours | no | no |  |  |
| `eventid` | integer | ID | yes | no |  |  |
| `externalid` | text | ExternalId | no | no |  |  |
| `fxrate` | currency2 | Exchange Rate | no | no |  |  |
| `owner` | select | Owner | no | no |  |  |
| `percenttimecomplete` | percent | Percent Complete | no | no |  |  |
| `priority` | select | Priority | no | no |  |  |
| `startdatebaseline` | datetime | Start Date | no | no |  |  |
| `starttime` | timeofday | Start Time | no | no |  |  |

## Sublist `costdetail` (Costs)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `account` | integer |  | no |  |  |
| `costcategory` | select | Cost Category | yes |  |  |
| `fixedrate` | currency2 | Fixed Rate | no |  |  |
| `id` | integer |  | no |  |  |
| `item` | select | Item | yes |  |  |
| `runrate` | currency2 | Run Rate | no |  |  |

## Sublist `predecessor` (Predecessors)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `enddate` | datetime | End Date | no |  |  |
| `lagamount` | integer | Lag Amount | no |  |  |
| `lagtype` | select | Lag Type | no |  |  |
| `lagunits` | text | Lag Units | no |  |  |
| `startdate` | datetime | Start Date | no |  |  |
| `task` | select | Task | yes |  |  |
| `type` | select | Type | yes |  |  |

## Tabs

| Tab ID | Label |
|---|---|
| `assignees` | Assignees |
| `communication` | Communication |
| `costdetails` | Cost Details |
| `custom` | Custom |
| `messagetab` | Message |
| `predecessors` | Predecessors |
| `s_sysinfo` | System Information |
| `workflow` | Workflow |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `actualruntime` | float | Actual Run Time |  |
| `actualsetuptime` | float | Actual Setup Time |  |
| `completedquantity` | float | Completed Quantity |  |
| `enddate` | date | End Date |  |
| `estimatedwork` | float | Estimated Work |  |
| `externalid` | select | External ID |  |
| `externalidstring` | text | External ID (Text) |  |
| `formuladate` | date | Formula (Date) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulatext` | text | Formula (Text) |  |
| `id` | integer | ID |  |
| `inputquantity` | float | Input Quantity |  |
| `internalid` | select | Internal ID |  |
| `internalidnumber` | integer | Internal ID (Number) |  |
| `laborresources` | double | Labor Resources |  |
| `lagamount` | float | Lag Amount |  |
| `lagtype` | select | Lag Type |  |
| `lagunits` | text | Lag Units |  |
| `machineresources` | double | Machine Resources |  |
| `manufacturingcosttemplate` | select | Manufacturing Cost Template |  |
| `manufacturingworkcenter` | select | Manufacturing Work Center |  |
| `name` | text | Operation Name |  |
| `order` | float | Order |  |
| `predecessor` | select | Predecessor |  |
| `remainingquantity` | float | Remaining Quantity |  |
| `runrate` | float | Run Rate (Min/Unit) |  |
| `runtime` | float | Run Time (Min) |  |
| `sequence` | integer | Operation Sequence |  |
| `setuptime` | float | Setup Time (Min) |  |
| `startdate` | date | Start Date |  |
| `status` | select | Status |  |
| `workorder` | select | Work Order |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `actualruntime` | float | Actual Run Time |  |
| `actualsetuptime` | float | Actual Setup Time |  |
| `completedquantity` | float | Completed Quantity |  |
| `enddate` | date | End Date |  |
| `estimatedwork` | float | Estimated Work |  |
| `externalid` | select | External ID |  |
| `formulacurrency` | currency | Formula (Currency) |  |
| `formuladate` | date | Formula (Date) |  |
| `formuladatetime` | datetime | Formula (Date/Time) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulapercent` | percent | Formula (Percent) |  |
| `formulatext` | text | Formula (Text) |  |
| `id` | integer | ID |  |
| `inputquantity` | float | Input Quantity |  |
| `internalid` | select | Internal ID |  |
| `laborresources` | double | Labor Resources |  |
| `lagamount` | float | Lag Amount |  |
| `lagtype` | text | Lag Type |  |
| `lagunits` | text | Lag Units |  |
| `machineresources` | double | Machine Resources |  |
| `manufacturingcosttemplate` | select | Manufacturing Cost Template |  |
| `manufacturingworkcenter` | select | Manufacturing Work Center |  |
| `message` | text | Notes |  |
| `name` | text | Operation Name |  |
| `order` | float | Order |  |
| `predecessor` | select | Predecessor |  |
| `predecessortype` | text | Predecessor Type |  |
| `remainingquantity` | float | Remaining Quantity |  |
| `runrate` | float | Run Rate (Min/Unit) |  |
| `runtime` | float | Run Time |  |
| `sequence` | integer | Operation Sequence |  |
| `setuptime` | float | Setup Time (Min) |  |
| `startdate` | date | Start Date |  |
| `status` | select | Status |  |
| `workorder` | text | Work Order |  |

## Search joins

| Join ID | Label | Joined record |
|---|---|---|
| `user` | User | Employee |

## Suspected source errors

> [!SOURCE-ERROR] `example_value_off_by_factor_of_ten` (on `laborresources`)
<!-- source-error: record:manufacturingoperationtask | laborresources | example_value_off_by_factor_of_ten -->

> [!SOURCE-ERROR] `example_value_off_by_factor_of_ten` (on `machineresources`)
<!-- source-error: record:manufacturingoperationtask | machineresources | example_value_off_by_factor_of_ten -->

> [!SOURCE-ERROR] `help_sentence_repeated` (on `runrate`)
<!-- source-error: record:manufacturingoperationtask | runrate | help_sentence_repeated -->
