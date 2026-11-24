# Project_task_dependencies (`project_task_dependencies`)
<!-- id: connect:project_task_dependencies -->

| Fact | Value |
|---|---|
| Table | `project_task_dependencies` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `dependency_type` | VARCHAR2 | 2 |  |  |  |  | `purpose`: `task_dependency_type`; `options`: `finish_to_start`, `start_to_start` |
| `lag_days` | NUMBER |  | 39 | 0 |  |  | `purpose`: `dependency_lag`; `value_unit`: `days` |
| `predecessor_task_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `predecessor_task` |
| `successor_task_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `successor_task` |
