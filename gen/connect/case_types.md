# Case_types (`case_types`)
<!-- id: connect:case_types -->

| Fact | Value |
|---|---|
| Table | `case_types` |
| Primary key | `case_type_id` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `case_type_extid` | VARCHAR2 | 255 |  |  |  |  | `purpose`: `case_type_external_id` |
| `case_type_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `case_type_id` |
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `date_last_modified` |
| `description` | VARCHAR2 | 200 |  |  |  |  | `purpose`: `description` |
| `isinactive` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `case_type_inactive` |
| `name` | VARCHAR2 | 50 |  |  |  |  | `purpose`: `name` |

## Referenced by

| Key | Table | Columns | Columns here |
|---|---|---|---|
| `Support_incidents_case_types_fk` | `support_incidents` | `case_type_id` | `case_type_id` |
