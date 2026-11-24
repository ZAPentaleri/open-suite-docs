# Topic (`topic`)
<!-- id: connect:topic -->

| Fact | Value |
|---|---|
| Table | `topic` |
| Record | `topic` |
| Primary key | `topic_id` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `description` | VARCHAR2 | 4000 |  |  |  |  | `purpose`: `description` |
| `is_inactive` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `is_inactive` |
| `long_description` | VARCHAR2 | 4000 |  |  |  |  | `purpose`: `long_description` |
| `name` | VARCHAR2 | 99 |  |  |  |  | `purpose`: `name` |
| `parent_topic_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `parent_topic_id` |
| `topic_extid` | VARCHAR2 | 255 |  |  |  |  | `purpose`: `topic_external_id` |
| `topic_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `topic_id` |
