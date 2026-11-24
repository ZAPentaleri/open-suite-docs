# Project_cost_categories (`project_cost_categories`)
<!-- id: connect:project_cost_categories -->

| Fact | Value |
|---|---|
| Table | `project_cost_categories` |
| Primary key | `project_cost_category_id` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `category_name` | VARCHAR2 | 4000 |  |  |  |  | `purpose`: `category_name` |
| `category_type` | VARCHAR2 | 10 |  |  |  |  | `purpose`: `cost_category_type`; `options`: `labor`, `expense`, `supplier`, `other` |
| `expense_category_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `expense_subcategory`; `caveat`: `null_unless_category_type_expense` |
| `full_name` | VARCHAR2 | 4000 |  |  |  |  | `purpose`: `category_full_name`; `format`: `category_type_prefix_then_name` |
| `is_other_category` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `is_other_category`; `value_examples`: `Labor:Other`, `Expense:Other` |
| `is_selected` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `category_used_for_budgeting_and_reporting` |
| `project_cost_category_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `id_of_the_category` |
| `service_item_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `labor_subcategory`; `caveat`: `null_unless_category_type_labor` |
| `supplier_category_id` | VARCHAR2 | 32 |  |  |  |  | `purpose`: `supplier_subcategory`; `caveat`: `null_unless_category_type_supplier` |
| `use_subcategories` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `subcategories_selected`; `applies_to`: `top_level_categories` |
