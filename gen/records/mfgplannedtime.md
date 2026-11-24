# Manufacturing Planned Time (`mfgplannedtime`)
<!-- id: record:mfgplannedtime -->

| Fact | Value |
|---|---|
| Internal ID | `mfgplannedtime` |
| Category | transaction |
| Client scripts | yes |
| Server scripts | yes |
| Searchable | yes |
| Tags | `custom_fields_supported`, `search_only`, `supports_search` |

## Record facts

- `script_types`: `client`, `server`
- `script_functions`: `search`
- `not_supported`: `create`, `copy`
- `script_types`: `client`, `server`
- `requires_preference`: `show_planned_capacity_on_work_orders`
- `purpose`: `search_work_order_planned_time_subtab_data`
- `value_source`: `derived_from_operation_tasks`
- `configured_by`: `accounting_preferences_show_planned_capacity_on_work_orders`
- `requires_feature`: `Manufacturing Routing and Work Center` (when `preference_available`) [`real_name`]
- `ui_location`: `reports_new_search_manufacturing_planned_time`

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `date` | date | Date |  |
| `duration` | float | Duration |  |
| `externalid` | select | External ID |  |
| `externalidstring` | text | External ID (Text) |  |
| `manufacturingworkcenter` | select | Manufacturing Work Center |  |
| `workorder` | select | Work Order |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `date` | date | Date |  |
| `duration` | text | Duration |  |
| `durationdecimal` | float | Duration (Decimal) |  |
| `externalid` | select | External ID |  |
| `manufacturingworkcenter` | text | Manufacturing Work Center |  |
| `operation` | text | Operation |  |

## Search joins

| Join ID | Label | Joined record |
|---|---|---|
| `manufacturingworkcenter` | Manufacturing Work Center | Group |
| `operationtask` | Operation Task | Manufacturing Operation Task |
