# Planning Item Group (`planningitemgroup`)
<!-- id: record:planningitemgroup -->

| Fact | Value |
|---|---|
| Internal ID | `planningitemgroup` |
| Category | item |
| Client scripts | yes |
| Server scripts | yes |
| Tags | `no_transform` |

## Record facts

- `script_types`: `client`, `server`
- `requires_feature`: `Material Requirements Planning` [`real_name`]
- `sublist_ids`: `ITEM_FS_TAB`
- `script_functions`: `create`, `read`, `update`, `delete`, `search`, `copy`
- `not_supported`: `transform`
- `purpose`: `reusable_item_set_planned_together`
- `configured_by`: `enable_features_items_and_inventory_subtab` [`material_requirements_planning`]

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `customform` | select | Custom Form | no |  |  |  |
| `description` | textarea | Description | no |  |  |  |
| `externalid` | text | External ID | no |  |  |  |
| `id` | integer | Internal ID | no |  |  |  |
| `name` | text | Name | yes |  |  |  |
| `templatestored` | checkbox |  | no |  |  |  |

## Sublist `planningitems`

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `id` | integer | Internal ID | no |  |  |
| `item` | integer | Item | no |  |  |
| `itemdescription` | text | Description | no |  |  |
| `itemorcategory` | select | Item / Item Category | yes |  |  |
| `itemtype` | select | Item Type | yes |  |  |
| `planningitemcategory` | integer | Planning Item Category | no |  |  |
| `planningitemgroup` | select | Planning Item Group id | no |  |  |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `description` | text | Description |  |
| `externalid` | select | External ID |  |
| `externalidstring` | text | External ID (Text) |  |
| `internalid` | select | Internal ID |  |
| `internalidnumber` | integer | Internal ID (Number) |  |
| `name` | text | Name |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `description` | text | Description |  |
| `externalid` | select | External ID |  |
| `internalid` | select | Internal ID |  |
| `name` | text | Name |  |
