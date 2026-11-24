# Merchandise Hierarchy Level (`merchandisehierarchylevel`)
<!-- id: record:merchandisehierarchylevel -->

| Fact | Value |
|---|---|
| Internal ID | `merchandisehierarchylevel` |
| Category | list |
| Client scripts | no |
| Server scripts | yes |
| Searchable | yes |
| Tags | `custom_fields_supported`, `supports_create`, `supports_delete`, `supports_read`, `supports_search`, `supports_update` |

## Record facts

- `script_types`: `server`
- `script_functions`: `read`, `create`, `update`, `delete`, `search`
- `requires_feature`: `Merchandise Hierarchy`, `Custom Segments` [`real_name`]
- `client_supported`: `false`
- `purpose`: `categorize_hierarchy_nodes_and_item_relationships`

## Body fields

| Field ID | Type | Label | Required | submitField | Read-only | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `description` | textarea | Description | no |  |  |  | `purpose`: `level_description`; `ui_effect`: `text_enhance_button_may_appear` |
| `insertbefore` | select | Insert Before | no |  |  |  | `purpose`: `insert_before_level`; `effect`: `determines_rank` |
| `name` | text | Name | yes |  |  |  | `purpose`: `level_name` |
| `rank` | integer | Rank | no |  | yes |  | `purpose`: `level_rank`; `value_source`: `insertbefore`; `option_meanings`: `1:top_level` |
| `externalid` | text | ExternalId | no |  |  |  |  |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `description` | text | Description |  |
| `externalid` | select | External ID |  |
| `externalidstring` | text | External ID (Text) |  |
| `internalid` | select | Internal ID |  |
| `internalidnumber` | integer | Internal ID (Number) |  |
| `name` | text | Name |  |
| `rank` | integer | Rank |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `description` | text | Description |  |
| `externalid` | select | External ID |  |
| `internalid` | select | Internal ID |  |
| `name` | text | Name |  |
| `rank` | integer | Rank |  |
