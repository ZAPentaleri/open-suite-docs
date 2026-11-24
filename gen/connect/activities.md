# Activities (`activities`)
<!-- id: connect:activities -->

| Fact | Value |
|---|---|
| Table | `activities` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `access_level` | VARCHAR2 | 10 |  |  |  |  | `purpose`: `access_level` |
| `activity_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `activity_id` |
| `assigned_to_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `assigned_to_id` |
| `case_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `case_id` |
| `contact_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `contact_id` |
| `date_0` | TIMESTAMP |  |  |  |  |  | `purpose`: `date`; `time_zone`: `gmt` |
| `date_completed` | TIMESTAMP |  |  |  |  |  | `purpose`: `date_completed`; `time_zone`: `gmt` |
| `date_created` | TIMESTAMP |  |  |  |  |  | `purpose`: `activity_creation_date`; `time_zone`: `gmt`; `not_supported_on`: `note_activities` |
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `date_last_modified`; `time_zone`: `gmt` |
| `entity_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `entity_id` |
| `memo` | VARCHAR2 | 4000 |  |  |  |  | `purpose`: `activity_memo`; `caveat`: `long_memos_truncated`; `workaround`: `join_message_table_for_full_message` |
| `opportunity_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `opportunity_id` |
| `owner_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `owner_id` |
| `status` | VARCHAR2 | 12 |  |  |  |  | `purpose`: `status` |
| `title` | VARCHAR2 | 457 |  |  |  |  | `purpose`: `title` |
| `type_0` | VARCHAR2 | 8 |  |  |  |  | `purpose`: `activity_type` |
