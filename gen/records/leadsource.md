# Lead Source (`leadsource`)
<!-- id: record:leadsource -->

| Fact | Value |
|---|---|
| Internal ID | `leadsource` |
| Category | marketing |
| Client scripts | yes |
| Server scripts | yes |
| Tags | `supports_create`, `supports_delete`, `supports_read`, `supports_update` |

## Record facts

- `script_types`: `client`, `server`
- `not_supported`: `copy`, `search`, `transform`
- `script_functions`: `create`, `read`, `update`, `delete`
- `requires_feature`: `Marketing Automation` [`real_name`]

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `description` | textarea | Description | no | no | marked_new | `purpose`: `description`; `doc_inconsistency`: `help_says_note_type` |
| `isinactive` | checkbox | Inactive | no | no | marked_new | `purpose`: `inactivate_record` |
| `name` | text | Name | yes | no | marked_new | `purpose`: `record_name` |
| `externalid` | text | External ID | no | no | marked_new |  |

## Suspected source errors

> [!SOURCE-ERROR] `help_says_note_type` (on `description`)
<!-- source-error: record:leadsource | description | help_says_note_type -->
