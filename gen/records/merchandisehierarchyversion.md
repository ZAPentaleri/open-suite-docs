# Merchandise Hierarchy Version (`merchandisehierarchyversion`)
<!-- id: record:merchandisehierarchyversion -->

| Fact | Value |
|---|---|
| Internal ID | `merchandisehierarchyversion` |
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
- `scriptable_subtabs`: `hierarchylevels`
- `requires`: `levels_created_first`
- `purpose`: `levels_of_hierarchy_for_time_period`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `description` | textarea | Description | no |  |  | `purpose`: `version_description`; `ui_effect`: `text_enhance_button_may_appear` |
| `enddate` | date | End Date | no |  |  | `purpose`: `version_end_date` |
| `name` | text | Name | yes |  |  | `purpose`: `version_name` |
| `startdate` | date | Start Date | no |  |  | `purpose`: `version_start_date` |
| `externalid` | text | ExternalId | no |  |  |  |

## Sublist `hierarchylevels` (Hierarchy Levels)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `hierarchylevel` | select | Hierarchy Level | no |  |  |
| `isincluded` | checkbox | Included In This Version | no |  |  |
| `levelrank` | integer | Level Rank | no |  |  |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `description` | text | Description |  |
| `enddate` | date | End Date |  |
| `externalid` | select | External ID |  |
| `externalidstring` | text | External ID (Text) |  |
| `hierarchylevel` | select | Hierarchy Level |  |
| `internalid` | select | Internal ID |  |
| `internalidnumber` | integer | Internal ID (Number) |  |
| `name` | text | Name |  |
| `startdate` | date | Start Date |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `description` | text | Description |  |
| `enddate` | date | End Date |  |
| `externalid` | select | External ID |  |
| `hierarchylevel` | text | Hierarchy Level |  |
| `internalid` | select | Internal ID |  |
| `name` | text | Name |  |
| `startdate` | date | Start Date |  |
