# Customer_group_pricing (`customer_group_pricing`)
<!-- id: connect:customer_group_pricing -->

| Fact | Value |
|---|---|
| Table | `customer_group_pricing` |
| Primary key | `customer_id`, `pricing_group_id` |
| Tags | `composite_primary_key` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `customer_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `customer_id` |
| `price_type_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `price_type_id` |
| `pricing_group_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `pricing_group_id` |
