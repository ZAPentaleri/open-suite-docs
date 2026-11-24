# Fixed_fee_charge_rules (`fixed_fee_charge_rules`)
<!-- id: connect:fixed_fee_charge_rules -->

| Fact | Value |
|---|---|
| Table | `fixed_fee_charge_rules` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `amount` | NUMBER |  | 23 | 5 |  |  | `purpose`: `amount` |
| `billing_item` | NUMBER |  | 39 | 0 |  |  | `purpose`: `billing_item` |
| `charge_rule_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `charge_rule_id` |
| `charge_rule_type` | VARCHAR2 | 32 |  |  |  |  | `purpose`: `charge_rule_type` |
| `charge_stage` | VARCHAR2 | 32 |  |  |  |  | `purpose`: `charge_stage` |
| `date_created` | TIMESTAMP |  |  |  |  |  | `purpose`: `date_created` |
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `date_last_modified` |
| `description` | VARCHAR2 | 4000 |  |  |  |  | `purpose`: `description` |
| `end_date` | TIMESTAMP |  |  |  |  |  | `purpose`: `end_date` |
| `external_id` | VARCHAR2 | 255 |  |  |  |  | `purpose`: `external_id` |
| `fixed_fee_charge_rule_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `fixed_fee_charge_rule_id` |
| `form_template` | NUMBER |  | 39 | 0 |  |  | `purpose`: `form_template` |
| `frequency` | VARCHAR2 | 4000 |  |  |  |  | `purpose`: `frequency` |
| `period` | NUMBER |  | 39 | 0 |  |  | `purpose`: `period` |
| `project_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `project_id` |
| `project_task` | NUMBER |  | 39 | 0 |  |  | `purpose`: `project_task` |
| `rule_name` | VARCHAR2 | 200 |  |  |  |  | `purpose`: `rule_name` |
| `start_date` | TIMESTAMP |  |  |  |  |  | `purpose`: `start_date` |
