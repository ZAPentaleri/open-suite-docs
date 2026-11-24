# Billing_rate_cards_versions (`billing_rate_cards_versions`)
<!-- id: connect:billing_rate_cards_versions -->

| Fact | Value |
|---|---|
| Table | `billing_rate_cards_versions` |
| Primary key | `billing_rate_card_id`, `version_0` |
| Tags | `composite_primary_key` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `billing_rate_card_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `billing_rate_card_id` |
| `date_created` | TIMESTAMP |  |  |  |  |  | `purpose`: `date_created` |
| `date_effective` | TIMESTAMP |  |  |  |  |  | `purpose`: `date_effective` |
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `date_last_modified` |
| `last_modified_by_id` | NUMBER |  | 39 | 0 | `employees`.`employee_id` | foreign_key | `purpose`: `last_modified_by_id` |
| `version_0` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `version_0` |

## Foreign keys

| Key | Columns | References table | Referenced columns |
|---|---|---|---|
| `billing_rate_cards_versions_employees_fk` | `last_modified_by_id` | `employees` | `employee_id` |

## Referenced by

| Key | Table | Columns | Columns here |
|---|---|---|---|
| `Billing_rate_cards_prices_billing_rate_cards_versions_fk` | `billing_rate_cards_prices` | `billing_rate_card_id`, `version_0` | `billing_rate_card_id`, `version_0` |
