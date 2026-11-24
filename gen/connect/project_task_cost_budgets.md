# Project_task_cost_budgets (`project_task_cost_budgets`)
<!-- id: connect:project_task_cost_budgets -->

| Fact | Value |
|---|---|
| Table | `project_task_cost_budgets` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `amount` | NUMBER |  | 20 | 2 |  |  | `purpose`: `monthly_category_budget` |
| `is_calculated` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `task_cost_budget_calculated` |
| `month_end` | TIMESTAMP |  |  |  |  |  | `purpose`: `budget_month_last_day` |
| `month_start` | TIMESTAMP |  |  |  |  |  | `purpose`: `budget_month_first_day` |
| `project_cost_category_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `budget_cost_category` |
| `project_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `project_id` |
| `project_task_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `budget_project_task` |
