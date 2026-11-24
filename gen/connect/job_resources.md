# Job_resources (`job_resources`)
<!-- id: connect:job_resources -->

| Fact | Value |
|---|---|
| Table | `job_resources` |
| Primary key | `job_id`, `resource_id`, `resource_role_id` |
| Tags | `composite_primary_key` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `job_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `project_record_id` |
| `resource_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `project_assigned_employee` |
| `resource_role_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `project_assigned_employee_role` |
