# Massupdate Script (`massupdatescript`)
<!-- id: record:massupdatescript -->

| Fact | Value |
|---|---|
| Internal ID | `massupdatescript` |

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
| `defaultfunction` | text | Function | yes | no | yes |  | `purpose`: `entry_point_implemented_flag`; `value_source`: `entry_points_returned_by_script` |
| `description` | textarea | Description | no | no |  |  | `purpose`: `script_description` |
| `isinactive` | checkbox | Inactive | no | no |  |  | `purpose`: `inactivate_script`; `effect`: `all_deployments_inactive` |
| `name` | text | Name | yes | no |  |  | `purpose`: `script_name`; `recommendation`: `generic_across_deployments`; `value_examples`: `omit_record_type_when_deployed_to_several` |
| `notifyadmins` | checkbox | Notify All Admins | no | no |  |  | `purpose`: `notify_admins_on_error`; `true_means`: `email_all_administrators_on_error` |
| `notifyemails` | emails | Notify Emails | no | no |  |  | `purpose`: `error_notification_emails`; `format`: `comma_separated_emails` |
| `notifygroup` | select | Notify Group | no | no |  |  | `purpose`: `notify_group_on_error`; `allowed_values`: `existing_groups`; `allowed_values_listed_in`: `group_records` |
| `notifyowner` | checkbox | Notify Script Owner | no | no |  |  | `purpose`: `notify_owner_on_error`; `true_means`: `email_owner_on_error` |
| `notifyuser` | checkbox | Notify Current User | no | no |  |  | `purpose`: `notify_current_user_on_error` |
| `owner` | select | Owner | no | no |  |  | `purpose`: `script_owner`; `default_behavior`: `current_user`; `constraint`: `only_owner_can_modify_script` |
| `scriptfile` | select | Script File | yes | no |  |  | `purpose`: `script_file`; `requires`: `file_uploaded_to_suitescripts_folder`; `ui_effect`: `new_icon_uploads_file_in_edit_mode` |
| `scriptid` | identifier | ID | no | no |  |  | `purpose`: `script_id`; `default_behavior`: `system_generated` (when `blank`); `required_prefix`: `customscript` [`added_on_save`]; `required_prefix`: `customdeploy` [`deployments`]; `format`: `lowercase_no_spaces`; `recommendation`: `custom_id_when_bundling`; `can`: `change_with_change_id_button`; `value_examples`: `employeeupdates_becomes_customdeployemployeeupdates`; `reason`: `custom_ids_avoid_conflicts_across_accounts` |
| `scripttype` | select | Type | no | no |  |  | `purpose`: `script_type`; `allowed_values`: `Suitelet`, `RESTlet`, `User Event`, `Scheduled`, `Map/Reduce`, `Client`, `Portlet`, `Mass Update`, `Workflow Action`, `Bundle Installation`; `caveat`: `client_means_record_level_deployed_client_script` |
| `externalid` | text | ExternalId | no | no |  |  |  |

## Tabs

| Tab ID | Label |
|---|---|
| `errorhandling` | Unhandled Errors |
| `executionlog` | Execution Log |
| `historytab` | History |
| `scriptdeployments` | Deployments |
| `scripts` | Scripts |
| `tab_parameters` | Parameters |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `afterinstallfunction` | text | After Install Function |  |
| `aftersubmitfunction` | text | After Submit Function |  |
| `afterupdatefunction` | text | After Update Function |  |
| `beforeinstallfunction` | text | Before Install Function |  |
| `beforeloadfunction` | text | Before Load Function |  |
| `beforesubmitfunction` | text | Before Submit Function |  |
| `beforeuninstallfunction` | text | Before Uninstall Function |  |
| `beforeupdatefunction` | text | Before Update Function |  |
| `defaultfunction` | text | Default Function |  |
| `deletefunction` | text | Delete Function |  |
| `description` | textarea | Details |  |
| `fieldchangedfunction` | text | Field Changed Function |  |
| `formuladate` | date | Formula (Date) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulatext` | text | Formula (Text) |  |
| `getfunction` | text | Get Function |  |
| `internalid` | select | Internal ID |  |
| `internalidnumber` | integer | Internal ID (Number) |  |
| `isinactive` | checkbox | Inactive |  |
| `lineinitfunction` | text | Line Init Function |  |
| `name` | text | Name |  |
| `owner` | select | Owner |  |
| `pageinitfunction` | text | Page Init Function |  |
| `portlettype` | select | Portlet Type |  |
| `postfunction` | text | Post Function |  |
| `postsourcingfunction` | text | Post Sourcing Function |  |
| `putfunction` | text | Put Function |  |
| `recalcfunction` | text | Recalc Function |  |
| `saverecordfunction` | text | Save Record Function |  |
| `scriptfile` | select | Script File |  |
| `scriptid` | text | Script ID |  |
| `scripttype` | select | Script Type |  |
| `validatedeletefunction` | text | Validate Delete Function |  |
| `validatefieldfunction` | text | Validate Field Function |  |
| `validateinsertfunction` | text | Validate Insert Function |  |
| `validatelinefunction` | text | Validate Line Function |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `afterinstallfunction` | text | After Install Function |  |
| `aftersubmitfunction` | text | After Submit Function |  |
| `afterupdatefunction` | text | After Update Function |  |
| `beforeinstallfunction` | text | Before Install Function |  |
| `beforeloadfunction` | text | Before Load Function |  |
| `beforesubmitfunction` | text | Before Submit Function |  |
| `beforeuninstallfunction` | text | Before Uninstall Function |  |
| `beforeupdatefunction` | text | Before Update Function |  |
| `defaultfunction` | text | Default Function |  |
| `deletefunction` | text | DELETE Function |  |
| `description` | text | Description |  |
| `fieldchangedfunction` | text | Field Changed Function |  |
| `formulacurrency` | currency | Formula (Currency) |  |
| `formuladate` | date | Formula (Date) |  |
| `formuladatetime` | datetime | Formula (Date/Time) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulapercent` | percent | Formula (Percent) |  |
| `formulatext` | text | Formula (Text) |  |
| `getfunction` | text | GET Function |  |
| `internalid` | select | Internal ID |  |
| `isinactive` | checkbox | Inactive |  |
| `lineinitfunction` | text | Line Init Function |  |
| `name` | text | Name |  |
| `owner` | select | Owner |  |
| `pageinitfunction` | text | Page Init Function |  |
| `postfunction` | text | POST Function |  |
| `postsourcingfunction` | text | Post Sourcing Function |  |
| `putfunction` | text | PUT Function |  |
| `recalcfunction` | text | Recalc Function |  |
| `saverecordfunction` | text | Save Record Function |  |
| `scriptfile` | select | Script File |  |
| `scriptid` | text | Script ID |  |
| `scripttype` | select | Script Type |  |
| `validatedeletefunction` | text | Validate Delete Function |  |
| `validatefieldfunction` | text | Validate Field Function |  |
| `validateinsertfunction` | text | Validate Insert Function |  |
| `validatelinefunction` | text | Validate Line Function |  |

## Search joins

| Join ID | Label | Joined record |
|---|---|---|
| `user` | User | Employee |

## Suspected source errors

> [!SOURCE-ERROR] `samples_claim_create_though_create_unsupported`
<!-- source-error: record:massupdatescript | - | samples_claim_create_though_create_unsupported -->
