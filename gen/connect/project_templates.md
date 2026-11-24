# Project_templates (`project_templates`)
<!-- id: connect:project_templates -->

| Fact | Value |
|---|---|
| Table | `project_templates` |
| Record | `projecttemplate` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `billing_rate_card_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `billing_rate_card_id` |
| `billing_schedule_type` | VARCHAR2 | 60 |  |  |  |  | `purpose`: `billing_schedule_type` |
| `date_scheduled_end` | TIMESTAMP |  |  |  |  |  | `purpose`: `scheduled_end_date` |
| `date_template_start` | TIMESTAMP |  |  |  |  |  | `purpose`: `date_template_start` |
| `is_inactive` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `project_is_inactive` |
| `is_limit_time_to_assignees` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `limit_time_and_expense_to_assignees` |
| `is_source_item_from_brc` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `service_item_sourced_from_rate_card` |
| `project_expense_type_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `project_expense_type_id` |
| `project_manager_id` | NUMBER |  | 39 | 0 | `employees`.`employee_id` | foreign_key | `purpose`: `project_manager_id` |
| `scheduling_method_id` | VARCHAR2 | 15 |  |  |  |  | `purpose`: `project_scheduling_method` |
| `template_id` | NUMBER |  | 39 | 0 | `billing_rate_cards`.`billing_rate_card_id` | foreign_key | `purpose`: `template_id` |
| `template_name` | VARCHAR2 | 83 |  |  |  |  | `purpose`: `template_name` |
| `use_budget_from_allocations` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `use_budget_from_allocations` |
| `use_calculated_billing_budget` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `use_calculated_billing_budget` |
| `use_calculated_cost_budget` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `use_calculated_cost_budget` |

## Foreign keys

| Key | Columns | References table | Referenced columns |
|---|---|---|---|
| `project_templates_billing_rate_cards_fk` | `template_id` | `billing_rate_cards` | `billing_rate_card_id` |
| `project_templates_employees_fk` | `project_manager_id` | `employees` | `employee_id` |
