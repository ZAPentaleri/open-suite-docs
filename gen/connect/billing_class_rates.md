# Billing_class_rates (`billing_class_rates`)
<!-- id: connect:billing_class_rates -->

| Fact | Value |
|---|---|
| Table | `billing_class_rates` |
| Primary key | `billing_class_id`, `currency_id`, `billing_rate_card_id` |
| Tags | `composite_primary_key` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `billing_class_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `billing_class_id` |
| `billing_rate_card_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `billing_rate_card_id` |
| `currency_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `currency_id` |
| `unit_price` | NUMBER |  | 20 | 2 |  |  | `purpose`: `unit_price` |
