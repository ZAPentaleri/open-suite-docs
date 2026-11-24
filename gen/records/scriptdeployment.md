# Script Deployment (`scriptdeployment`)
<!-- id: record:scriptdeployment -->

| Fact | Value |
|---|---|
| Internal ID | `scriptdeployment` |
| Category | customization |
| Client scripts | no |
| Server scripts | yes |
| Searchable | yes |

## Record facts

- `script_types`: `server`
- `script_types`: `server`
- `not_scriptable_fields`: `recurrenceminutes`, `starttime`
- `not_scriptable`: `change_id_button`, `history_subtab`, `system_notes_subtab`, `execution_log_subtab`, `scheduling_subtab`
- `read_only_subtabs`: `parameters`
- `requires`: `script_owner_role_with_suitescript_permission`
- `rule`: `bundle_scripts_access_only_same_bundle_deployments`
- `not_supported_in`: `client_scripts`
- `caveat`: `scripts_reach_deployments_only_in_same_bundle`
- `requires`: `numeric_internal_id_not_customdeploy_script_id` (when `id_parameter`)
- `ui_location`: `script_deployments_list_internal_id_column`
- `configured_by`: `show_internal_ids_set_preference` (when `showing_internal_id_column`)
- `value_examples`: `119:customdeploy_newsfeed_email`
- `initialization_default_value`: `script`, `script_internal_id` (refs `N/record.create`, `N/record.copy`, `N/record.transform`, `N/record.load`)
- `script_functions`: `create`, `read`, `copy`, `update`, `delete`, `search`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `allemployees` | checkbox | Select All | no | no |  | `purpose`: `run_for_all_employees`; `true_means`: `executes_for_all_employees` |
| `allpartners` | checkbox | Select All | no | no |  | `purpose`: `run_for_all_partners`; `true_means`: `executes_for_all_partners` |
| `allroles` | checkbox | Select All | no | no |  | `purpose`: `run_for_all_internal_roles`; `true_means`: `executes_for_all_internal_roles` |
| `auddepartment` | select | Departments | no | no |  | `purpose`: `deployment_audience_departments`; `value_kind`: `multi_select` |
| `audemployee` | select | Employees | no | no |  | `purpose`: `deployment_audience_employees`; `value_kind`: `multi_select` |
| `audgroup` | select | Groups | no | no |  | `purpose`: `deployment_audience_groups`; `value_kind`: `multi_select` |
| `audpartner` | select | Partners | no | no |  | `purpose`: `deployment_audience_partners`; `value_kind`: `multi_select` |
| `audslctrole` | select | Roles | no | no |  | `purpose`: `deployment_audience_internal_roles`; `value_kind`: `multi_select` |
| `audsubsidiary` | select | Subsidiaries | no | no |  | `purpose`: `deployment_audience_subsidiaries`; `value_kind`: `multi_select`; `requires_feature`: `OneWorld` [`real_name`] |
| `externalurl` | text | External URL | no | no |  | `purpose`: `external_url`; `requires`: `all_roles_and_no_other_audience` (when `suitelet_without_login`); `constraint`: `logged_in_users_only` (when `restlet`) |
| `isdeployed` | checkbox | Deployed | no | no |  | `purpose`: `deployed`; `false_means`: `script_does_not_run` |
| `isonline` | checkbox | Available Without Login | no | no |  | `purpose`: `available_without_login`; `required_when`: `suitelet_external_redirects`; `effect`: `no_session_needed`; `requires_feature`: `Website` (when `client_scripts_in_external_suitelet`) [`real_name`]; `risk`: `suitelet_data_less_secure` |
| `loglevel` | select | Log Level | no | no |  | `purpose`: `log_level`; `options`: `Debug`, `Audit`, `Error`, `Emergency`; `option_meanings`: `debug:testing_all_levels`, `audit:production_events`, `error:unexpected_errors_only`, `emergency:most_critical_only` |
| `recurrenceminutes` | select | Repeat | no | no |  | `purpose`: `queue_recurrence`; `effect`: `requeued_every_n_hours_until_midnight` |
| `script` | select | Script | no | no |  | `purpose`: `script`; `allowed_values`: `Suitelet`, `RESTlet`, `User Event`, `Scheduled`, `Map/Reduce`, `Client`, `Portlet`, `Mass Update`, `Workflow Action`, `Bundle Installation`; `caveat`: `client_means_record_level_deployed_client_script` |
| `scriptid` | identifier | ID | no | no |  | `purpose`: `script_id`; `default_behavior`: `system_generated` (when `blank`); `required_prefix`: `customscript` [`added_on_save`]; `required_prefix`: `customdeploy` [`deployments`]; `format`: `lowercase_no_spaces`; `recommendation`: `custom_id_when_bundling`; `can`: `change_with_change_id_button`; `value_examples`: `employeeupdates_becomes_customdeployemployeeupdates`; `reason`: `custom_ids_avoid_conflicts_across_accounts` |
| `starttime` | select | Start Time | no | no |  | `purpose`: `queue_start_time`; `caveat`: `time_queued_not_time_executed` |
| `status` | select | Status | yes | no |  | `purpose`: `deployment_status` |
| `title` | text | Title | yes | no |  | `purpose`: `deployment_name` |
| `url` | text | URL | no | no |  | `purpose`: `internal_suitelet_url`; `value_source`: `system_displayed` |
| `externalid` | text | ExternalId | no | no |  |  |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `eventtype` | select | Event Type |  |
| `formuladate` | date | Formula (Date) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulatext` | text | Formula (Text) |  |
| `internalid` | select | Internal ID |  |
| `internalidnumber` | integer | Internal ID (Number) |  |
| `isdeployed` | checkbox | Is Deployed |  |
| `loglevel` | select | Log Level |  |
| `queueid` | integer | Queue |  |
| `recordtype` | select | Record Type |  |
| `script` | select | Script ID |  |
| `scriptid` | text | Custom ID |  |
| `status` | select | Status |  |
| `title` | text | Name |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `eventtype` | select | Event Type |  |
| `formulacurrency` | currency | Formula (Currency) |  |
| `formuladate` | date | Formula (Date) |  |
| `formuladatetime` | datetime | Formula (Date/Time) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulapercent` | percent | Formula (Percent) |  |
| `formulatext` | text | Formula (Text) |  |
| `internalid` | select | Internal ID |  |
| `isdeployed` | checkbox | Is Deployed |  |
| `loglevel` | select | Log Level |  |
| `queueid` | text | Queue |  |
| `recordtype` | select | Record Type |  |
| `script` | select | Script ID |  |
| `scriptid` | text | Custom ID |  |
| `scripttype` | select | Script Type |  |
| `status` | select | Status |  |
| `title` | text | Title |  |

## Search joins

| Join ID | Label | Joined record |
|---|---|---|
| `script` | Script | Script |
| `user` | User | Employee |
