# Item_price_history (`item_price_history`)
<!-- id: connect:item_price_history -->

| Fact | Value |
|---|---|
| Table | `item_price_history` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `currency_id` | NUMBER |  | 39 | 0 | `currencies`.`currency_id` | foreign_key |  |
| `item_id` | NUMBER |  | 39 | 0 | `items`.`item_id` | foreign_key |  |
| `item_quantity_id` | NUMBER |  | 39 | 0 |  |  |  |
| `min_count` | NUMBER |  | 15 | 5 |  |  |  |
| `price_type_id` | NUMBER |  | 39 | 0 | `price_types`.`price_type_id` | foreign_key |  |
| `unit_price` | NUMBER |  | 30 | 15 |  |  |  |
| `version_0` | NUMBER |  | 39 | 0 |  |  |  |

## Foreign keys

| Key | Columns | References table | Referenced columns |
|---|---|---|---|
| `item_price_history_currencies_fk` | `currency_id` | `currencies` | `currency_id` |
| `item_price_history_items_fk` | `item_id` | `items` | `item_id` |
| `item_price_history_price_types_fk` | `price_type_id` | `price_types` | `price_type_id` |
