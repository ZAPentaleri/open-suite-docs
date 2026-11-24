# Project_revenue_rules (`project_revenue_rules`)
<!-- id: connect:project_revenue_rules -->

| Fact | Value |
|---|---|
| Table | `project_revenue_rules` |
| Primary key | `project_revenue_rule_id` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `billing_rate_card_id` | NUMBER |  | 39 | 0 | `billing_rate_cards`.`billing_rate_card_id` | foreign_key | `purpose`: `billing_rate_card_id` |
| `created_by` | NUMBER |  | 39 | 0 | `employees`.`employee_id` | foreign_key | `purpose`: `created_by` |
| `date_created` | TIMESTAMP |  |  |  |  |  | `purpose`: `date_created` |
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `date_last_modified` |
| `description` | VARCHAR2 | 4000 |  |  |  |  | `purpose`: `description` |
| `fixed_amount_type` | VARCHAR2 | 20 |  |  |  |  | `purpose`: `fixed_amount_type` |
| `fixed_schedule_type` | VARCHAR2 | 10 |  |  |  |  | `purpose`: `fixed_schedule_type` |
| `is_inactive` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `is_inactive` |
| `is_revenue_reconciled` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `is_revenue_reconciled` |
| `last_modified_by` | NUMBER |  | 39 | 0 | `employees`.`employee_id` | foreign_key | `purpose`: `last_modified_by` |
| `project_id` | NUMBER |  | 39 | 0 | `customers`.`customer_id` | foreign_key | `purpose`: `project_id` |
| `project_revenue_rule_extid` | VARCHAR2 | 255 |  |  |  |  | `purpose`: `project_revenue_rule_external_id` |
| `project_revenue_rule_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `project_revenue_rule_id` |
| `rate_multiplier` | NUMBER |  | 12 | 3 |  |  | `purpose`: `project_billing_rate_multiplier` |
| `rate_source_type_id` | VARCHAR2 | 10 |  |  |  |  | `purpose`: `billing_rate_source_type` |
| `recurrence_end_date` | TIMESTAMP |  |  |  |  |  | `purpose`: `billing_recurrence_end_date` |
| `recurrence_frequency` | VARCHAR2 | 4000 |  |  |  |  | `purpose`: `project_billing_recurrence_frequency` |
| `recurrence_period` | NUMBER |  | 39 | 0 |  |  | `purpose`: `project_billing_recurrence_period` |
| `recurrence_start_date` | TIMESTAMP |  |  |  |  |  | `purpose`: `billing_recurrence_start_date` |
| `rounding_type_id` | VARCHAR2 | 10 |  |  |  |  | `purpose`: `billing_rounding_type` |
| `rule_name` | VARCHAR2 | 200 |  |  |  |  | `purpose`: `charge_rule_name` |
| `rule_type_id` | VARCHAR2 | 20 |  |  |  |  | `purpose`: `charge_rule_type_id` |
| `saved_search_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `saved_search_id` |
| `service_item_id` | NUMBER |  | 39 | 0 | `service_items`.`item_id` | foreign_key | `purpose`: `service_item_id` |

## Foreign keys

| Key | Columns | References table | Referenced columns |
|---|---|---|---|
| `project_revenue_rules_billing_rate_cards_fk` | `billing_rate_card_id` | `billing_rate_cards` | `billing_rate_card_id` |
| `project_revenue_rules_customers_fk` | `project_id` | `customers` | `customer_id` |
| `project_revenue_rules_employees_fk_1` | `created_by` | `employees` | `employee_id` |
| `project_revenue_rules_employees_fk_2` | `last_modified_by` | `employees` | `employee_id` |
| `project_revenue_rules_service_items_fk` | `service_item_id` | `service_items` | `item_id` |

## Referenced by

| Key | Table | Columns | Columns here |
|---|---|---|---|
| `Project_revenue_rl_plans_project_revenue_rules_fk` | `project_revenue_rl_plans` | `project_revenue_rule_id` | `project_revenue_rule_id` |
| `Project_revenue_rls_charge_rls_project_revenue_rules_fk` | `project_revenue_rls_charge_rls` | `project_revenue_rule_id` | `project_revenue_rule_id` |
