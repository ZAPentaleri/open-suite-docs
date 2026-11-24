# Zone (`zone`)
<!-- id: record:zone -->

| Fact | Value |
|---|---|
| Internal ID | `zone` |
| Category | list |
| Client scripts | yes |
| Server scripts | yes |
| Searchable | yes |
| Tags | `scripting_full` |

## Record facts

- `script_types`: `client`, `server`
- `requires_feature`: `Warehouse Management` [`real_name`]
- `purpose`: `warehouse_areas_for_putaway_storage_picking`
- `script_functions`: `create`, `copy`, `update`, `delete`, `search`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `description` | textarea | Description | no |  |  | `purpose`: `description` |
| `isinactive` | checkbox | Inactive | no |  |  | `purpose`: `inactivate_zone`; `ui_effect`: `hidden_from_lists_and_not_selectable` |
| `location` | select | Location | yes |  |  | `purpose`: `warehouse_location` |
| `name` | text | Name | yes |  |  | `purpose`: `zone_name`; `constraint`: `unique_within_location` |
| `externalid` | text | External ID | no |  |  |  |

## Sublist `translations` (Translation)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `description` | textarea | Description | no |  |  |
| `language` | text | Language | no |  |  |
| `locale` | select | Locale | no |  |  |
| `name` | text | Name | no |  |  |
| `zone` | select | Zone | no |  |  |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `description` | textarea | Description |  |
| `descriptiontranslated` | textarea | Description (Translated) |  |
| `externalid` | select | External ID |  |
| `internalid` | select | Internal ID |  |
| `isinactive` | checkbox | Inactive |  |
| `location` | select | Location |  |
| `name` | text | Name |  |
| `nametranslated` | text | Name (Translated) |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `description` | textarea | Description |  |
| `descriptiontranslated` | textarea | Description (Translated) |  |
| `externalid` | select | External ID |  |
| `internalid` | select | Internal ID |  |
| `isinactive` | checkbox | Inactive |  |
| `location` | select | Location |  |
| `name` | text | Name |  |
| `nametranslated` | text | Name (Translated) |  |
