# Location_costing_groups (`location_costing_groups`)
<!-- id: connect:location_costing_groups -->

| Fact | Value |
|---|---|
| Table | `location_costing_groups` |
| Primary key | `location_costing_group_id` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `currency_id` | NUMBER |  | 39 | 0 | `currencies`.`currency_id` | foreign_key |  |
| `location_costing_group_id` | NUMBER |  | 38 | 0 |  | primary_key |  |
| `location_costing_group_name` | VARCHAR2 | 100 |  |  |  |  |  |
| `memo` | VARCHAR2 | 999 |  |  |  |  |  |

## Foreign keys

| Key | Columns | References table | Referenced columns |
|---|---|---|---|
| `location_costing_groups_currencies_fk` | `currency_id` | `currencies` | `currency_id` |

## Referenced by

| Key | Table | Columns | Columns here |
|---|---|---|---|
| `Location_costing_grp_locations_location_costing_groups_fk` | `location_costing_grp_locations` | `location_costing_group_id` | `location_costing_group_id` |
