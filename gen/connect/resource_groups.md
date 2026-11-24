# Resource_groups (`resource_groups`)
<!-- id: connect:resource_groups -->

| Fact | Value |
|---|---|
| Table | `resource_groups` |
| Record | `resourcegroup` |
| Primary key | `resource_group_id` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `date_created` | TIMESTAMP |  |  |  |  |  | `purpose`: `date_created` |
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `date_last_modified` |
| `description` | VARCHAR2 | 2000 |  |  |  |  | `purpose`: `description` |
| `is_inactive` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `inactive` |
| `resource_group_extid` | VARCHAR2 | 255 |  |  |  |  | `purpose`: `resource_group_id` |
| `resource_group_id` | NUMBER |  | 39 | 0 | `entity`.`entity_id` | primary_key | `purpose`: `internal_resource_group_id` |
| `resource_group_name` | VARCHAR2 | 83 |  |  |  |  | `purpose`: `resource_group_name` |

## Foreign keys

| Key | Columns | References table | Referenced columns |
|---|---|---|---|
| `resource_groups_entity_fk` | `resource_group_id` | `entity` | `entity_id` |

## Referenced by

| Key | Table | Columns | Columns here |
|---|---|---|---|
| `Resource_group_entity_map_resource_groups_fk` | `resource_group_entity_map` | `group_id` | `resource_group_id` |
