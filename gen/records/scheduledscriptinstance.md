# Scheduled Script Instance (`scheduledscriptinstance`)
<!-- id: record:scheduledscriptinstance -->

| Fact | Value |
|---|---|
| Internal ID | `scheduledscriptinstance` |
| Category | customization |
| Client scripts | no |
| Server scripts | yes |
| Searchable | yes |
| Tags | `custom_fields_supported` |

## Record facts

- `script_types`: `server`
- `user_events`: `beforeLoad`, `beforeSubmit`, `afterSubmit`
- `script_types`: `server`
- `user_events`: `beforeLoad`, `beforeSubmit`, `afterSubmit`
- `covers`: `scheduled_script_instances`, `map_reduce_script_instances`
- `script_functions`: `search`
- `not_supported_in`: `client_scripts`
- `displayed_on`: `scheduled_script_status_page`, `map_reduce_script_status_page`
- `ui_location`: `customization_scripting_scheduled_script_status`
- `ui_location`: `customization_scripting_map_reduce_script_status`
- `ui_effect`: `details_link_shows_map_reduce_jobs_of_instance`
- `difference`: `scheduled_instance_one_job_map_reduce_instance_many_jobs`
- `value_examples`: `search_in_progress_instances_joined_to_script_and_deployment`
- `value_source`: `priority_when_job_created` [`search_column_originalpriority`]
- `options`: `low`, `standard`, `high` [`search_column_originalpriority`]
- `caveat`: `jobs_created_during_processing_inherit_creator_priority` [`search_column_originalpriority`]
- `cannot`: `group_search_results_by_text_priority` [`search_column_originalpriority`]
- `use_instead`: `originalprioritynumber` (when `grouping_search_results_by_priority`) [`search_column_originalpriority`]
- `option_meanings`: `1:high`, `2:standard`, `3:low` [`search_column_originalprioritynumber`]
- `value_kind`: `time_current_priority_set` [`search_column_prioritytimestamp`]
- `caveat`: `user_priority_changes_leave_existing_jobs_unchanged` [`search_column_prioritytimestamp`]
- `effect`: `system_priority_elevation_updates_timestamp` [`search_column_prioritytimestamp`]

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `datecreated` | datetime | Date Created |  |
| `enddate` | datetime | End Date |  |
| `formuladate` | date | Formula (Date) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulatext` | text | Formula (Text) |  |
| `percentcomplete` | percent | Percent Complete |  |
| `queue` | text | Queue |  |
| `startdate` | datetime | Start Date |  |
| `status` | select | Status |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `datecreated` | datetime | Date Created |  |
| `enddate` | datetime | End Date |  |
| `formulacurrency` | currency | Formula (Currency) |  |
| `formuladate` | date | Formula (Date) |  |
| `formuladatetime` | datetime | Formula (Date/Time) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulapercent` | percent | Formula (Percent) |  |
| `formulatext` | text | Formula (Text) |  |
| `percentcomplete` | percent | Percent Complete |  |
| `queue` | text | Queue |  |
| `queueposition` | integer | Queue Position |  |
| `startdate` | datetime | Start Date |  |
| `status` | text | Status |  |

## Search joins

| Join ID | Label | Joined record |
|---|---|---|
| `script` | Script | Script |
| `scriptdeployment` | Script Deployment | Script Deployment |
