# Employee_expense_sources (`employee_expense_sources`)
<!-- id: connect:employee_expense_sources -->

| Fact | Value |
|---|---|
| Table | `employee_expense_sources` |
| Primary key | `source_type_id` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `date_created` | TIMESTAMP |  |  |  |  |  | `purpose`: `source_type_created_date` |
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `source_type_last_modified_date` |
| `is_inactive` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `source_type_inactive` |
| `source_description` | VARCHAR2 | 400 |  |  |  |  | `purpose`: `source_type_description` |
| `source_extid` | VARCHAR2 | 255 |  |  |  |  | `purpose`: `source_type_external_id` |
| `source_name` | VARCHAR2 | 100 |  |  |  |  | `purpose`: `source_type_name` |
| `source_type_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `source_type_id`; `same_as`: `imported_employee_expenses.source_id` |

## Referenced by

| Key | Table | Columns | Columns here |
|---|---|---|---|
| `Imported_employee_expenses_employee_expense_sources_fk` | `imported_employee_expenses` | `source_id` | `source_type_id` |
