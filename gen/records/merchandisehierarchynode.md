# Merchandise Hierarchy Node (`merchandisehierarchynode`)
<!-- id: record:merchandisehierarchynode -->

| Fact | Value |
|---|---|
| Internal ID | `merchandisehierarchynode` |
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
- `scriptable_subtabs`: `hierarchyversions`
- `purpose`: `group_items_with_similar_characteristics`
- `contains`: `hierarchy_position`, `versions`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `description` | textarea | Description | no |  |  | `purpose`: `node_description`; `ui_effect`: `text_enhance_button_may_appear` |
| `name` | text | Name | yes |  |  | `purpose`: `node_name` |
| `externalid` | text | ExternalId | no |  |  |  |

## Sublist `hierarchyversions` (Hierarchy Versions)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `enddate` | date | End Date | no |  |  |
| `hierarchylevel` | select | Hierarchy Level | no |  |  |
| `hierarchyversion` | select | Hierarchy Version | no |  |  |
| `isincluded` | checkbox | Included In Version | no |  |  |
| `parentnode` | select | Parent Node | no |  |  |
| `startdate` | date | Start Date | no |  |  |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `description` | text | Description |  |
| `externalid` | select | External ID |  |
| `externalidstring` | text | External ID (Text) |  |
| `hierarchylevel` | select | Hierarchy Level |  |
| `hierarchyversion` | select | Hierarchy Version |  |
| `internalid` | select | Internal ID |  |
| `internalidnumber` | integer | Internal ID (Number) |  |
| `name` | text | Name |  |
| `parentnode` | select | Hierarchy Parent Node |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `description` | text | Description |  |
| `externalid` | select | External ID |  |
| `hierarchylevel` | text | Hierarchy Level |  |
| `hierarchyversion` | text | Hierarchy Version |  |
| `internalid` | select | Internal ID |  |
| `name` | text | Name |  |
| `parentnode` | text | Hierarchy Parent Node |  |
