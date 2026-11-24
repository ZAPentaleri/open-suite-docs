# Planning Rule Group (`planningrulegroup`)
<!-- id: record:planningrulegroup -->

| Fact | Value |
|---|---|
| Internal ID | `planningrulegroup` |
| Category | item |
| Client scripts | yes |
| Server scripts | yes |
| Tags | `no_transform` |

## Record facts

- `script_types`: `client`, `server`
- `requires_feature`: `Material Requirements Planning` [`real_name`]
- `sublist_ids`: `RULE_FS_TAB`
- `applies_to`: `transfer_orders`
- `script_functions`: `create`, `read`, `update`, `delete`, `search`, `copy`
- `not_supported`: `transform`
- `purpose`: `rules_for_replenishing_items_and_categories`
- `value_examples`: `supply_source_for_item_at_location`
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

## Sublist `planningrules`

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `action` | select | Action | yes |  |  |
| `id` | integer | Internal ID | no |  |  |
| `item` | integer | Item | no |  |  |
| `itemdescription` | text | Description | no |  |  |
| `itemorcategory` | select | Item / Item Category | yes |  |  |
| `itemtype` | select | Item Type | yes |  |  |
| `leadtime` | integer | Lead Time (Days) | yes |  |  |
| `location` | select | Location | yes |  |  |
| `planningitemcategory` | integer | Planning Item Category | no |  |  |
| `planningrulegroup` | select | Planning Rule Group id | no |  |  |
| `sourcelocation` | select | Source Location | yes |  |  |

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
<!-- source-error: record:planningrulegroup | - | ui_help_link_says_planned_order_record -->
