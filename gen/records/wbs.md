# Work Breakdown Structure (`wbs`)
<!-- id: record:wbs -->

| Fact | Value |
|---|---|
| Internal ID | `wbs` |
| Category | list |
| Client scripts | yes |
| Server scripts | yes |
| Tags | `no_copy`, `no_transform` |

## Record facts

- `requires_feature`: `Job Costing and Project Budgeting`, `Advanced Project Accounting`, `Custom Records`, `Custom Segments`, `Activity Codes` [`real_name`]
- `timeline_types`: `global:default`, `monthly`
- `rule`: `one_task_or_activity_code_per_line`
- `record_macros`: `getAmountsFields`, `setAmountFieldValue`, `getAmountFieldValue`, `getUnmatchedActuals`
- `purpose`: `project_work_split_into_exclusive_parts`
- `displayed_on`: `project_wbs_subtab`
- `ui_location`: `lists_relationships_projects`
- `caveat`: `monthly_timeline_hides_actuals_outside_range`
- `effect`: `unmatched_revenue_actuals_shown_separately`
- `effect`: `actuals_matched_by_task_then_activity_code`
- `requires`: `project_field_set_when_scripting`
- `difference`: `ui_fills_project_context_script_must_set_it`
- `recommendation`: `suitescript_2_x_for_macros`
- `script_functions`: `create`, `read`, `update`, `delete`, `search`
- `record_macros`: `getAmountsFields`, `setAmountFieldValue`, `getAmountFieldValue`, `getUnmatchedActuals`
- `raises_error`: `INVALID_FLD_VALUE` (when `invalid_value_type`)
- `raises_error`: `SSS_MISSING_REQD_ARGUMENT` (when `required_argument_missing`)
- `raises_error`: `SSS_COLUMN_DOES_NOT_EXIST` (when `invalid_field_id`)
- `raises_error`: `YOU_HAVE_ATTEMPTED_AN_UNSUPPORTED_ACTION` (when `unsupported_field`)
- `raises_error`: `SSS_INVALID_SUBLIST_OPERATION` (when `invalid_line_index`)
- `ui_location`: `get_amounts_fields_set_get_amount_unmatched_actuals_buttons`
- `constraint`: `only_etc_field_settable`
- `format`: `zero_based_lineindex`
- `value_examples`: `update_etc_on_first_line`, `unmatched_cost_and_revenue`
- `doc_inconsistency`: `set_params_missing_get_lists_value_param`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `description` | text | Description | no |  |  | `purpose`: `description` |
| `enddate` | date | End Date | no |  |  | `purpose`: `wbs_end_date`; `default_behavior`: `global_wbs` (when `blank`) |
| `margintarget` | percent | Margin Target | no |  |  | `purpose`: `margin_target`; `effect`: `updates_estimated_margin_chart` |
| `project` | select | Project | yes |  |  | `purpose`: `project`; `value_source`: `system_displayed` |
| `startdate` | date | Start Date | no |  |  | `purpose`: `wbs_start_date`; `effect`: `estimates_start_from_month` |
| `timelinetype` | select | Timeline Type | yes |  |  | `purpose`: `timeline_type`; `options`: `Global`, `Monthly`; `default_value`: `Global`; `caveat`: `monthly_hides_actuals_outside_range` |
| `createdby` | select | Created By | no |  |  |  |
| `createddate` | datetime | Created Date | no |  |  |  |
| `customform` | select | Custom Form | no |  |  |  |
| `externalid` | text | External ID | no |  |  |  |
| `id` | integer | Internal ID | no |  |  |  |
| `internalid` | integer | Internal ID | no |  |  |  |
| `isinactive` | checkbox | Inactive | no |  |  |  |
| `lastmodifiedby` | select | Last Modified By | no |  |  |  |
| `lastmodifieddate` | datetime | Last Modified Date | no |  |  |  |
| `templatestored` | checkbox |  | no |  |  |  |

## Sublist `lines` (Work Breakdown Structure Line)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `key` | integer | key | no |  |  |
| `name` | text | Name | yes |  |  |
| `parentkey` | integer | Parent | no |  |  |
| `projecttask` | select | Project Task | no |  |  |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `amountdate` | date | Amounts - Date |  |
| `amounttype` | select | Amounts - Type |  |
| `created` | date | Created |  |
| `createdby` | select | Created By |  |
| `description` | text | Description |  |
| `eac` | currency | Amounts - Estimation at Completion |  |
| `enddate` | date | End Date |  |
| `externalid` | select | External ID |  |
| `externalidstring` | text | External ID (Text) |  |
| `internalid` | select | Internal ID |  |
| `internalidnumber` | integer | Internal ID (Number) |  |
| `isinactive` | checkbox | Inactive |  |
| `item` | select | Lines - Item |  |
| `lastmodified` | date | Last Modified |  |
| `lastmodifiedby` | select | Last Modified By |  |
| `margintarget` | percent | Margin Target |  |
| `name` | text | Lines - Name |  |
| `project` | select | Project |  |
| `projecttask` | select | Lines - Project Task |  |
| `resource` | select | Lines - Resource |  |
| `resourceitem` | select | Lines - Resource Item |  |
| `startdate` | date | Start Date |  |
| `timelinetype` | select | Timeline Type |  |
| `vendor` | select | Lines - Vendor |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `amountdate` | date | Amounts - Date |  |
| `amounttype` | select | Amounts - Type |  |
| `created` | date | Created |  |
| `createdby` | select | Created By |  |
| `description` | text | Description |  |
| `eac` | currency | Amounts - Estimation at Completion |  |
| `enddate` | date | End Date |  |
| `externalid` | select | External ID |  |
| `internalid` | select | Internal ID |  |
| `isinactive` | checkbox | Inactive |  |
| `item` | select | Lines - Item |  |
| `lastmodified` | date | Last Modified |  |
| `lastmodifiedby` | select | Last Modified By |  |
| `margintarget` | percent | Margin Target |  |
| `name` | text | Lines - Name |  |
| `project` | select | Project |  |
| `projecttask` | select | Lines - Project Task |  |
| `resource` | select | Lines - Resource |  |
| `resourceitem` | select | Lines - Resource Item |  |
| `startdate` | date | Start Date |  |
| `timelinetype` | select | Timeline Type |  |
| `vendor` | select | Lines - Vendor |  |

## Search joins

| Join ID | Label | Joined record |
|---|---|---|
| `job` | Job | Job |
| `projecttask` | Project Task | Project Task |

## Suspected source errors

> [!SOURCE-ERROR] `set_params_missing_get_lists_value_param`
<!-- source-error: record:wbs | - | set_params_missing_get_lists_value_param -->
