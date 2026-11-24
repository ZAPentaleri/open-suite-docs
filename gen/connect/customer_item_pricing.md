# Customer_item_pricing (`customer_item_pricing`)
<!-- id: connect:customer_item_pricing -->

| Fact | Value |
|---|---|
| Table | `customer_item_pricing` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `currency_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `currency_id` |
| `customer_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `customer_id` |
| `item_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `item_id` |
| `item_unit_price` | NUMBER |  | 30 | 15 |  |  | `purpose`: `item_unit_price` |
| `price_type_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `price_type_id` |
