# Map/Reduce Script (`mapreducescript`)
<!-- id: record:mapreducescript -->

| Fact | Value |
|---|---|
| Internal ID | `mapreducescript` |

## Record facts

- `script_types`: `server`
- `user_events`: `beforeLoad`, `beforeSubmit`, `afterSubmit`
- `script_functions`: `update`, `search`
- `not_supported`: `create`, `copy`, `delete`
- `not_scriptable`: `change_id_button`, `unhandled_errors_subtab`, `history_subtab`, `system_notes_subtab`, `execution_log_subtab`, `libraries_subtab`
- `not_supported_in`: `client_scripts`
- `read_only_fields`: `type` [`parameters_subtab_read_only`]
- `requires`: `script_owner_role_with_suitescript_permission`
- `caveat`: `scripts_reach_script_records_only_in_same_bundle`
- `requires`: `numeric_internal_id_not_customscript_id` (when `id_parameter`)
- `ui_location`: `customization_scripting_scripts_internal_id_column`
- `configured_by`: `show_internal_ids_set_preference` (when `showing_internal_id_column`)
- `value_examples`: `191:customscript_csalert`
- `doc_inconsistency`: `samples_claim_create_though_create_unsupported`

## Body fields

| Field ID | Type | Label | Required | submitField | Read-only | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `apiversion` | select | API VERSION | no | no | yes |  | `purpose`: `suitescript_version` |
| `description` | textarea | Description | no | no |  |  | `purpose`: `script_description` |
| `isinactive` | checkbox | Inactive | no | no |  |  | `purpose`: `inactivate_script`; `effect`: `all_deployments_inactive` |
| `name` | text | Name | yes | no |  |  | `purpose`: `script_name`; `recommendation`: `generic_across_deployments`; `value_examples`: `omit_record_type_when_deployed_to_several` |
| `notifyadmins` | checkbox | Notify All Admins | no | no |  |  | `purpose`: `notify_admins_on_error`; `true_means`: `email_all_administrators_on_error` |
| `notifyemails` | emails | Notify Emails | no | no |  |  | `purpose`: `error_notification_emails`; `format`: `comma_separated_emails` |
| `notifygroup` | select | Notify Group | no | no |  |  | `purpose`: `notify_group_on_error`; `allowed_values`: `existing_groups`; `allowed_values_listed_in`: `group_records` |
| `notifyowner` | checkbox | Notify Script Owner | no | no |  |  | `purpose`: `notify_owner_on_error`; `true_means`: `email_owner_on_error` |
| `owner` | select | Owner | no | no |  |  | `purpose`: `script_owner`; `default_behavior`: `current_user`; `constraint`: `only_owner_can_modify_script` |
| `scriptfile` | select | Script File | yes | no |  |  | `purpose`: `script_file`; `requires`: `file_uploaded_to_suitescripts_folder`; `ui_effect`: `new_icon_uploads_file_in_edit_mode` |
| `scriptid` | identifier | ID | no | no |  |  | `purpose`: `script_id`; `default_behavior`: `system_generated` (when `blank`); `required_prefix`: `customscript` [`added_on_save`]; `required_prefix`: `customdeploy` [`deployments`]; `format`: `lowercase_no_spaces`; `recommendation`: `custom_id_when_bundling`; `can`: `change_with_change_id_button`; `value_examples`: `employeeupdates_becomes_customdeployemployeeupdates`; `reason`: `custom_ids_avoid_conflicts_across_accounts` |
| `externalid` | text | ExternalId | no | no |  |  |  |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `apiversion` | select | API Version |  |
| `description` | textarea | Details |  |
| `formuladate` | date | Formula (Date) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulatext` | text | Formula (Text) |  |
| `internalid` | select | Internal ID |  |
| `internalidnumber` | integer | Internal ID (Number) |  |
| `isinactive` | checkbox | Inactive |  |
| `name` | text | Name |  |
| `owner` | select | Owner |  |
| `scriptfile` | select | Script File |  |
| `scriptid` | text | Script ID |  |
| `scripttype` | select | Script Type |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `apiversion` | text | API Version |  |
| `description` | text | Description |  |
| `formulacurrency` | currency | Formula (Currency) |  |
| `formuladate` | date | Formula (Date) |  |
| `formuladatetime` | datetime | Formula (Date/Time) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulapercent` | percent | Formula (Percent) |  |
| `formulatext` | text | Formula (Text) |  |
| `internalid` | select | Internal ID |  |
| `isinactive` | checkbox | Inactive |  |
| `name` | text | Name |  |
| `owner` | select | Owner |  |
| `scriptfile` | select | Script File |  |
| `scriptid` | text | Script ID |  |
| `scripttype` | select | Script Type |  |

## Search joins

| Join ID | Label | Joined record |
|---|---|---|
| `user` | User | Employee |

## Suspected source errors

> [!SOURCE-ERROR] `samples_claim_create_though_create_unsupported`
<!-- source-error: record:mapreducescript | - | samples_claim_create_though_create_unsupported -->
