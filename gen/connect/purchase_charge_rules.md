# Purchase_charge_rules (`purchase_charge_rules`)
<!-- id: connect:purchase_charge_rules -->

| Fact | Value |
|---|---|
| Table | `purchase_charge_rules` |
| Primary key | `charge_rule_id` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `cap` | NUMBER |  | 12 | 3 |  |  | `purpose`: `maximum_charge` |
| `charge_rule_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `purchase_charge_rule_id` |
| `charge_rule_type_id` | VARCHAR2 | 32 |  |  |  |  | `purpose`: `rule_type` |
| `charge_stage_id` | VARCHAR2 | 32 |  |  |  |  | `purpose`: `default_charge_stage` |
| `date_created` | TIMESTAMP |  |  |  |  |  | `purpose`: `date_created` |
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `date_last_modified` |
| `description` | VARCHAR2 | 4000 |  |  |  |  | `purpose`: `description` |
| `mark_up_item_id` | NUMBER |  | 39 | 0 | `items`.`item_id` | foreign_key | `purpose`: `mark_up_item` |
| `project_id` | NUMBER |  | 39 | 0 | `customers`.`customer_id` | foreign_key | `purpose`: `project_id` |
| `purchase_charge_rule_extid` | VARCHAR2 | 255 |  |  |  |  | `purpose`: `purchase_charge_rule_id` |
| `purchase_saved_search_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `transaction_filter_id` |
| `rate_multiplier` | NUMBER |  | 12 | 3 |  |  | `purpose`: `rate_multiplier` |
| `rule_name` | VARCHAR2 | 200 |  |  |  |  | `purpose`: `rule_name` |
| `rule_order` | NUMBER |  | 39 | 0 |  |  | `purpose`: `rule_order` |

## Foreign keys

| Key | Columns | References table | Referenced columns |
|---|---|---|---|
| `purchase_charge_rules_customers_fk` | `project_id` | `customers` | `customer_id` |
| `purchase_charge_rules_items_fk` | `mark_up_item_id` | `items` | `item_id` |
