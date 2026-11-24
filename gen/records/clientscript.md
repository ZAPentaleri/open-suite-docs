# Client Script (`clientscript`)
<!-- id: record:clientscript -->

| Fact | Value |
|---|---|
| Internal ID | `clientscript` |

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

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `description` | textarea | Description | no | no |  | `purpose`: `script_description` |
| `fieldchangedfunction` | text | Field Changed Function | no | no |  | `purpose`: `field_changed_function`; `format`: `function_name_without_parentheses`; `runs_when`: `field_changed_by_user_or_client_api`; `value_examples`: `sampleFunction_not_sampleFunction(arg1)` |
| `isinactive` | checkbox | Inactive | no | no |  | `purpose`: `inactivate_script`; `effect`: `all_deployments_inactive` |
| `lineinitfunction` | text | Line Init Function | no | no |  | `purpose`: `line_init_function`; `format`: `function_name_without_parentheses`; `runs_when`: `existing_line_selected`; `value_examples`: `sampleFunction_not_sampleFunction(arg1)` |
| `name` | text | Name | yes | no |  | `purpose`: `script_name`; `recommendation`: `generic_across_deployments`; `value_examples`: `omit_record_type_when_deployed_to_several` |
| `notifyadmins` | checkbox | Notify All Admins | no | no |  | `purpose`: `notify_admins_on_error`; `true_means`: `email_all_administrators_on_error` |
| `notifyemails` | emails | Notify Emails | no | no |  | `purpose`: `error_notification_emails`; `format`: `comma_separated_emails` |
| `notifygroup` | select | Notify Group | no | no |  | `purpose`: `notify_group_on_error`; `allowed_values`: `existing_groups`; `allowed_values_listed_in`: `group_records` |
| `notifyowner` | checkbox | Notify Script Owner | no | no |  | `purpose`: `notify_owner_on_error`; `true_means`: `email_owner_on_error` |
| `notifyuser` | checkbox | Notify Current User | no | no |  | `purpose`: `notify_current_user_on_error` |
| `owner` | select | Owner | no | no |  | `purpose`: `script_owner`; `default_behavior`: `current_user`; `constraint`: `only_owner_can_modify_script` |
| `pageinitfunction` | text | Page Init Function | no | no |  | `purpose`: `page_init_function`; `format`: `function_name_without_parentheses`; `runs_when`: `page_loaded_or_form_reset`; `value_examples`: `sampleFunction_not_sampleFunction(arg1)` |
| `postsourcingfunction` | text | Post Sourcing Function | no | no |  | `purpose`: `post_sourcing_function`; `format`: `function_name_without_parentheses`; `runs_when`: `after_field_change_child_values_sourced`; `value_examples`: `sampleFunction_not_sampleFunction(arg1)`; `use_when`: `logic_needs_all_dependent_values_sourced` |
| `recalcfunction` | text | Recalc Function | no | no |  | `purpose`: `recalc_function`; `format`: `function_name_without_parentheses`; `runs_when`: `after_line_added_to_sublist`; `value_examples`: `sampleFunction_not_sampleFunction(arg1)`; `use_when`: `recalculating_totals_after_sublist_change` |
| `saverecordfunction` | text | Save Record Function | no | no |  | `purpose`: `save_record_function`; `format`: `function_name_without_parentheses`; `runs_when`: `before_form_submitted_on_save`; `value_examples`: `sampleFunction_not_sampleFunction(arg1)` |
| `scriptfile` | select | Script File | yes | no |  | `purpose`: `script_file`; `requires`: `file_uploaded_to_suitescripts_folder`; `ui_effect`: `new_icon_uploads_file_in_edit_mode` |
| `scriptid` | identifier | ID | no | no |  | `purpose`: `script_id`; `default_behavior`: `system_generated` (when `blank`); `required_prefix`: `customscript` [`added_on_save`]; `required_prefix`: `customdeploy` [`deployments`]; `format`: `lowercase_no_spaces`; `recommendation`: `custom_id_when_bundling`; `can`: `change_with_change_id_button`; `value_examples`: `employeeupdates_becomes_customdeployemployeeupdates`; `reason`: `custom_ids_avoid_conflicts_across_accounts` |
| `scripttype` | select | Type | no | no |  | `purpose`: `script_type`; `allowed_values`: `Suitelet`, `RESTlet`, `User Event`, `Scheduled`, `Map/Reduce`, `Client`, `Portlet`, `Mass Update`, `Workflow Action`, `Bundle Installation`; `caveat`: `client_means_record_level_deployed_client_script` |
| `validatedeletefunction` | text | Validate Delete Function | no | no |  | `purpose`: `validate_delete_function`; `runs_when`: `removing_line_from_edit_sublist`; `false_means`: `removal_blocked` |
| `validatefieldfunction` | text | Validate Field Function | no | no |  | `purpose`: `validate_field_function`; `format`: `function_name_without_parentheses`; `runs_when`: `field_about_to_change_by_user_or_client_api`; `value_examples`: `sampleFunction_not_sampleFunction(arg1)` |
| `validateinsertfunction` | text | Validate Insert Function | no | no |  | `purpose`: `validate_insert_function`; `runs_when`: `line_inserted_into_edit_sublist`; `comparable_to`: `insert_button_or_insertlineitem`; `false_means`: `insert_blocked` |
| `validatelinefunction` | text | Validate Line Function | no | no |  | `purpose`: `validate_line_function`; `format`: `function_name_without_parentheses`; `runs_when`: `before_line_added_to_sublist`; `value_examples`: `sampleFunction_not_sampleFunction(arg1)` |
| `externalid` | text | ExternalId | no | no |  |  |

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
<!-- source-error: record:clientscript | - | samples_claim_create_though_create_unsupported -->
