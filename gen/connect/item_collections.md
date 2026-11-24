# Item_collections (`item_collections`)
<!-- id: connect:item_collections -->

| Fact | Value |
|---|---|
| Table | `item_collections` |
| Record | `itemcollection` |
| Primary key | `item_collection_id` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `description` | VARCHAR2 | 2000 |  |  |  |  | `purpose`: `item_collection_description` |
| `is_inactive` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `item_collection_inactive` |
| `item_collection_extid` | VARCHAR2 | 255 |  |  |  |  | `purpose`: `item_collection_external_id` |
| `item_collection_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `item_collection_id` |
| `name` | VARCHAR2 | 200 |  |  |  |  | `purpose`: `item_collection_name` |

## Referenced by

| Key | Table | Columns | Columns here |
|---|---|---|---|
| `Item_collection_item_map_item_collections_fk` | `item_collection_item_map` | `item_collection_id` | `item_collection_id` |
