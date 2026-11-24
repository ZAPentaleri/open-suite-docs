# Mfg_cost_template_items (`mfg_cost_template_items`)
<!-- id: connect:mfg_cost_template_items -->

| Fact | Value |
|---|---|
| Table | `mfg_cost_template_items` |
| Primary key | `mfg_cost_template_id`, `mfg_cost_template_line_order` |
| Tags | `composite_primary_key` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `item_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `cost_template_item` |
| `mfg_cost_template_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `mfg_cost_template_id` |
| `mfg_cost_template_line_order` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `cost_template_line_order` |
