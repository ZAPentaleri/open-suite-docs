# Item_supply_plan_attributes (`item_supply_plan_attributes`)
<!-- id: connect:item_supply_plan_attributes -->

| Fact | Value |
|---|---|
| Table | `item_supply_plan_attributes` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `demand_source` | VARCHAR2 | 50 |  |  |  |  | `purpose`: `demand_source` |
| `end_date` | TIMESTAMP |  |  |  |  |  | `purpose`: `end_date` |
| `item_supply_plan_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `item_supply_plan_id` |
| `lead_time` | NUMBER |  | 39 | 0 |  |  | `purpose`: `lead_time` |
| `quantity_on_hand` | NUMBER |  | 21 | 11 |  |  | `purpose`: `quantity_on_hand` |
| `safety_stock` | NUMBER |  | 18 | 8 |  |  | `purpose`: `safety_stock` |
| `start_date` | TIMESTAMP |  |  |  |  |  | `purpose`: `start_date` |
