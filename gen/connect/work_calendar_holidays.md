# Work_calendar_holidays (`work_calendar_holidays`)
<!-- id: connect:work_calendar_holidays -->

| Fact | Value |
|---|---|
| Table | `work_calendar_holidays` |
| Primary key | `work_calendar_id`, `date_holiday` |
| Tags | `composite_primary_key` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `date_holiday` | TIMESTAMP |  |  |  |  | primary_key | `purpose`: `date_of_holiday` |
| `description` | VARCHAR2 | 100 |  |  |  |  | `purpose`: `description` |
| `work_calendar_id` | NUMBER |  | 39 | 0 | `work_calendars`.`work_calendar_id` | primary_key | `purpose`: `work_calendar_id` |

## Foreign keys

| Key | Columns | References table | Referenced columns |
|---|---|---|---|
| `work_calendar_holidays_work_calendars_fk` | `work_calendar_id` | `work_calendars` | `work_calendar_id` |
