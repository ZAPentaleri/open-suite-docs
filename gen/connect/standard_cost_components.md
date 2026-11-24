# Standard_cost_components (`standard_cost_components`)
<!-- id: connect:standard_cost_components -->

| Fact | Value |
|---|---|
| Table | `standard_cost_components` |
| Primary key | `planned_standard_cost_id`, `line_id` |
| Tags | `composite_primary_key` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `component_id` | NUMBER |  | 39 | 0 | `items`.`item_id` | foreign_key | `purpose`: `component_id` |
| `cost_category` | VARCHAR2 | 50 |  |  |  |  | `purpose`: `cost_category` |
| `line_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `line_id` |
| `planned_standard_cost_id` | NUMBER |  | 39 | 0 | `planned_standard_costs`.`planned_standard_cost_id` | primary_key | `purpose`: `planned_standard_cost_id` |
| `quantity` | NUMBER |  | 39 | 0 |  |  | `purpose`: `quantity` |
| `standard_cost` | NUMBER |  | 39 | 0 |  |  | `purpose`: `standard_cost` |

## Foreign keys

| Key | Columns | References table | Referenced columns |
|---|---|---|---|
| `standard_cost_components_items_fk` | `component_id` | `items` | `item_id` |
| `standard_cost_components_planned_standard_costs_fk` | `planned_standard_cost_id` | `planned_standard_costs` | `planned_standard_cost_id` |
