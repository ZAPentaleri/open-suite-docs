# Inventory_cost_template_items (`inventory_cost_template_items`)
<!-- id: connect:inventory_cost_template_items -->

| Fact | Value |
|---|---|
| Table | `inventory_cost_template_items` |
| Primary key | `inv_cost_template_id`, `inv_cost_template_line_order` |
| Tags | `composite_primary_key` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `inv_cost_template_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `inventory_cost_template_id` |
| `inv_cost_template_line_order` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `cost_template_line_order` |
| `item_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `item_id` |
