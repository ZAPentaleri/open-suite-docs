# Billing_rate_cards (`billing_rate_cards`)
<!-- id: connect:billing_rate_cards -->

| Fact | Value |
|---|---|
| Table | `billing_rate_cards` |
| Record | `billingratecard` |
| Primary key | `billing_rate_card_id` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `billing_rate_card_extid` | VARCHAR2 | 255 |  |  |  |  | `purpose`: `billing_rate_card_external_id` |
| `billing_rate_card_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `billing_rate_card_id` |
| `customer_id` | NUMBER |  | 39 | 0 | `customers`.`customer_id` | foreign_key | `purpose`: `customer_id` |
| `date_created` | TIMESTAMP |  |  |  |  |  | `purpose`: `date_created` |
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `date_last_modified` |
| `is_inactive` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `is_inactive` |
| `name` | VARCHAR2 | 100 |  |  |  |  | `purpose`: `name` |

## Foreign keys

| Key | Columns | References table | Referenced columns |
|---|---|---|---|
| `billing_rate_cards_customers_fk` | `customer_id` | `customers` | `customer_id` |

## Referenced by

| Key | Table | Columns | Columns here |
|---|---|---|---|
| `Billing_rate_cards_prices_billing_rate_cards_fk` | `billing_rate_cards_prices` | `billing_rate_card_id` | `billing_rate_card_id` |
| `Customers_billing_rate_cards_fk` | `customers` | `billing_rate_card_id` | `billing_rate_card_id` |
| `Project_revenue_rules_billing_rate_cards_fk` | `project_revenue_rules` | `billing_rate_card_id` | `billing_rate_card_id` |
| `Project_templates_billing_rate_cards_fk` | `project_templates` | `template_id` | `billing_rate_card_id` |
| `Time_based_charge_rules_billing_rate_cards_fk` | `time_based_charge_rules` | `billing_rate_card_id` | `billing_rate_card_id` |
