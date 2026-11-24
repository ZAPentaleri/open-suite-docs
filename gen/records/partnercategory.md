# Partner Category (`partnercategory`)
<!-- id: record:partnercategory -->

| Fact | Value |
|---|---|
| Internal ID | `partnercategory` |
| Category | list |
| Client scripts | yes |
| Server scripts | yes |
| Searchable | yes |
| Tags | `custom_fields_supported`, `scripting_full` |

## Record facts

- `script_types`: `client`, `server`
- `doc_inconsistency`: `script_type_sentence_names_note_type`
- `ui_location`: `setup_sales_setup_tasks_crm_lists`
- `script_functions`: `create`, `update`, `copy`, `delete`, `search`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `isinactive` | checkbox | Inactive | no | no |  | `purpose`: `inactivate_record` |
| `name` | text | Partner Category/Role | yes | no |  | `purpose`: `record_name` |
| `parent` | select | Subtype of | no | no |  | `purpose`: `parent_category` |
| `externalid` | text | ExternalId | no | no |  |  |
| `id` | integer | Internal ID | no |  | marked_new |  |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `externalid` | select | External ID |  |
| `externalidstring` | text | External ID (Text) |  |
| `internalid` | select | Internal ID |  |
| `internalidnumber` | integer | Internal ID (Number) |  |
| `isinactive` | checkbox | Inactive |  |
| `name` | text | Name |  |
| `parent` | select | Parent |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `externalid` | select | External ID |  |
| `internalid` | select | Internal ID |  |
| `isinactive` | checkbox | Inactive |  |
| `name` | text | Name |  |
| `parent` | select | Parent |  |

## Search joins

| Join ID | Label | Joined record |
|---|---|---|
| `user` | User | Employee |

## Suspected source errors

> [!SOURCE-ERROR] `script_type_sentence_names_note_type`
<!-- source-error: record:partnercategory | - | script_type_sentence_names_note_type -->
