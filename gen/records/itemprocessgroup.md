# Item Process Group (`itemprocessgroup`)
<!-- id: record:itemprocessgroup -->

| Fact | Value |
|---|---|
| Internal ID | `itemprocessgroup` |
| Category | list |
| Client scripts | yes |
| Server scripts | yes |
| Searchable | yes |
| Tags | `scripting_full` |

## Record facts

- `script_types`: `client`, `server`
- `requires_feature`: `Warehouse Management` [`real_name`]
- `purpose`: `item_category_finer_than_item_process_family`
- `value_examples`: `clothing_family_with_pants_and_jeans_groups`
- `script_functions`: `create`, `copy`, `update`, `delete`, `search`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `description` | textarea | Description | no |  |  | `purpose`: `description` |
| `isinactive` | checkbox | Inactive | no |  |  | `purpose`: `inactivate_group`; `ui_effect`: `hidden_from_lists_and_not_selectable` |
| `name` | text | Name | yes |  |  | `purpose`: `group_name`; `constraint`: `unique` |
| `customform` | select | Custom Form | no |  |  |  |
| `externalid` | text | External ID | no |  |  |  |

## Sublist `translations` (Translation)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `description` | textarea | Description | no |  |  |
| `language` | text | Language | no |  |  |
| `locale` | select | Locale | no |  |  |
| `name` | text | Name | no |  |  |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `description` | textarea | Description |  |
| `descriptiontranslated` | textarea | Description (Translated) |  |
| `externalid` | select | External ID |  |
| `externalidstring` | text | External ID (Text) |  |
| `internalid` | select | Internal ID |  |
| `internalidnumber` | integer | Internal ID (Number) |  |
| `isinactive` | checkbox | Inactive |  |
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
| `name` | text | Name |  |
| `nametranslated` | text | Name (Translated) |  |
