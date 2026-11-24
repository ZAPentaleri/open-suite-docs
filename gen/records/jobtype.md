# Job Type (`jobtype`)
<!-- id: record:jobtype -->

| Fact | Value |
|---|---|
| Internal ID | `jobtype` |
| Category | entity |
| Client scripts | yes |
| Server scripts | yes |
| Searchable | yes |
| Tags | `custom_fields_supported`, `scripting_full` |

## Record facts

- `script_types`: `client`, `server`
- `script_functions`: `create`, `update`, `copy`, `delete`, `search`, `transform`
- `requires_feature`: `Projects` [`project_management_for_advanced_tracking`, `real_name`]
- `configured_by`: `enable_features_company_subtab` [`projects`, `project_management`]
- `caveat`: `project_management_box_needs_purchased_add_on`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `isinactive` | checkbox | Inactive | no | no |  | `purpose`: `inactivate_record`; `can`: `reactivate_any_time`; `doc_inconsistency`: `says_removes_all_references` |
| `name` | text | Job Type | yes | no |  | `purpose`: `record_name`; `displayed_on`: `lists` |
| `parent` | select | Subtype of | no | no |  | `purpose`: `parent_job_type` |
| `externalid` | text | ExternalId | no | no |  |  |

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

> [!SOURCE-ERROR] `says_removes_all_references` (on `isinactive`)
<!-- source-error: record:jobtype | isinactive | says_removes_all_references -->
