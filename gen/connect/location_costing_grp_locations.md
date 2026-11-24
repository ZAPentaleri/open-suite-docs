# Location_costing_grp_locations (`location_costing_grp_locations`)
<!-- id: connect:location_costing_grp_locations -->

| Fact | Value |
|---|---|
| Table | `location_costing_grp_locations` |
| Primary key | `location_costing_group_id`, `location_id` |
| Tags | `composite_primary_key` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `location_costing_group_id` | NUMBER |  | 39 | 0 | `location_costing_groups`.`location_costing_group_id` | primary_key |  |
| `location_id` | NUMBER |  | 39 | 0 | `locations`.`location_id` | primary_key |  |

## Foreign keys

| Key | Columns | References table | Referenced columns |
|---|---|---|---|
| `location_costing_grp_locations_location_costing_groups_fk` | `location_costing_group_id` | `location_costing_groups` | `location_costing_group_id` |
| `location_costing_grp_locations_locations_fk` | `location_id` | `locations` | `location_id` |
