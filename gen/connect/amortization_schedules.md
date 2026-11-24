# Amortization_schedules (`amortization_schedules`)
<!-- id: connect:amortization_schedules -->

| Fact | Value |
|---|---|
| Table | `amortization_schedules` |
| Record | `amortizationschedule` |
| Primary key | `schedule_id` |
| Domains | Expense_amortization, Invoice_with_amortization |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `amount` | NUMBER |  | 20 | 2 |  |  | `purpose`: `amount` |
| `initial_amount` | VARCHAR2 | 42 |  |  |  |  | `purpose`: `initial_amount` |
| `is_template` | VARCHAR2 | 1 |  |  |  |  | `purpose`: `is_template` |
| `name` | VARCHAR2 | 40 |  |  |  |  | `purpose`: `schedule_name` |
| `period_offset` | NUMBER |  | 10 | 0 |  |  | `purpose`: `period_offset` |
| `residual` | VARCHAR2 | 42 |  |  |  |  | `purpose`: `residual` |
| `schedule_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `amortization_schedule_id` |
| `schedule_method` | VARCHAR2 | 56 |  |  |  |  | `purpose`: `method` |
| `schedule_number` | VARCHAR2 | 99 |  |  |  |  | `purpose`: `amortization_schedule_number` |
| `schedule_type` | VARCHAR2 | 8 |  |  |  |  | `purpose`: `type` |
| `start_offset` | NUMBER |  | 39 | 0 |  |  | `purpose`: `start_offset` |
| `term_source` | VARCHAR2 | 16 |  |  |  |  | `purpose`: `term_source` |

## Referenced by

| Key | Table | Columns | Columns here |
|---|---|---|---|
| `Amortization_sched_lines_amortization_schedules_fk` | `amortization_sched_lines` | `schedule_id` | `schedule_id` |
