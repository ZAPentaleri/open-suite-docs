# Item-Item Collection Relationship (`itemcollectionitemmap`)
<!-- id: record:itemcollectionitemmap -->

| Fact | Value |
|---|---|
| Internal ID | `itemcollectionitemmap` |

## Record facts

- `script_types`: `client`, `server`
- `script_functions`: `read`, `create`, `delete`, `search`
- `requires_feature`: `Inventory` [`real_name`]
- `ui_accessible`: `false`
- `purpose`: `links_item_collection_to_its_items`
- `configured_by`: `enable_features_items_and_inventory_subtab` [`inventory`]
- `value_examples`: `delete_map_record_id_1`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `externalid` | text | External ID | no |  |  |  |
| `internalid` | integer | Internal ID | no |  |  |  |
| `item` | select | Item | yes |  |  |  |
| `itemcollection` | select | Item Collection | yes |  |  |  |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `externalid` | select | External ID |  |
| `externalidstring` | text | External ID (Text) |  |
| `internalid` | select | Internal ID |  |
| `item` | select | Item |  |
| `itemcollection` | select | Item Collection |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `externalid` | select | External ID |  |
| `internalid` | select | Internal ID |  |
| `item` | select | Item |  |
| `itemcollection` | select | Item Collection |  |
