# Project_task_assignments (`project_task_assignments`)
<!-- id: connect:project_task_assignments -->

| Fact | Value |
|---|---|
| Table | `project_task_assignments` |
| Record | `projecttaskassignment` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `allocated_work` | NUMBER |  | 39 | 0 |  |  | `purpose`: `allocated_work` |
| `billing_class_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `assignment_billing_class`; `applies_to`: `employees`, `vendors` |
| `calculated_work` | NUMBER |  | 39 | 0 |  |  | `purpose`: `task_calculated_work_current` |
| `estimated_work` | NUMBER |  | 39 | 0 |  |  | `purpose`: `resource_time_required_for_task` |
| `item_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `assignment_service_item` |
| `planned_work` | NUMBER |  | 39 | 0 |  |  | `purpose`: `task_planned_work_current` |
| `project_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `project_id` |
| `project_task_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `project_task_id` |
| `resource_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `resource_id` |
| `unit_cost` | NUMBER |  | 39 | 0 |  |  | `purpose`: `resource_unit_cost` |
| `unit_cost_foreign` | NUMBER |  | 39 | 0 |  |  | `purpose`: `resource_unit_cost`; `value_unit`: `foreign_currency` |
| `unit_price` | NUMBER |  | 39 | 0 |  |  | `purpose`: `resource_unit_price` |
| `unit_price_foreign` | NUMBER |  | 39 | 0 |  |  | `purpose`: `resource_unit_price`; `value_unit`: `foreign_currency` |
| `units` | NUMBER |  | 39 | 0 |  |  | `purpose`: `resource_share_on_task`; `value_unit`: `percent` |
