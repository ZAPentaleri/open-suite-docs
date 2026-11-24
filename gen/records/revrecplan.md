# Revenue Recognition Plan (`revrecplan`)
<!-- id: record:revrecplan -->

| Fact | Value |
|---|---|
| Internal ID | `revrecplan` |
| Category | list |
| Client scripts | no |
| Server scripts | yes |
| User event scripts | no |
| Tags | `no_copy`, `no_create`, `no_delete` |

## Record facts

- `not_supported`: `create`, `copy`, `delete`, `transform`
- `script_functions`: `read`, `update`, `search`
- `requires_feature`: `Accounting Periods`, `Advanced Revenue Management (Essentials)` [`real_name`]
- `script_types`: `server`
- `user_events_supported`: `false`
- `sublists`: `plannedrevenue:inline_editor`
- `sublist_fields`: `amount`, `plannedperiod`
- `purpose`: `periods_and_amounts_of_revenue_recognition`
- `effect`: `derived_from_revenue_recognition_rules`
- `contains`: `forecast_plan`, `actual_plans` [`per_revenue_element`]
