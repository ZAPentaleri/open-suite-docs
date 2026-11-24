# Planassignmap (`planassignmap`)
<!-- id: connect:planassignmap -->

| Fact | Value |
|---|---|
| Table | `planassignmap` |
| Primary key | `plan_id`, `employee_id`, `start_date` |
| Tags | `composite_primary_key` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `date_created` | TIMESTAMP |  |  |  |  |  | `purpose`: `date_created`; `time_zone`: `gmt` |
| `employee_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `employee_id` |
| `end_date` | TIMESTAMP |  |  |  |  |  | `purpose`: `end_date` |
| `plan_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `plan_id` |
| `start_date` | TIMESTAMP |  |  |  |  | primary_key | `purpose`: `start_date` |
