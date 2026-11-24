# Expense_based_charge_rules (`expense_based_charge_rules`)
<!-- id: connect:expense_based_charge_rules -->

| Fact | Value |
|---|---|
| Table | `expense_based_charge_rules` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `charge_rule_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `charge_rule_id` |
| `charge_rule_type` | VARCHAR2 | 32 |  |  |  |  | `purpose`: `charge_rule_type` |
| `charge_stage` | VARCHAR2 | 32 |  |  |  |  | `purpose`: `charge_stage` |
| `date_created` | TIMESTAMP |  |  |  |  |  | `purpose`: `date_created` |
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `date_last_modified` |
| `description` | VARCHAR2 | 4000 |  |  |  |  | `purpose`: `description` |
| `expense_amount_multiplier` | NUMBER |  | 12 | 3 |  |  | `purpose`: `expense_amount_multiplier` |
| `expense_based_charge_rule_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `expense_based_charge_rule_id` |
| `expense_saved_search` | NUMBER |  | 39 | 0 |  |  | `purpose`: `expense_saved_search` |
| `external_id` | VARCHAR2 | 255 |  |  |  |  | `purpose`: `external_id` |
| `form_template` | NUMBER |  | 39 | 0 |  |  | `purpose`: `form_template` |
| `project_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `project_id` |
| `rule_name` | VARCHAR2 | 200 |  |  |  |  | `purpose`: `rule_name` |
| `rule_order` | NUMBER |  | 39 | 0 |  |  | `purpose`: `rule_order` |
