# Case_stage_changes (`case_stage_changes`)
<!-- id: connect:case_stage_changes -->

| Fact | Value |
|---|---|
| Table | `case_stage_changes` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `case_id` | NUMBER |  | 39 | 0 | `support_incidents`.`case_id` | foreign_key | `purpose`: `case_id` |
| `casestage` | VARCHAR2 | 12 |  |  |  |  | `purpose`: `case_stage` |
| `start_date` | TIMESTAMP |  |  |  |  |  | `purpose`: `start_date` |

## Foreign keys

| Key | Columns | References table | Referenced columns |
|---|---|---|---|
| `case_stage_changes_support_incidents_fk` | `case_id` | `support_incidents` | `case_id` |
