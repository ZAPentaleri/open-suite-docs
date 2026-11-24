# Contactrole (`contactrole`)
<!-- id: connect:contactrole -->

| Fact | Value |
|---|---|
| Table | `contactrole` |
| Record | `contactrole` |
| Primary key | `contactrole_id` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `contactrole_extid` | VARCHAR2 | 255 |  |  |  |  | `purpose`: `contact_role_external_id` |
| `contactrole_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `contact_role_id` |
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `date_last_modified` |
| `is_inactive` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `whether_is_inactive` |
| `name` | VARCHAR2 | 31 |  |  |  |  | `purpose`: `name` |
