# Entity_status (`entity_status`)
<!-- id: connect:entity_status -->

| Fact | Value |
|---|---|
| Table | `entity_status` |
| Primary key | `entity_status_id` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `date_last_modified` |
| `description` | VARCHAR2 | 199 |  |  |  |  | `purpose`: `description` |
| `entity_status_extid` | VARCHAR2 | 255 |  |  |  |  | `purpose`: `entity_status_external_id` |
| `entity_status_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `entity_status_id` |
| `isinactive` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `inactive` |
| `name` | VARCHAR2 | 199 |  |  |  |  | `purpose`: `name` |
| `probability` | NUMBER |  | 6 | 2 |  |  | `purpose`: `probability` |
| `read_only` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `read_only` |
| `type_0` | VARCHAR2 | 30 |  |  |  |  | `purpose`: `type` |
