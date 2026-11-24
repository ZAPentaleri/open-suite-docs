# Caseorigin (`caseorigin`)
<!-- id: connect:caseorigin -->

| Fact | Value |
|---|---|
| Table | `caseorigin` |
| Primary key | `case_origin_id` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `case_origin_extid` | VARCHAR2 | 255 |  |  |  |  | `purpose`: `case_origin_external_id` |
| `case_origin_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `case_origin_id` |
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `date_last_modified` |
| `description` | VARCHAR2 | 200 |  |  |  |  | `purpose`: `description` |
| `is_inactive` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `inactive` |
| `name` | VARCHAR2 | 50 |  |  |  |  | `purpose`: `name` |
