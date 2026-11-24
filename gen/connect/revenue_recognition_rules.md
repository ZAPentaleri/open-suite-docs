# Revenue_recognition_rules (`revenue_recognition_rules`)
<!-- id: connect:revenue_recognition_rules -->

| Fact | Value |
|---|---|
| Table | `revenue_recognition_rules` |
| Primary key | `rev_rec_rule_id` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `amount_source` | VARCHAR2 | 150 |  |  |  |  | `purpose`: `amount_derivation` |
| `date_created` | TIMESTAMP |  |  |  |  |  | `purpose`: `date_created` |
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `date_last_modified` |
| `end_date_change_impact` | VARCHAR2 | 150 |  |  |  |  | `purpose`: `end_date_change_behavior` |
| `end_date_source` | VARCHAR2 | 150 |  |  |  |  | `purpose`: `default_end_date_source` |
| `initial_amount` | VARCHAR2 | 42 |  |  |  |  | `purpose`: `initial_amount` |
| `name` | VARCHAR2 | 99 |  |  |  |  | `purpose`: `name` |
| `period_offset` | NUMBER |  | 39 | 0 |  |  | `purpose`: `plan_start_delay_periods`; `effect`: `end_date_also_postponed` |
| `recognition_period` | NUMBER |  | 39 | 0 |  |  | `purpose`: `plan_period_count` |
| `reforecast_method` | VARCHAR2 | 150 |  |  |  |  | `purpose`: `reforecast_method` |
| `rev_rec_rule_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `revenue_recognition_rule_id` |
| `rule_method` | VARCHAR2 | 150 |  |  |  |  | `purpose`: `rule_method` |
| `start_date_source` | VARCHAR2 | 150 |  |  |  |  | `purpose`: `start_date_source` |
| `start_offset` | NUMBER |  | 39 | 0 |  |  | `purpose`: `recognition_start_delay_periods`; `effect`: `end_date_kept` |
| `term_in_days` | NUMBER |  | 39 | 0 |  |  | `purpose`: `term_in_days` |
| `term_in_months` | NUMBER |  | 39 | 0 |  |  | `purpose`: `plan_term`; `value_unit`: `months` |

## Referenced by

| Key | Table | Columns | Columns here |
|---|---|---|---|
| `Revenue_plans_revenue_recognition_rules_fk` | `revenue_plans` | `rev_rec_rule_id` | `rev_rec_rule_id` |
