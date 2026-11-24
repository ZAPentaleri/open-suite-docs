# Item Collection (`itemcollection`)
<!-- id: record:itemcollection -->

| Fact | Value |
|---|---|
| Internal ID | `itemcollection` |
| Category | list |
| Client scripts | yes |
| Server scripts | yes |
| Tags | `no_copy`, `no_transform` |

## Record facts

- `script_types`: `client`, `server`
- `script_functions`: `read`, `create`, `update`, `delete`, `search`
- `requires_feature`: `Inventory` [`real_name`]
- `not_scriptable`: `items_sublist`, `customer_segments_sublist`
- `items_added_via` (refs `record:itemcollectionitemmap`)
- `purpose`: `group_catalog_items_for_personalized_catalog_views`
- `ui_location`: `lists_accounting_item_collections`
- `configured_by`: `enable_features_items_and_inventory_subtab` [`inventory`]

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `description` | textarea | Description | no |  |  | `purpose`: `collection_description`; `ui_effect`: `text_enhance_button_may_appear` |
| `isinactive` | checkbox | Inactive | no |  |  | `purpose`: `inactivate_collection` |
| `name` | text | Name | yes |  |  | `purpose`: `collection_name` |
| `savedsearch` | select | Saved Search | no |  |  | `purpose`: `item_saved_search_for_members`; `allowed_values_listed_in`: `item_saved_searches` |
| `externalid` | text | External ID | no |  |  |  |
| `id` | integer | Internal ID | no |  |  |  |
| `internalid` | integer | Internal ID | no |  |  |  |

## Sublist `customersegments` (Customer Segment)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `customersegment` | select | Customer Segment | yes |  |  |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `description` | text | Description |  |
| `externalid` | select | External ID |  |
| `externalidstring` | text | External ID (Text) |  |
| `internalid` | select | Internal ID |  |
| `isdynamic` | checkbox | Dynamic |  |
| `isinactive` | checkbox | Inactive |  |
| `name` | text | Name |  |
| `savedsearch` | select | Saved Search |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `description` | text | Description |  |
| `externalid` | select | External ID |  |
| `internalid` | select | Internal ID |  |
| `isinactive` | checkbox | Inactive |  |
| `name` | text | Name |  |
| `savedsearch` | text | Saved Search |  |
