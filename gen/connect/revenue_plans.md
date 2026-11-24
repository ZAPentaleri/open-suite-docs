# Revenue_plans (`revenue_plans`)
<!-- id: connect:revenue_plans -->

| Fact | Value |
|---|---|
| Table | `revenue_plans` |
| Record | `revenueplan` |
| Primary key | `plan_id` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `accounting_period_id` | NUMBER |  | 39 | 0 | `accounting_periods`.`accounting_period_id` | foreign_key | `purpose`: `accounting_period_id` |
| `comments` | VARCHAR2 | 500 |  |  |  |  | `purpose`: `comments` |
| `created_from` | VARCHAR2 | 4000 |  |  |  |  | `purpose`: `source_revenue_element` |
| `creation_triggered_by` | VARCHAR2 | 4000 |  |  |  |  | `purpose`: `record_triggering_plan_creation` |
| `date_created` | TIMESTAMP |  |  |  |  |  | `purpose`: `date_created` |
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `date_last_modified` |
| `end_date` | TIMESTAMP |  |  |  |  |  | `purpose`: `end_date` |
| `is_hold_rev_rec` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `hold_revenue_recognition_plan` |
| `plan_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `plan_id` |
| `plan_number` | VARCHAR2 | 99 |  |  |  |  | `purpose`: `plan_number` |
| `reforecast_method` | VARCHAR2 | 150 |  |  |  |  | `purpose`: `reforecast_method` |
| `rev_rec_rule_id` | NUMBER |  | 39 | 0 | `revenue_recognition_rules`.`rev_rec_rule_id` | foreign_key | `purpose`: `revenue_recognition_rule_id` |
| `revenue_element_id` | NUMBER |  | 39 | 0 | `revenue_elements`.`revenue_element_id` | foreign_key | `purpose`: `revenue_elements_id` |
| `revenue_plan_status` | VARCHAR2 | 128 |  |  |  |  | `purpose`: `revenue_plan_status` |
| `revenue_plan_type` | VARCHAR2 | 128 |  |  |  |  | `purpose`: `revenue_plan_type` |
| `start_date` | TIMESTAMP |  |  |  |  |  | `purpose`: `start_date` |

## Foreign keys

| Key | Columns | References table | Referenced columns |
|---|---|---|---|
| `revenue_plans_accounting_periods_fk` | `accounting_period_id` | `accounting_periods` | `accounting_period_id` |
| `revenue_plans_revenue_elements_fk` | `revenue_element_id` | `revenue_elements` | `revenue_element_id` |
| `revenue_plans_revenue_recognition_rules_fk` | `rev_rec_rule_id` | `revenue_recognition_rules` | `rev_rec_rule_id` |

## Referenced by

| Key | Table | Columns | Columns here |
|---|---|---|---|
| `Revenue_plan_lines_revenue_plans_fk` | `revenue_plan_lines` | `plan_id` | `plan_id` |
| `Revenue_plan_versions_revenue_plans_fk` | `revenue_plan_versions` | `plan_id` | `plan_id` |
