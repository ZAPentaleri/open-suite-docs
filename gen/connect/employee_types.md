# Employee_types (`employee_types`)
<!-- id: connect:employee_types -->

| Fact | Value |
|---|---|
| Table | `employee_types` |
| Record | `employeetype` |
| Primary key | `employee_type_id` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `employee_type_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `employee_type_id` |
| `isinactive` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `inactive` |
| `name` | VARCHAR2 | 99 |  |  |  |  | `purpose`: `name` |
| `parent_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `child_of` |

## Referenced by

| Key | Table | Columns | Columns here |
|---|---|---|---|
| `Employees_employee_types_fk` | `employees` | `employee_type_id` | `employee_type_id` |
