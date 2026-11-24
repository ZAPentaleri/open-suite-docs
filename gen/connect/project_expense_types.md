# Project_expense_types (`project_expense_types`)
<!-- id: connect:project_expense_types -->

| Fact | Value |
|---|---|
| Table | `project_expense_types` |
| Record | `projectexpensetype` |
| Primary key | `project_expense_type_id` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `account_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `account_id` |
| `description` | VARCHAR2 | 2000 |  |  |  |  | `purpose`: `description` |
| `is_inactive` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `inactive` |
| `name` | VARCHAR2 | 200 |  |  |  |  | `purpose`: `name` |
| `project_expense_type_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `project_expense_type_id` |

## Referenced by

| Key | Table | Columns | Columns here |
|---|---|---|---|
| `Customers_project_expense_types_fk` | `customers` | `project_expense_type_id` | `project_expense_type_id` |
