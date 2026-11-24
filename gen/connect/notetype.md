# Notetype (`notetype`)
<!-- id: connect:notetype -->

| Fact | Value |
|---|---|
| Table | `notetype` |
| Record | `notetype` |
| Primary key | `note_type_id` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `date_last_modified`; `time_zone`: `gmt` |
| `description` | VARCHAR2 | 75 |  |  |  |  | `purpose`: `description` |
| `is_inactive` | VARCHAR2 | 1 |  |  |  |  | `purpose`: `is_inactive` |
| `note_type` | VARCHAR2 | 40 |  |  |  |  | `purpose`: `note_type` |
| `note_type_extid` | VARCHAR2 | 255 |  |  |  |  | `purpose`: `note_type_external_id` |
| `note_type_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `note_type_id` |

## Referenced by

| Key | Table | Columns | Columns here |
|---|---|---|---|
| `Notes_user_notetype_fk` | `notes_user` | `note_type_id` | `note_type_id` |
| `System_notes_custom_notetype_fk` | `system_notes_custom` | `note_type_id` | `note_type_id` |
| `System_notes_notetype_fk` | `system_notes` | `note_type_id` | `note_type_id` |
