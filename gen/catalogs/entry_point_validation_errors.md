# Catalog: entry_point_validation_errors
<!-- id: catalog:entry_point_validation_errors -->

| ID | Label | Value | Tags |
|---|---|---|---|
| `CANNOT_CHANGE_API_VERSION` |  | keep_api_version_of_attached_file | on_edit_of_attached_file, may_follow_invalid_api_version |
| `FAILED_TO_VALIDATE_SCRIPT_FILE` |  | add_required_elements_and_fix_structure | at_upload, napiversion_missing, global_variables_not_initialized |
| `INVALID_API_VERSION` |  | use_2.0_2.1_2.x_or_2.X | at_upload, on_edit |
| `INVALID_JSDOC_TAG_VALUE` |  | fix_jsdoc_tag_value | any_jsdoc_tag |
| `MISSING_SCRIPT_TYPE` |  | add_nscripttype_tag | at_script_record, on_edit_of_attached_file, upload_still_succeeds |
| `MULTIPLE_DEFINE_CALLS` |  | keep_one_define_statement | more_than_one_define |
| `SCRIPT_OF_API_VERSION_20_CANNOT_IMPLEMENT_MORE_THAN_ONE_SCRIPT_TYPE_INTERFACES` |  | one_script_type_per_file | entry_points_of_several_script_types |
| `SCRIPT_OF_API_VERSION_20_MUST_IMPLEMENT_A_SCRIPT_TYPE_INTERFACE` |  | include_at_least_one_entry_point | interface_missing_or_broken, at_upload, on_edit |
| `SS_V2_FILE_USED_FOR_FORM_SCRIPTS_MUST_IMPLEMENT_CLIENT_SCRIPT_TYPE` |  | attach_only_client_scripts_to_forms | at_form_attach |
| `SYNTAX_ERROR` |  | fix_javascript_syntax | javascript_syntax_error |
| `THE_FILE_IS_USED_AS_SCRIPT_TYPE_1_CANNOT_BE_CHANGED_TO_2` |  | use_new_file_and_new_script_record | on_edit_of_attached_file, script_type_changed |
| `WRONG_SCRIPT_TYPE` |  | match_nscripttype_to_entry_points | at_upload, nscripttype_incompatible |
