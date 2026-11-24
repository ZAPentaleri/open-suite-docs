# Subsidiary_department_map (`subsidiary_department_map`)
<!-- id: connect:subsidiary_department_map -->

| Fact | Value |
|---|---|
| Table | `subsidiary_department_map` |
| Primary key | `subsidiary_id`, `department_id` |
| Tags | `composite_primary_key` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `department_id` | NUMBER |  | 39 | 0 | `departments`.`department_id` | primary_key | `purpose`: `department_id` |
| `subsidiary_id` | NUMBER |  | 39 | 0 | `subsidiaries`.`subsidiary_id` | primary_key | `purpose`: `subsidiary_id` |

## Foreign keys

| Key | Columns | References table | Referenced columns |
|---|---|---|---|
| `subsidiary_department_map_departments_fk` | `department_id` | `departments` | `department_id` |
| `subsidiary_department_map_subsidiaries_fk` | `subsidiary_id` | `subsidiaries` | `subsidiary_id` |
