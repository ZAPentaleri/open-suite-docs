# Work_calendars (`work_calendars`)
<!-- id: connect:work_calendars -->

| Fact | Value |
|---|---|
| Table | `work_calendars` |
| Primary key | `work_calendar_id` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `isdefault` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `is_default` |
| `isfriday` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `is_friday` |
| `isinactive` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `is_inactive` |
| `ismonday` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `is_monday` |
| `issaturday` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `is_saturday` |
| `issunday` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `is_sunday` |
| `isthursday` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `is_thursday` |
| `istuesday` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `is_saturday` |
| `iswednesday` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `is_wednesday` |
| `message` | VARCHAR2 | 4000 |  |  |  |  | `purpose`: `message` |
| `name` | VARCHAR2 | 100 |  |  |  |  | `purpose`: `name` |
| `start_hour` | TIMESTAMP |  |  |  |  |  | `purpose`: `start_hour` |
| `work_calendar_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `work_calendar_id` |
| `work_hours_per_day` | NUMBER |  | 39 | 0 |  |  | `purpose`: `work_hours_per_day` |

## Referenced by

| Key | Table | Columns | Columns here |
|---|---|---|---|
| `Employees_work_calendars_fk` | `employees` | `work_calendar_id` | `work_calendar_id` |
| `Work_calendar_holidays_work_calendars_fk` | `work_calendar_holidays` | `work_calendar_id` | `work_calendar_id` |
