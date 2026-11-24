# Planning Item Category (`planningitemcategory`)
<!-- id: record:planningitemcategory -->

| Fact | Value |
|---|---|
| Internal ID | `planningitemcategory` |
| Category | item |
| Client scripts | yes |
| Server scripts | yes |
| Tags | `no_transform` |

## Record facts

- `script_types`: `client`, `server`
- `requires_feature`: `Material Requirements Planning` [`real_name`]
- `script_functions`: `create`, `read`, `update`, `delete`, `search`, `copy`
- `not_supported`: `transform`
- `purpose`: `item_hierarchy_for_aggregating_supply_planning`
- `configured_by`: `enable_features_items_and_inventory_subtab` [`material_requirements_planning`]
- `doc_inconsistency`: `ui_help_link_says_planned_order_record`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `customform` | select | Custom Form | no |  |  |  |
| `description` | textarea | Description | no |  |  |  |
| `externalid` | text | External ID | no |  |  |  |
| `id` | integer | Internal ID | no |  |  |  |
| `name` | text | Name | yes |  |  |  |
| `templatestored` | checkbox |  | no |  |  |  |

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

## Suspected source errors

> [!SOURCE-ERROR] `ui_help_link_says_planned_order_record`
<!-- source-error: record:planningitemcategory | - | ui_help_link_says_planned_order_record -->
