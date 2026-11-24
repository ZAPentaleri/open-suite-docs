# Tasks (`tasks`)
<!-- id: connect:tasks -->

| Fact | Value |
|---|---|
| Table | `tasks` |
| Record | `task` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `assigned_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `assigned_to` |
| `date_created` | TIMESTAMP |  |  |  |  |  | `purpose`: `date_created` |
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `date_last_modified`; `time_zone`: `gmt` |
| `due_date` | TIMESTAMP |  |  |  |  |  | `purpose`: `due_date` |
| `estimated_hours` | NUMBER |  | 12 | 5 |  |  | `purpose`: `initial_time_budget` |
| `estimated_hours_override` | NUMBER |  | 12 | 5 |  |  | `purpose`: `estimated_work` |
| `isinactive` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `inactive` |
| `message` | VARCHAR2 | 4000 |  |  |  |  | `purpose`: `message` |
| `name` | VARCHAR2 | 256 |  |  |  |  | `purpose`: `name` |
| `owner_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `owner_id` |
| `priority` | VARCHAR2 | 32 |  |  |  |  | `purpose`: `priority` |
| `start_date` | TIMESTAMP |  |  |  |  |  | `purpose`: `start_date` |
| `status` | VARCHAR2 | 64 |  |  |  |  | `purpose`: `status` |
| `task_extid` | VARCHAR2 | 255 |  |  |  |  | `purpose`: `task_external_id` |
| `task_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `task_id` |
| `taskorder` | NUMBER |  | 39 | 0 |  |  | `purpose`: `task_order` |
