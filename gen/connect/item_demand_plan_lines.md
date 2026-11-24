# Item_demand_plan_lines (`item_demand_plan_lines`)
<!-- id: connect:item_demand_plan_lines -->

| Fact | Value |
|---|---|
| Table | `item_demand_plan_lines` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `demand_date` | TIMESTAMP |  |  |  |  |  | `purpose`: `date_demanded` |
| `item_demand_plan_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `item_demand_plan_id` |
| `quantity` | NUMBER |  | 39 | 0 |  |  | `purpose`: `quantity_demanded` |
