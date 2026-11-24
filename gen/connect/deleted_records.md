# Deleted_records (`deleted_records`)
<!-- id: connect:deleted_records -->

| Fact | Value |
|---|---|
| Table | `deleted_records` |
| Record | `deletedrecord` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `custom_record_type` | VARCHAR2 | 4000 |  |  |  |  | `purpose`: `custom_record_type` |
| `date_deleted` | TIMESTAMP |  |  |  |  |  | `purpose`: `date_deleted` |
| `entity_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `entity_id` |
| `entity_name` | VARCHAR2 | 999 |  |  |  |  | `purpose`: `entity_name` |
| `is_custom_list` | VARCHAR2 | 1 |  |  |  |  | `purpose`: `whether_is_custom_list` |
| `name` | VARCHAR2 | 999 |  |  |  |  | `purpose`: `name` |
| `record_base_type` | VARCHAR2 | 30 |  |  |  |  | `purpose`: `record_base_type` |
| `record_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `record_id` |
| `record_type_name` | VARCHAR2 | 30 |  |  |  |  | `purpose`: `record_type_name` |
