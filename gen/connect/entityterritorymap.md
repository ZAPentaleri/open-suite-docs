# Entityterritorymap (`entityterritorymap`)
<!-- id: connect:entityterritorymap -->

| Fact | Value |
|---|---|
| Table | `entityterritorymap` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `entity_id` | NUMBER |  | 39 | 0 | `entity`.`entity_id` | foreign_key | `purpose`: `entity_id` |
| `territory_id` | NUMBER |  | 39 | 0 | `territory`.`territory_id` | foreign_key | `purpose`: `territory_id` |

## Foreign keys

| Key | Columns | References table | Referenced columns |
|---|---|---|---|
| `entityterritorymap_entity_fk` | `entity_id` | `entity` | `entity_id` |
| `entityterritorymap_territory_fk` | `territory_id` | `territory` | `territory_id` |
