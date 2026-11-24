# Item_demand_plans (`item_demand_plans`)
<!-- id: connect:item_demand_plans -->

| Fact | Value |
|---|---|
| Table | `item_demand_plans` |
| Record | `itemdemandplan` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `alt_demand_source_item_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `alternate_source_item_id` |
| `create_date` | TIMESTAMP |  |  |  |  |  | `purpose`: `date_created`; `time_zone`: `gmt` |
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `date_last_modified` |
| `historical_analysis_duration` | NUMBER |  | 39 | 0 |  |  | `purpose`: `past_periods_for_demand_projection` |
| `item_demand_plan_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `item_demand_plan_id` |
| `item_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `item_id` |
| `last_modified_date` | TIMESTAMP |  |  |  |  |  | `purpose`: `date_last_modified`; `time_zone`: `gmt` |
| `location_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `location_id` |
| `memo` | VARCHAR2 | 4000 |  |  |  |  | `purpose`: `memo` |
| `projection_duration` | NUMBER |  | 39 | 0 |  |  | `purpose`: `demand_projection_period` |
| `projection_interval` | VARCHAR2 | 10 |  |  |  |  | `purpose`: `demand_projection_period_count` |
| `projection_method` | VARCHAR2 | 50 |  |  |  |  | `purpose`: `demand_projection_method` |
| `projection_start_date` | TIMESTAMP |  |  |  |  |  | `purpose`: `demand_projection_start_date` |
| `subsidiary_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `subsidiary_id` |
