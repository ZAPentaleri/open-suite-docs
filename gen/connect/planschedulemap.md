# Planschedulemap (`planschedulemap`)
<!-- id: connect:planschedulemap -->

| Fact | Value |
|---|---|
| Table | `planschedulemap` |
| Primary key | `plan_id`, `schedule_id` |
| Tags | `composite_primary_key` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `date_created` | TIMESTAMP |  |  |  |  |  | `purpose`: `date_created`; `time_zone`: `gmt` |
| `plan_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `plan_id` |
| `schedule_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `schedule_id` |
