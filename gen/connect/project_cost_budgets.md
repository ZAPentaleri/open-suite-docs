# Project_cost_budgets (`project_cost_budgets`)
<!-- id: connect:project_cost_budgets -->

| Fact | Value |
|---|---|
| Table | `project_cost_budgets` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `amount` | NUMBER |  | 20 | 2 |  |  | `purpose`: `monthly_category_budget` |
| `is_calculated` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `budget_is_calculated` |
| `is_calculated_from_allocations` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `cost_budget_from_allocations` |
| `month_end` | TIMESTAMP |  |  |  |  |  | `purpose`: `budget_month_last_day` |
| `month_start` | TIMESTAMP |  |  |  |  |  | `purpose`: `budget_month_first_day` |
| `project_cost_category_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `budget_cost_category` |
| `project_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `project_id` |
