# Case_origins (`case_origins`)
<!-- id: connect:case_origins -->

| Fact | Value |
|---|---|
| Table | `case_origins` |
| Primary key | `case_origin_id` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `case_origin_extid` | VARCHAR2 | 255 |  |  |  |  | `purpose`: `case_origin_external_id` |
| `case_origin_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `case_origin_id` |
| `date_last_modified_` | TIMESTAMP |  |  |  |  |  | `purpose`: `date_last_modified` |
| `description` | VARCHAR2 | 200 |  |  |  |  | `purpose`: `description` |
| `isinactive` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `whether_is_inactive` |
| `name` | VARCHAR2 | 50 |  |  |  |  | `purpose`: `name` |

## Referenced by

| Key | Table | Columns | Columns here |
|---|---|---|---|
| `Support_incidents_case_origins_fk` | `support_incidents` | `case_origin_id` | `case_origin_id` |
