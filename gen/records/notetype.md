# Note Type (`notetype`)
<!-- id: record:notetype -->

| Fact | Value |
|---|---|
| Internal ID | `notetype` |
| Category | list |
| Client scripts | yes |
| Server scripts | yes |
| Searchable | yes |
| Tags | `custom_fields_supported`, `scripting_full` |

## Record facts

- `script_types`: `client`, `server`
- `purpose`: `note_type_values`
- `ui_location`: `setup_sales_crm_lists_new_note_type`
- `script_functions`: `create`, `update`, `copy`, `delete`, `search`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `description` | text | Description | no | no |  | `purpose`: `description`; `doc_inconsistency`: `help_says_note_type` |
| `isinactive` | checkbox | Inactive | no | no |  | `purpose`: `inactivate_record` |
| `name` | text | Note Type | yes | no |  | `purpose`: `record_name` |
| `externalid` | text | ExternalId | no | no |  |  |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `description` | text | Description |  |
| `externalid` | select | External ID |  |
| `externalidstring` | text | External ID (Text) |  |
| `internalid` | select | Internal ID |  |
| `internalidnumber` | integer | Internal ID (Number) |  |
| `isinactive` | checkbox | Inactive |  |
| `name` | text | Name |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `description` | text | Description |  |
| `externalid` | select | External ID |  |
| `internalid` | select | Internal ID |  |
| `isinactive` | checkbox | Inactive |  |
| `name` | text | Name |  |

## Search joins

| Join ID | Label | Joined record |
|---|---|---|
| `user` | User | Employee |

## Suspected source errors

> [!SOURCE-ERROR] `help_says_note_type` (on `description`)
<!-- source-error: record:notetype | description | help_says_note_type -->
