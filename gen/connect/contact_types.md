# Contact_types (`contact_types`)
<!-- id: connect:contact_types -->

| Fact | Value |
|---|---|
| Table | `contact_types` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `contact_id` | NUMBER |  | 39 | 0 | `entity`.`entity_id` | foreign_key | `purpose`: `contact_id` |
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `date_last_modified` |
| `isinactive` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `item_is_inactive` |
| `name` | VARCHAR2 | 31 |  |  |  |  | `purpose`: `name` |
| `owner_id` | NUMBER |  | 39 | 0 | `entity`.`entity_id` | foreign_key | `purpose`: `owner_id` |
| `parent_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `child_of` |
| `shared_0` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `contacts_are_shared` |

## Foreign keys

| Key | Columns | References table | Referenced columns |
|---|---|---|---|
| `contact_types_entity_fk` | `contact_id` | `entity` | `entity_id` |
| `contact_types_entity_fk_2` | `owner_id` | `entity` | `entity_id` |
