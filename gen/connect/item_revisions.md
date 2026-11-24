# Item_revisions (`item_revisions`)
<!-- id: connect:item_revisions -->

| Fact | Value |
|---|---|
| Table | `item_revisions` |
| Record | `itemrevision` |
| Primary key | `item_revision_id` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `create_date` | TIMESTAMP |  |  |  |  |  | `purpose`: `create_date` |
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `date_last_modified` |
| `effective_date` | TIMESTAMP |  |  |  |  |  | `purpose`: `effective_date` |
| `externalid` | VARCHAR2 | 255 |  |  |  |  | `purpose`: `external_id` |
| `isinactive` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `is_inactive` |
| `item_revision_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `item_revision_id` |
| `item_revision_item` | NUMBER |  | 39 | 0 |  |  | `purpose`: `item_revision_item` |
| `item_revision_name` | VARCHAR2 | 100 |  |  |  |  | `purpose`: `item_revision_name` |
| `memo` | VARCHAR2 | 255 |  |  |  |  | `purpose`: `memo` |
| `obsolete_date` | TIMESTAMP |  |  |  |  |  | `purpose`: `obsolete_date` |
