# Timesheet (`timesheet`)
<!-- id: connect:timesheet -->

| Fact | Value |
|---|---|
| Table | `timesheet` |
| Record | `timesheet` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `date_last_modified` |
| `employee_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `employee_id` |
| `end_date` | TIMESTAMP |  |  |  |  |  | `purpose`: `end_date` |
| `start_date` | TIMESTAMP |  |  |  |  |  | `purpose`: `start_date` |
| `status_code` | VARCHAR2 | 200 |  |  |  |  | `purpose`: `status_code` |
| `timesheet_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `timesheet_id` |
