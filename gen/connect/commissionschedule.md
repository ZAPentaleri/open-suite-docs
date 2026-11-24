# Commissionschedule (`commissionschedule`)
<!-- id: connect:commissionschedule -->

| Fact | Value |
|---|---|
| Table | `commissionschedule` |
| Primary key | `schedule_id` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `author` | NUMBER |  | 39 | 0 |  |  | `purpose`: `author` |
| `category_0` | VARCHAR2 | 10 |  |  |  |  | `purpose`: `category` |
| `date_created` | TIMESTAMP |  |  |  |  |  | `purpose`: `date_created` |
| `description` | VARCHAR2 | 199 |  |  |  |  | `purpose`: `description` |
| `is_inactive` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `is_inactive` |
| `manager_` | VARCHAR2 | 1 |  |  |  |  | `purpose`: `manager` |
| `name` | VARCHAR2 | 50 |  |  |  |  | `purpose`: `name` |
| `period_type` | VARCHAR2 | 10 |  |  |  |  | `purpose`: `period_type` |
| `schedule_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `commission_schedule_id` |
| `target_amount` | NUMBER |  | 39 | 0 |  |  | `purpose`: `target_amount` |
| `type_id` | VARCHAR2 | 10 |  |  |  |  | `purpose`: `type_id` |
