# Memorized_trans (`memorized_trans`)
<!-- id: connect:memorized_trans -->

| Fact | Value |
|---|---|
| Table | `memorized_trans` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `action` | VARCHAR2 | 24 |  |  |  |  | `purpose`: `action` |
| `automatic_entry` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `automatic_entry` |
| `date_next_posting` | TIMESTAMP |  |  |  |  |  | `purpose`: `date_of_next_posting` |
| `date_subsequent_posting` | TIMESTAMP |  |  |  |  |  | `purpose`: `date_of_subsequent_posting` |
| `days_in_advance` | NUMBER |  | 39 | 0 |  |  | `purpose`: `days_in_advance` |
| `employee_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `employee_id` |
| `frequency` | VARCHAR2 | 128 |  |  |  |  | `purpose`: `frequency` |
| `ingroup` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `in_group` |
| `is_inactive` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `is_inactive` |
| `is_indefinite` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `is_indefinite` |
| `isdeferred` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `is_deferred` |
| `isgroup` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `is_group` |
| `memorized_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `memorized_id` |
| `next_occurance` | TIMESTAMP |  |  |  |  |  | `purpose`: `next_occurrence` |
| `number_remaining` | NUMBER |  | 39 | 0 |  |  | `purpose`: `number_remaining` |
| `repeat_every` | NUMBER |  | 39 | 0 |  |  | `purpose`: `repeat_every` |
| `subsequent_occurance` | TIMESTAMP |  |  |  |  |  | `purpose`: `subsequent_occurrence` |
| `time_period` | VARCHAR2 | 24 |  |  |  |  | `purpose`: `time_period` |
| `transaction_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `transaction_id` |
| `update_addresses` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `update_addresses` |
| `update_prices` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `update_prices` |
