# Project_time_approval_types (`project_time_approval_types`)
<!-- id: connect:project_time_approval_types -->

| Fact | Value |
|---|---|
| Table | `project_time_approval_types` |
| Primary key | `project_time_approval_type_id` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `description` | VARCHAR2 | 4000 |  |  |  |  | `purpose`: `description` |
| `project_time_approval_type_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `time_approval_type_id` |

## Referenced by

| Key | Table | Columns | Columns here |
|---|---|---|---|
| `Customers_project_time_approval_types_fk` | `customers` | `time_approval_type_id` | `project_time_approval_type_id` |
