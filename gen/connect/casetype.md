# Casetype (`casetype`)
<!-- id: connect:casetype -->

| Fact | Value |
|---|---|
| Table | `casetype` |
| Primary key | `case_type` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `case_type` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `case_type` |
| `case_type_extid` | VARCHAR2 | 255 |  |  |  |  | `purpose`: `case_type_external_id` |
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `date_last_modified` |
| `description` | VARCHAR2 | 200 |  |  |  |  | `purpose`: `description` |
| `is_inactive` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `whether_is_inactive` |
| `name` | VARCHAR2 | 50 |  |  |  |  | `purpose`: `name` |
