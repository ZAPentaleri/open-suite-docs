# Entity_status_history (`entity_status_history`)
<!-- id: connect:entity_status_history -->

| Fact | Value |
|---|---|
| Table | `entity_status_history` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `comment_0` | VARCHAR2 | 199 |  |  |  |  | `purpose`: `comment` |
| `date_0` | TIMESTAMP |  |  |  |  |  | `purpose`: `date` |
| `entity_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `entity_id` |
| `entity_status_new_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `entity_status_new_id` |
| `entity_status_old_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `entity_status_old_id` |
