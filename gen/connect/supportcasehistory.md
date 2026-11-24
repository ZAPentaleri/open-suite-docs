# Supportcasehistory (`supportcasehistory`)
<!-- id: connect:supportcasehistory -->

| Fact | Value |
|---|---|
| Table | `supportcasehistory` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `case_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `support_case_id` |
| `date_closed` | TIMESTAMP |  |  |  |  |  | `purpose`: `date_closed` |
| `date_created` | TIMESTAMP |  |  |  |  |  | `purpose`: `date_created`; `time_zone`: `gmt` |
| `employee_id` | NUMBER |  | 39 | 0 | `entity`.`entity_id` | foreign_key | `purpose`: `employee_id` |

## Foreign keys

| Key | Columns | References table | Referenced columns |
|---|---|---|---|
| `supportcasehistory_entity_fk` | `employee_id` | `entity` | `entity_id` |
