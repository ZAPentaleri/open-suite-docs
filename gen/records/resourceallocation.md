# Resource Allocation (`resourceallocation`)
<!-- id: record:resourceallocation -->

| Fact | Value |
|---|---|
| Internal ID | `resourceallocation` |
| Category | activity |
| Client scripts | yes |
| Server scripts | yes |
| Searchable | yes |
| Tags | `custom_fields_supported`, `scripting_full` |

## Record facts

- `script_types`: `client`, `server`
- `script_functions`: `create`, `update`, `copy`, `delete`, `search`
- `user_events`: `beforeLoad`, `beforeSubmit`, `afterSubmit`
- `requires_feature`: `Resource Allocations` [`real_name`]
- `required_fields`: `project`, `allocationresource`
- `allocationresource_requires`: `employee_marked_project_resource`
- `read_only_fields`: `numberhours`, `percentoftime`
- `purpose`: `reserve_employee_time_for_project`
- `ui_location`: `activities_scheduling_resource_allocations`
- `displayed_on`: `project_resources_subtab`
- `configured_by`: `enable_features_company_subtab` [`resource_allocations`]
- `caveat`: `feature_option_hidden_until_account_provisioned`
- `ui_location`: `employee_human_resources_subtab_project_resource_box` (when `making_employee_allocatable`)
- `doc_inconsistency`: `percentoftime_misspelled_and_project_record_vs_resource`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `allocationamount` | posfloat | Allocate | yes | no |  | `purpose`: `allocation_amount`; `accepts_forms`: `hours`, `percent_of_time`; `value_examples`: `50_percent_of_8h_day_two_weeks_is_40_hours` |
| `allocationresource` | select | Resource | yes | no |  | `purpose`: `allocated_resource`; `constraint`: `project_resource_employees_or_vendors_only` |
| `allocationtype` | select | Allocation Type | yes | no |  | `purpose`: `allocation_type`; `options`: `Hard`, `Soft`; `option_meanings`: `hard:committed_dates_and_hours`, `soft:adjustable` |
| `allocationunit` | select |  | no | no |  | `purpose`: `allocation_unit`; `allowed_values`: `Hours`, `Percent of Time`; `value_source`: `work_calendar_for_percent` [`hours`] |
| `customform` | select | Custom Form | yes | no |  | `purpose`: `custom_form` |
| `enddate` | date | End Date | yes | no |  | `purpose`: `allocation_end_date` |
| `notes` | textarea | Notes | no | no |  | `purpose`: `notes` |
| `numberhours` | posfloat | Number of Hours | no | no |  | `purpose`: `allocated_hours`; `value_source`: `percent_dates_and_work_calendar` (when `percent_of_time`) |
| `percentoftime` | posfloat | Percentage of Time | no | no |  | `purpose`: `allocated_percent`; `value_source`: `hours_dates_and_work_calendar` (when `number_of_hours`) |
| `project` | select | Customer:Project | yes | no |  | `purpose`: `allocation_project` |
| `startdate` | date | Start Date | yes | no |  | `purpose`: `allocation_start_date` |
| `externalid` | text | ExternalId | no | no |  |  |
| `requestor` | select | Requested By | no | no |  |  |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `allocationtype` | select | Allocation Type |  |
| `allocationunit` | select | Allocate By |  |
| `approvalstatus` | select | Approval Status |  |
| `customer` | select | Customer |  |
| `enddate` | date | End Date |  |
| `externalid` | select | External ID |  |
| `externalidstring` | text | External ID (Text) |  |
| `formuladate` | date | Formula (Date) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulatext` | text | Formula (Text) |  |
| `internalid` | select | Internal ID |  |
| `internalidnumber` | integer | Internal ID (Number) |  |
| `nextapprover` | select | Next Approver |  |
| `notes` | text | Notes |  |
| `numberhours` | float | Number of Hours |  |
| `percentoftime` | percent | Percentage of Time |  |
| `project` | select | Project |  |
| `requestedby` | select | Requested by |  |
| `resource` | select | Resource |  |
| `startdate` | date | Start Date |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `allocationtype` | select | Allocation Type |  |
| `allocationunit` | select | Allocate By |  |
| `approvalstatus` | select | Approval Status |  |
| `company` | select | Project |  |
| `customer` | select | Customer |  |
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
| `nextapprover` | select | Next Approver |  |
| `notes` | text | Notes |  |
| `numberhours` | float | Number of Hours |  |
| `percentoftime` | percent | Percentage of Time |  |
| `requestedby` | select | Requested By |  |
| `resource` | select | Resource |  |
| `startdate` | date | Start Date |  |

## Search joins

| Join ID | Label | Joined record |
|---|---|---|
| `customer` | Customer | Customer |
| `employee` | Resource - Employee | Employee |
| `job` | Project | Job |
| `user` | User | Employee |
| `vendor` | Resource - Vendor | Vendor |

## Suspected source errors

> [!SOURCE-ERROR] `percentoftime_misspelled_and_project_record_vs_resource`
<!-- source-error: record:resourceallocation | - | percentoftime_misspelled_and_project_record_vs_resource -->
