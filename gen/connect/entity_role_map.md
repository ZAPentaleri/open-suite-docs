# Entity_role_map (`entity_role_map`)
<!-- id: connect:entity_role_map -->

| Fact | Value |
|---|---|
| Table | `entity_role_map` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `contact_id` | NUMBER |  | 39 | 0 | `contacts`.`contact_id` | foreign_key | `purpose`: `contact_id` |
| `entity_id` | NUMBER |  | 39 | 0 | `entity`.`entity_id` | foreign_key | `purpose`: `entity_id` |
| `role_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `role_id` |

## Foreign keys

| Key | Columns | References table | Referenced columns |
|---|---|---|---|
| `entity_role_map_contacts_fk` | `contact_id` | `contacts` | `contact_id` |
| `entity_role_map_entity_fk` | `entity_id` | `entity` | `entity_id` |
