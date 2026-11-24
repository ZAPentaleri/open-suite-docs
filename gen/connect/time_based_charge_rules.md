# Time_based_charge_rules (`time_based_charge_rules`)
<!-- id: connect:time_based_charge_rules -->

| Fact | Value |
|---|---|
| Table | `time_based_charge_rules` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `adjust_time_to_fit_under_cap` | VARCHAR2 | 1 |  |  |  |  | `purpose`: `trim_time_to_fit_cap` |
| `billing_rate_card_id` | NUMBER |  | 39 | 0 | `billing_rate_cards`.`billing_rate_card_id` | foreign_key | `purpose`: `billing_rate_card_id` |
| `cap_hours` | NUMBER |  | 12 | 5 |  |  | `purpose`: `cap_hours` |
| `cap_money` | NUMBER |  | 20 | 2 |  |  | `purpose`: `cap_money` |
| `cap_type` | VARCHAR2 | 5 |  |  |  |  | `purpose`: `cap_type` |
| `charge_rule_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `charge_rule_id` |
| `charge_rule_type` | VARCHAR2 | 32 |  |  |  |  | `purpose`: `charge_rule_type` |
| `charge_stage` | VARCHAR2 | 32 |  |  |  |  | `purpose`: `charge_stage` |
| `date_created` | TIMESTAMP |  |  |  |  |  | `purpose`: `date_created` |
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `date_last_modified` |
| `description` | VARCHAR2 | 4000 |  |  |  |  | `purpose`: `description` |
| `dont_bill_entry_exceeding_cap` | VARCHAR2 | 1 |  |  |  |  | `purpose`: `skip_billing_entries_over_cap` |
| `end_date` | TIMESTAMP |  |  |  |  |  | `purpose`: `end_date` |
| `external_id` | VARCHAR2 | 255 |  |  |  |  | `purpose`: `external_id` |
| `form_template` | NUMBER |  | 39 | 0 |  |  | `purpose`: `form_template` |
| `project_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `project_id` |
| `rate_multiplier` | NUMBER |  | 12 | 3 |  |  | `purpose`: `rate_multiplier` |
| `rate_source` | VARCHAR2 | 10 |  |  |  |  | `purpose`: `rate_source` |
| `rounding` | VARCHAR2 | 10 |  |  |  |  | `purpose`: `rounding` |
| `rule_name` | VARCHAR2 | 200 |  |  |  |  | `purpose`: `rule_name` |
| `rule_order` | NUMBER |  | 39 | 0 |  |  | `purpose`: `rule_order` |
| `sale_unit_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `sale_unit_id` |
| `saved_search` | NUMBER |  | 39 | 0 |  |  | `purpose`: `saved_search` |
| `start_date` | TIMESTAMP |  |  |  |  |  | `purpose`: `start_date` |
| `time_based_charge_rule_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `time_based_charge_rule_id` |
| `units_type_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `units_type_id` |

## Foreign keys

| Key | Columns | References table | Referenced columns |
|---|---|---|---|
| `time_based_charge_rules_billing_rate_cards_fk` | `billing_rate_card_id` | `billing_rate_cards` | `billing_rate_card_id` |
