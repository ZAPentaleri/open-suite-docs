# Project_revenue_rls_charge_rls (`project_revenue_rls_charge_rls`)
<!-- id: connect:project_revenue_rls_charge_rls -->

| Fact | Value |
|---|---|
| Table | `project_revenue_rls_charge_rls` |
| Primary key | `project_revenue_rule_id`, `charge_rule_id` |
| Tags | `composite_primary_key` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `actual_amount` | NUMBER |  | 23 | 5 |  |  | `purpose`: `actual_amount` |
| `charge_rule_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `charge_rule_id` |
| `date_amount_modified` | TIMESTAMP |  |  |  |  |  | `purpose`: `date_amount_modified` |
| `forecast_amount` | NUMBER |  | 23 | 5 |  |  | `purpose`: `forecast_amount` |
| `project_revenue_rule_id` | NUMBER |  | 39 | 0 | `project_revenue_rules`.`project_revenue_rule_id` | primary_key | `purpose`: `project_revenue_rule_id` |

## Foreign keys

| Key | Columns | References table | Referenced columns |
|---|---|---|---|
| `project_revenue_rls_charge_rls_project_revenue_rules_fk` | `project_revenue_rule_id` | `project_revenue_rules` | `project_revenue_rule_id` |
