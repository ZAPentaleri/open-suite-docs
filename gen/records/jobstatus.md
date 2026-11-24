# Job Status (`jobstatus`)
<!-- id: record:jobstatus -->

| Fact | Value |
|---|---|
| Internal ID | `jobstatus` |
| Category | entity |
| Client scripts | yes |
| Server scripts | yes |
| Searchable | yes |
| Tags | `custom_fields_supported`, `scripting_full` |

## Record facts

- `script_types`: `client`, `server`
- `script_functions`: `create`, `update`, `copy`, `delete`, `search`, `transform`
- `requires_feature`: `Projects` [`project_management_for_advanced_tracking`, `real_name`]
- `purpose`: `project_progress_status`
- `ui_location`: `setup_accounting_accounting_lists_new_project_status`
- `configured_by`: `enable_features_company_subtab` [`projects`, `project_management`]
- `caveat`: `project_management_box_needs_purchased_add_on`
- `record_actions`: `recalculatepercentcompleteoverride`
- `action_effect`: `percent_complete_recalculated_every_8_hours`
- `doc_inconsistency`: `action_described_on_project_revenue_rules`
- `ui_location`: `recalculate_percent_complete_override_button`
- `default_value`: `true` [`in_progress_status_auto_recalculation`]
- `can`: `trigger_recalculation_from_project_actions_menu`
- `user_events_supported`: `false`
- `configured_by`: `percent_complete_automatic_recalculation_box`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `description` | textarea | Description | no | no |  | `purpose`: `list_item_description` |
| `isinactive` | checkbox | Inactive | no | no |  | `purpose`: `inactivate_record`; `can`: `reactivate_any_time`; `doc_inconsistency`: `says_removes_all_references` |
| `name` | text | Status | yes | no |  | `purpose`: `record_name`; `displayed_on`: `lists` |
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

> [!SOURCE-ERROR] `action_described_on_project_revenue_rules`
<!-- source-error: record:jobstatus | - | action_described_on_project_revenue_rules -->

> [!SOURCE-ERROR] `says_removes_all_references` (on `isinactive`)
<!-- source-error: record:jobstatus | isinactive | says_removes_all_references -->
