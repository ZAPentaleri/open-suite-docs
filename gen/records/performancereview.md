# Performance Review (`performancereview`)
<!-- id: record:performancereview -->

| Fact | Value |
|---|---|
| Internal ID | `performancereview` |
| Category | list |
| Client scripts | yes |
| Server scripts | yes |
| Tags | `no_copy` |

## Record facts

- `script_types`: `client`, `server`
- `requires_feature`: `Performance Management` [`real_name`]
- `subrecords`: `performancereviewanswer`, `goal`
- `script_functions`: `create`, `edit`, `delete`, `search`
- `not_updatable_fields`: `goals`, `employeeSubmittedDate`, `reviewSubmittedDate`
- `role_limits`: `hr_generalist:no_answers_delete_only_launched_or_canceled`, `employee:answers_and_ratings_only`, `manager:no_date_edits`
- `editable_only_when`: `status_hr_approved_sign_off_required` [`employee_acknowledgment`]
