# Inventory_cost_template (`inventory_cost_template`)
<!-- id: connect:inventory_cost_template -->

| Fact | Value |
|---|---|
| Table | `inventory_cost_template` |
| Primary key | `inv_cost_template_id` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `create_date` | TIMESTAMP |  |  |  |  |  | `purpose`: `create_date` |
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `date_last_modified` |
| `inv_cost_template_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `inventory_cost_template_id` |
| `inv_cost_template_memo` | VARCHAR2 | 4000 |  |  |  |  | `purpose`: `inventory_cost_template_memo` |
| `inv_cost_template_name` | VARCHAR2 | 100 |  |  |  |  | `purpose`: `inventory_cost_template_name` |
| `isinactive` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `is_inactive` |
| `subsidiary_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `subsidiary_id` |
