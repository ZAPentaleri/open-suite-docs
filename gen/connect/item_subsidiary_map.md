# Item_subsidiary_map (`item_subsidiary_map`)
<!-- id: connect:item_subsidiary_map -->

| Fact | Value |
|---|---|
| Table | `item_subsidiary_map` |
| Primary key | `subsidiary_id`, `item_id` |
| Tags | `composite_primary_key` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `item_id` | NUMBER |  | 39 | 0 | `items`.`item_id` | primary_key | `purpose`: `item_id` |
| `subsidiary_id` | NUMBER |  | 39 | 0 | `subsidiaries`.`subsidiary_id` | primary_key | `purpose`: `subsidiary_id` |

## Foreign keys

| Key | Columns | References table | Referenced columns |
|---|---|---|---|
| `item_subsidiary_map_items_fk` | `item_id` | `items` | `item_id` |
| `item_subsidiary_map_subsidiaries_fk` | `subsidiary_id` | `subsidiaries` | `subsidiary_id` |
