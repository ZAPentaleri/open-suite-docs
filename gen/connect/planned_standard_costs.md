# Planned_standard_costs (`planned_standard_costs`)
<!-- id: connect:planned_standard_costs -->

| Fact | Value |
|---|---|
| Table | `planned_standard_costs` |
| Primary key | `planned_standard_cost_id` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `date_created` | TIMESTAMP |  |  |  |  |  | `purpose`: `date_created` |
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `date_last_modified` |
| `item_id` | NUMBER |  | 39 | 0 | `items`.`item_id` | foreign_key | `purpose`: `item_id` |
| `location_id` | NUMBER |  | 39 | 0 | `locations`.`location_id` | foreign_key | `purpose`: `location_id` |
| `memo` | VARCHAR2 | 4000 |  |  |  |  | `purpose`: `memo` |
| `planned_standard_cost_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `planned_standard_cost_id` |
| `standard_cost_version_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `standard_cost_version_id` |

## Foreign keys

| Key | Columns | References table | Referenced columns |
|---|---|---|---|
| `planned_standard_costs_items_fk` | `item_id` | `items` | `item_id` |
| `planned_standard_costs_locations_fk` | `location_id` | `locations` | `location_id` |

## Referenced by

| Key | Table | Columns | Columns here |
|---|---|---|---|
| `Standard_cost_components_planned_standard_costs_fk` | `standard_cost_components` | `planned_standard_cost_id` | `planned_standard_cost_id` |
