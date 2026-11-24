# Project_tasks (`project_tasks`)
<!-- id: connect:project_tasks -->

| Fact | Value |
|---|---|
| Table | `project_tasks` |
| Record | `projecttask` |
| Primary key | `projecttask_id` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `actual_work` | NUMBER |  | 39 | 0 |  |  | `purpose`: `task_work_completed` |
| `allocated_work` | NUMBER |  | 39 | 0 |  |  | `purpose`: `allocated_work` |
| `assigned_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `assigned_to` |
| `calculated_work` | NUMBER |  | 39 | 0 |  |  | `purpose`: `task_calculated_work_current` |
| `calculated_work_baseline` | NUMBER |  | 39 | 0 |  |  | `purpose`: `task_calculated_work_baseline` |
| `constraint_type` | VARCHAR2 | 15 |  |  |  |  | `purpose`: `task_date_constraint_type`; `options`: `as_soon_as_possible`, `fixed_date` |
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `date_last_modified`; `time_zone`: `gmt` |
| `due_date` | TIMESTAMP |  |  |  |  |  | `purpose`: `task_due_date` |
| `end_date` | TIMESTAMP |  |  |  |  |  | `purpose`: `task_estimated_end_current` |
| `end_date_baseline` | TIMESTAMP |  |  |  |  |  | `purpose`: `task_estimated_end_baseline` |
| `estimated_cost` | NUMBER |  | 39 | 0 |  |  | `purpose`: `task_estimated_cost_current` |
| `estimated_cost_baseline` | NUMBER |  | 39 | 0 |  |  | `purpose`: `task_estimated_cost_baseline` |
| `estimated_cost_baseline_frn` | NUMBER |  | 39 | 0 |  |  | `purpose`: `task_estimated_cost_baseline`; `value_unit`: `foreign_currency` |
| `estimated_cost_foreign` | NUMBER |  | 39 | 0 |  |  | `purpose`: `task_estimated_cost_current`; `value_unit`: `foreign_currency` |
| `estimated_work` | NUMBER |  | 39 | 0 |  |  | `purpose`: `task_estimated_work_current` |
| `estimated_work_baseline` | NUMBER |  | 39 | 0 |  |  | `purpose`: `task_estimated_work_baseline` |
| `finish_by_date` | TIMESTAMP |  |  |  |  |  | `purpose`: `finish_no_later_than` |
| `is_milestone` | VARCHAR2 | 1 |  |  |  |  | `purpose`: `task_is_milestone` |
| `is_non_billable` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `task_non_billable` |
| `is_on_critical_path` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `task_on_critical_path` |
| `is_summary_task` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `is_parent_task` |
| `isinactive` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `is_inactive` |
| `message` | VARCHAR2 | 4000 |  |  |  |  | `purpose`: `notes_about_project_task` |
| `name` | VARCHAR2 | 256 |  |  |  |  | `purpose`: `project_task_name` |
| `owner_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `project_task_owner` |
| `parent_task_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `parent_project_task`; `caveat`: `empty_for_top_level_tasks` |
| `percent_complete` | NUMBER |  | 39 | 0 |  |  | `purpose`: `task_percent_complete` |
| `percent_complete_by_allocation` | NUMBER |  | 39 | 0 |  |  | `purpose`: `task_percent_complete_by_allocation` |
| `planned_work` | NUMBER |  | 39 | 0 |  |  | `purpose`: `task_planned_work_current` |
| `planned_work_baseline` | NUMBER |  | 39 | 0 |  |  | `purpose`: `task_planned_work_baseline` |
| `priority` | VARCHAR2 | 32 |  |  |  |  | `purpose`: `task_priority`; `options`: `high`, `medium`, `low` |
| `project_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `project_id` |
| `projecttask_extid` | VARCHAR2 | 255 |  |  |  |  | `purpose`: `project_task_external_id` |
| `projecttask_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `project_task_id` |
| `slack_minutes` | NUMBER |  | 39 | 0 |  |  | `purpose`: `task_slack`; `value_unit`: `minutes`; `value_kind`: `start_delay_not_delaying_project_end` |
| `start_date` | TIMESTAMP |  |  |  |  |  | `purpose`: `task_estimated_start_current` |
| `start_date_0` | TIMESTAMP |  |  |  |  |  | `purpose`: `start_date` |
| `start_date_baseline` | TIMESTAMP |  |  |  |  |  | `purpose`: `task_estimated_start_baseline` |
| `status` | VARCHAR2 | 64 |  |  |  |  | `purpose`: `status_of_project_task` |
| `taskorder` | NUMBER |  | 39 | 0 |  |  | `purpose`: `project_task_that_follows` |
| `use_calculated_billing_budget` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `use_calculated_billing_budget` |
| `use_calculated_cost_budget` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `use_calculated_cost_budget` |
