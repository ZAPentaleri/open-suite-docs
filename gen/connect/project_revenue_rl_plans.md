# Project_revenue_rl_plans (`project_revenue_rl_plans`)
<!-- id: connect:project_revenue_rl_plans -->

| Fact | Value |
|---|---|
| Table | `project_revenue_rl_plans` |
| Primary key | `project_revenue_rl_plan_id` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `date_recognized` | TIMESTAMP |  |  |  |  |  | `purpose`: `date_recognized` |
| `fixed_amount` | NUMBER |  | 20 | 2 |  |  | `purpose`: `fixed_amount` |
| `percent_from_total` | NUMBER |  | 12 | 4 |  |  | `purpose`: `percent_from_total` |
| `project_revenue_rl_plan_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `project_revenue_rule_plan_id` |
| `project_revenue_rule_id` | NUMBER |  | 39 | 0 | `project_revenue_rules`.`project_revenue_rule_id` | foreign_key | `purpose`: `project_revenue_rule_id` |
| `project_task_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `project_task_id` |

## Foreign keys

| Key | Columns | References table | Referenced columns |
|---|---|---|---|
| `project_revenue_rl_plans_project_revenue_rules_fk` | `project_revenue_rule_id` | `project_revenue_rules` | `project_revenue_rule_id` |
