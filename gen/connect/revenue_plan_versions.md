# Revenue_plan_versions (`revenue_plan_versions`)
<!-- id: connect:revenue_plan_versions -->

| Fact | Value |
|---|---|
| Table | `revenue_plan_versions` |
| Primary key | `plan_version_id` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `created_by_id` | NUMBER |  | 39 | 0 | `employees`.`employee_id` | foreign_key | `purpose`: `user_whose_change_created_version` |
| `date_created` | TIMESTAMP |  |  |  |  |  | `purpose`: `edit_timestamp_that_created_version` |
| `plan_id` | NUMBER |  | 39 | 0 | `revenue_plans`.`plan_id` | foreign_key | `purpose`: `version_revenue_plan` |
| `plan_version` | NUMBER |  | 39 | 0 |  |  | `purpose`: `previous_plan_version` |
| `plan_version_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `plan_version_id` |

## Foreign keys

| Key | Columns | References table | Referenced columns |
|---|---|---|---|
| `revenue_plan_versions_employees_fk` | `created_by_id` | `employees` | `employee_id` |
| `revenue_plan_versions_revenue_plans_fk` | `plan_id` | `revenue_plans` | `plan_id` |

## Referenced by

| Key | Table | Columns | Columns here |
|---|---|---|---|
| `Revenue_plan_version_lines_revenue_plan_versions_fk` | `revenue_plan_version_lines` | `plan_version_id` | `plan_version_id` |
