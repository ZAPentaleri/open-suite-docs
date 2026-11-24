# Item_supply_plans (`item_supply_plans`)
<!-- id: connect:item_supply_plans -->

| Fact | Value |
|---|---|
| Table | `item_supply_plans` |
| Record | `itemsupplyplan` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `create_date` | TIMESTAMP |  |  |  |  |  | `purpose`: `date_created`; `time_zone`: `gmt` |
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `date_last_modified` |
| `item_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `item_id` |
| `item_supply_plan_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `item_supply_plan_id` |
| `last_modified_date` | TIMESTAMP |  |  |  |  |  | `purpose`: `date_last_modified`; `time_zone`: `gmt` |
| `location_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `location_id` |
| `memo` | VARCHAR2 | 4000 |  |  |  |  | `purpose`: `memo` |
| `subsidiary_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `subsidiary_id` |
