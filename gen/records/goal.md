# Goal (`goal`)
<!-- id: record:goal -->

| Fact | Value |
|---|---|
| Internal ID | `goal` |
| Category | list |
| Client scripts | yes |
| Server scripts | yes |
| Searchable | yes |
| Tags | `scripting_full` |

## Record facts

- `script_types`: `client`, `server`
- `requires_feature`: `Performance Management` [`real_name`]
- `subrecords`: `goalcomments`, `goalevaluations`
- `rule`: `copied_goal_status_planned`
- `script_functions`: `create`, `edit`, `copy`, `delete`, `search`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `details` | textarea | Details | no |  |  | `purpose`: `goal_details`; `cannot`: `change_after_approval` |
| `employee` | select | Employee | yes |  |  | `purpose`: `goal_creator` |
| `goalstatus` | select | Status | yes |  |  | `purpose`: `goal_status`; `allowed_values`: `Planned`, `In Progress`, `Completed`, `Not Completed`, `Canceled`; `default_value`: `Planned` (when `new_goal`); `editable_only_when`: `manager_approved`; `effect`: `in_progress_updates_actual_metric_value`; `option_meanings`: `planned:initial`, `in_progress:working_after_approval`, `completed:done`, `not_completed:missed_target_date`, `canceled:no_longer_relevant` |
| `isinactive` | checkbox | Inactive | no |  |  | `purpose`: `inactivate_goal`; `ui_effect`: `hidden_from_lists_and_searches`; `constraint`: `only_creator_can_inactivate` |
| `mood` | select | Mood | yes |  |  | `purpose`: `goal_mood`; `allowed_values`: `Not Set`, `On Track`, `Behind`, `At Risk`; `option_meanings`: `not_set:no_mood`, `on_track:on_track`, `behind:behind_may_achieve`, `at_risk:may_not_achieve` |
| `name` | text | Name | yes |  |  | `purpose`: `goal_name`; `cannot`: `change_after_approval` |
| `targetdate` | date | Target Completion Date | yes |  |  | `purpose`: `target_completion_date`; `required_when`: `performance_metric_selected` |
| `externalid` | text | External ID | no |  |  |  |
| `id` | integer | Internal ID | no |  |  |  |
| `internalid` | integer | Internal ID | no |  |  |  |
