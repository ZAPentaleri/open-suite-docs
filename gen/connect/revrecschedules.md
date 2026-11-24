# Revrecschedules (`revrecschedules`)
<!-- id: connect:revrecschedules -->

| Fact | Value |
|---|---|
| Table | `revrecschedules` |
| Record | `revrecschedule` |
| Primary key | `schedule_id` |
| Domains | General_accounting, Multibooks, Revenue_recognition |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `amount` | NUMBER |  | 20 | 2 |  |  | `purpose`: `amount` |
| `initial_amount` | VARCHAR2 | 42 |  |  |  |  | `purpose`: `initial_amount` |
| `is_template` | VARCHAR2 | 1 |  |  |  |  | `purpose`: `is_rev_rec_template` |
| `name` | VARCHAR2 | 40 |  |  |  |  | `purpose`: `name` |
| `period_offset` | NUMBER |  | 10 | 0 |  |  | `purpose`: `period_offset` |
| `schedule_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `revenue_recognition_schedule_id` |
| `schedule_method` | VARCHAR2 | 56 |  |  |  |  | `purpose`: `method` |
| `schedule_number` | VARCHAR2 | 99 |  |  |  |  | `purpose`: `schedule_number` |
| `schedule_type` | VARCHAR2 | 8 |  |  |  |  | `purpose`: `type` |
| `start_offset` | NUMBER |  | 39 | 0 |  |  | `purpose`: `start_offset` |
| `term_source` | VARCHAR2 | 49 |  |  |  |  | `purpose`: `term_source` |

## Referenced by

| Key | Table | Columns | Columns here |
|---|---|---|---|
| `Customers_revrecschedules_fk` | `customers` | `rev_rec_forecast_template` | `schedule_id` |
| `Revrecschedulelines_revrecschedules_fk` | `revrecschedulelines` | `schedule_id` | `schedule_id` |
| `Transaction_line_book_map_revrecschedules_fk` | `transaction_line_book_map` | `schedule_id` | `schedule_id` |
