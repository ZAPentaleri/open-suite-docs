# Item_collection_item_map (`item_collection_item_map`)
<!-- id: connect:item_collection_item_map -->

| Fact | Value |
|---|---|
| Table | `item_collection_item_map` |
| Record | `itemcollectionitemmap` |
| Primary key | `item_collection_item_map_id` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `item_collection_id` | NUMBER |  | 39 | 0 | `item_collections`.`item_collection_id` | foreign_key | `purpose`: `item_collection`; `allowed_values_listed_in`: `item_collection_table` |
| `item_collection_item_map_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `item_collection_assignment_id`; `constraint`: `unique` |
| `item_id` | NUMBER |  | 39 | 0 | `items`.`item_id` | foreign_key | `purpose`: `collection_member_item` |
| `item_map_extid` | VARCHAR2 | 255 |  |  |  |  | `purpose`: `item_collection_assignment_external_id` |

## Foreign keys

| Key | Columns | References table | Referenced columns |
|---|---|---|---|
| `item_collection_item_map_item_collections_fk` | `item_collection_id` | `item_collections` | `item_collection_id` |
| `item_collection_item_map_items_fk` | `item_id` | `items` | `item_id` |
