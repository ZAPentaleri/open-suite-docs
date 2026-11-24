# Item_quantity (`item_quantity`)
<!-- id: connect:item_quantity -->

| Fact | Value |
|---|---|
| Table | `item_quantity` |
| Primary key | `item_id`, `item_quantity_id` |
| Tags | `composite_primary_key` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `item_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `item_id` |
| `item_quantity_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `item_quantity_id` |
| `max_count` | NUMBER |  | 15 | 5 |  |  | `purpose`: `maximum_number_of_items` |
| `min_count` | NUMBER |  | 15 | 5 |  |  | `purpose`: `minimum_number_of_items` |
