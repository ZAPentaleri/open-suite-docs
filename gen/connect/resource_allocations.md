# Resource_allocations (`resource_allocations`)
<!-- id: connect:resource_allocations -->

| Fact | Value |
|---|---|
| Table | `resource_allocations` |
| Record | `resourceallocation` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `allocation_type` | VARCHAR2 | 200 |  |  |  |  | `purpose`: `allocation_type`; `options`: `soft`, `hard` |
| `allocation_unit` | VARCHAR2 | 1 |  |  |  |  | `purpose`: `allocation_unit`; `option_meanings`: `H:hours`, `P:percent_of_time` |
| `customer_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `allocation_customer` |
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `last_modification_date` |
| `end_date` | TIMESTAMP |  |  |  |  |  | `purpose`: `end_date` |
| `is_inactive` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `inactive` |
| `notes` | VARCHAR2 | 4000 |  |  |  |  | `purpose`: `notes` |
| `num_hours` | NUMBER |  | 10 | 4 |  |  | `purpose`: `number_of_hours` |
| `num_percent` | NUMBER |  | 12 | 4 |  |  | `purpose`: `allocated_working_time_percent`; `value_source`: `resource_work_calendar` |
| `project_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `allocation_project` |
| `project_task_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `project_task_id` |
| `requestor_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `allocation_requestor`; `same_as`: `allocation_owner` |
| `resource_allocation_extid` | VARCHAR2 | 255 |  |  |  |  | `purpose`: `resource_allocation_external_id` |
| `resource_allocation_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `resource_allocation_id` |
| `resource_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `allocated_resource_employee_vendor` |
| `start_date` | TIMESTAMP |  |  |  |  |  | `purpose`: `start_date` |
