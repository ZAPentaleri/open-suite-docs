# Vendor_currencies (`vendor_currencies`)
<!-- id: connect:vendor_currencies -->

| Fact | Value |
|---|---|
| Table | `vendor_currencies` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `currency_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `currency_id` |
| `in_transit_balance_foreign` | NUMBER |  | 20 | 2 |  |  | `purpose`: `in_transit_foreign_currency_balance` |
| `openbalance_foreign` | NUMBER |  | 20 | 2 |  |  | `purpose`: `foreign_open_balance` |
| `unbilled_orders_foreign` | NUMBER |  | 20 | 2 |  |  | `purpose`: `foreign_currency_unbilled_orders` |
| `vendor_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `vendor_id` |
| `prepayment_balance_foreign` | NUMBER |  | 20 | 2 |  |  |  |
