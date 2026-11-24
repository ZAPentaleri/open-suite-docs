# Job_resource_role (`job_resource_role`)
<!-- id: connect:job_resource_role -->

| Fact | Value |
|---|---|
| Table | `job_resource_role` |
| Primary key | `job_resource_role_id` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `description` | VARCHAR2 | 999 |  |  |  |  | `purpose`: `job_role_description` |
| `has_own_time_approval` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `has_own_time_approval` |
| `has_replace_assignments` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `has_replace_assignments` |
| `is_project_time_approver` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `is_project_time_approver` |
| `isinactive` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `is_role_inactive` |
| `job_resource_role_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `job_resource_role_id` |
| `name` | VARCHAR2 | 50 |  |  |  |  | `purpose`: `job_role_name` |
| `send_mail_for_ratoff_conflict` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `email_on_allocation_time_off_conflict` |

## Referenced by

| Key | Table | Columns | Columns here |
|---|---|---|---|
| `Employees_job_resource_role_fk` | `employees` | `default_job_resource_role_id` | `job_resource_role_id` |
