# Customer_currencies (`customer_currencies`)
<!-- id: connect:customer_currencies -->

| Fact | Value |
|---|---|
| Table | `customer_currencies` |
| Primary key | `customer_id`, `currency_id` |
| Tags | `composite_primary_key` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `currency_id` | NUMBER |  | 39 | 0 | `currencies`.`currency_id` | primary_key | `purpose`: `currency_id` |
| `customer_id` | NUMBER |  | 39 | 0 | `customers`.`customer_id` | primary_key | `purpose`: `customer_id` |
| `deposit_balance_foreign` | NUMBER |  | 39 | 0 |  |  | `purpose`: `foreign_currency_deposit_balance` |
| `openbalance_foreign` | NUMBER |  | 39 | 0 |  |  | `purpose`: `foreign_currency_open_balance` |
| `unbilled_orders_foreign` | NUMBER |  | 39 | 0 |  |  | `purpose`: `foreign_currency_unbilled_orders` |

## Foreign keys

| Key | Columns | References table | Referenced columns |
|---|---|---|---|
| `customer_currencies_currencies_fk` | `currency_id` | `currencies` | `currency_id` |
| `customer_currencies_customers_fk` | `customer_id` | `customers` | `customer_id` |
