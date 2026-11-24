# Resource_group_entity_map (`resource_group_entity_map`)
<!-- id: connect:resource_group_entity_map -->

| Fact | Value |
|---|---|
| Table | `resource_group_entity_map` |
| Primary key | `group_id`, `entity_id` |
| Tags | `composite_primary_key` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `entity_id` | NUMBER |  | 39 | 0 | `entity`.`entity_id` | primary_key | `purpose`: `employee_id` |
| `group_id` | NUMBER |  | 39 | 0 | `resource_groups`.`resource_group_id` | primary_key | `purpose`: `internal_resource_group_id` |

## Foreign keys

| Key | Columns | References table | Referenced columns |
|---|---|---|---|
| `resource_group_entity_map_entity_fk` | `entity_id` | `entity` | `entity_id` |
| `resource_group_entity_map_resource_groups_fk` | `group_id` | `resource_groups` | `resource_group_id` |
