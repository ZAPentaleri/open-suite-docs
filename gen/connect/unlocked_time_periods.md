# Unlocked_time_periods (`unlocked_time_periods`)
<!-- id: connect:unlocked_time_periods -->

| Fact | Value |
|---|---|
| Table | `unlocked_time_periods` |
| Record | `unlockedtimeperiod` |
| Primary key | `unlocked_time_period_id` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `date_created` | TIMESTAMP |  |  |  |  |  | `purpose`: `record_created_date` |
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `record_last_modified_date` |
| `date_period_end` | TIMESTAMP |  |  |  |  |  | `purpose`: `period_end_date` |
| `date_period_start` | TIMESTAMP |  |  |  |  |  | `purpose`: `period_start_date` |
| `date_valid_until` | TIMESTAMP |  |  |  |  |  | `purpose`: `unlock_valid_until` |
| `employee_id` | NUMBER |  | 39 | 0 | `employees`.`employee_id` | foreign_key | `purpose`: `employee_for_unlocked_periods` |
| `is_inactive` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `unlocked_period_inactive` |
| `last_modified_by_id` | NUMBER |  | 39 | 0 | `employees`.`employee_id` | foreign_key | `purpose`: `last_modified_by` |
| `unlocked_time_period_extid` | VARCHAR2 | 255 |  |  |  |  | `purpose`: `unlocked_time_period_external_id` |
| `unlocked_time_period_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `unlocked_time_period_id` |

## Foreign keys

| Key | Columns | References table | Referenced columns |
|---|---|---|---|
| `unlocked_time_periods_employees_fk` | `employee_id` | `employees` | `employee_id` |
| `unlocked_time_periods_employees_fk_2` | `last_modified_by_id` | `employees` | `employee_id` |
