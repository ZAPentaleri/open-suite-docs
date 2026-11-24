# Subsidiary_location_map (`subsidiary_location_map`)
<!-- id: connect:subsidiary_location_map -->

| Fact | Value |
|---|---|
| Table | `subsidiary_location_map` |
| Primary key | `subsidiary_id`, `location_id` |
| Tags | `composite_primary_key` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `location_id` | NUMBER |  | 39 | 0 | `locations`.`location_id` | primary_key | `purpose`: `location_id` |
| `subsidiary_id` | NUMBER |  | 39 | 0 | `subsidiaries`.`subsidiary_id` | primary_key | `purpose`: `subsidiary_id` |

## Foreign keys

| Key | Columns | References table | Referenced columns |
|---|---|---|---|
| `subsidiary_location_map_locations_fk` | `location_id` | `locations` | `location_id` |
| `subsidiary_location_map_subsidiaries_fk` | `subsidiary_id` | `subsidiaries` | `subsidiary_id` |
