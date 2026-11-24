# Wave (`wave`)
<!-- id: record:wave -->

| Fact | Value |
|---|---|
| Internal ID | `wave` |
| Category | transaction |
| Client scripts | yes |
| Server scripts | yes |
| Searchable | yes |
| Search data via | Transaction |
| Tags | `supports_create`, `supports_delete`, `supports_read`, `supports_search`, `supports_update` |

## Record facts

- `script_types`: `client`, `server`
- `script_functions`: `create`, `update`, `delete`, `search`
- `requires_feature`: `Warehouse Management` [`real_name`]
- `script_functions`: `create`, `update`, `delete`, `search`
- `search_supports`: `search_create_not_search_load`
- `read_only_fields`: `wave_number`, `wave_type`, `location`, `created_date`, `released_date`, `completed_date`
- `not_updatable_when`: `picking_type_and_status:released_or_later`
- `purpose`: `consolidate_orders_for_warehouse_release`
- `effect`: `release_makes_items_pickable_on_mobile`
- `displayed_on`: `edit_mode_only` [`customform`]
- `allowed_values_listed_in`: `wave_criteria_transaction_type` [`wave_type`]
- `displayed_on`: `released_status` [`released_date`]
- `displayed_on`: `completed_status_only` [`completed_date`]
- `displayed_on`: `pending_release_or_released` [`pick_tasks_subtab`]
- `displayed_on`: `pending_release_only` [`assign_selected_pick_tasks_to`]
- `ui_effect`: `custom_subtab_holds_unplaced_fields`
- `editable_only_when`: `pending_release` [`assigned_picker_column`]
- `not_supported`: `assigned_picker_on_create`
- `same_as`: `pick_task_status_and_recommended_bin` [`pick_task_sublist_columns`]

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `createddate` | datetime | Date Created | no |  |  | `purpose`: `wave_created_date`; `value_source`: `system_displayed` |
| `location` | select | Location | no |  |  | `purpose`: `wave_location`; `value_source`: `system_displayed` |
| `name` | text | Wave # | no |  |  | `purpose`: `wave_number`; `value_source`: `auto_generated` |
| `newwavestatus` | select | Status | no |  |  | `purpose`: `wave_status`; `options`: `Pending Release`, `Released`; `option_meanings`: `pending_release:pick_tasks_generated_saved`, `released:pick_tasks_generated_released` |
| `picktype` | select | Picking Type | no |  |  | `purpose`: `pick_type`; `options`: `Single Order`, `Multiple Order`, `Bulk Picking`; `option_meanings`: `single:task_per_order_line`, `multiple:combined_items_per_order`, `bulk:whole_quantity_distributed_by_app` |
| `priority` | select | Priority | no |  |  | `purpose`: `wave_priority`; `allowed_range`: `1`, `9`; `option_meanings`: `1:top`, `9:lowest`; `displayed_on`: `mobile_list_order` |
| `releaseddate` | date | Released Date | no |  |  | `purpose`: `wave_release_date`; `value_source`: `system_displayed` |
| `status` | select | Status | no |  |  | `purpose`: `wave_status_display`; `value_source`: `system_displayed` |
| `wavetype` | select | Wave Type | no |  |  | `purpose`: `wave_order_type`; `value_source`: `system_displayed` |
| `completeddate` | date | Completed Date | no |  |  |  |
| `createdby` | select | Created By | no |  |  |  |
| `externalid` | text | External ID | no |  |  |  |
| `searchtemplateid` | identifier | Search Template ID | no | no |  |  |
| `subsidiary` | select | subsidiary | no |  |  |  |
| `totalassigned` | integer | Assigned Pick Tasks | no |  |  |  |
| `totalorderlines` | integer | Line Items Selected | no |  |  |  |
| `totalorderlinesfailed` | integer | Line Items Failed | no |  |  |  |
| `totalorderlinesreleased` | integer | Line Items Released | no |  |  |  |
| `totalpicktasks` | integer | Total Pick Tasks Created | no |  |  |  |
| `totalunassigned` | integer | Unassigned Pick Tasks | no |  |  |  |
| `waveorderlimit` | posinteger | Wave Order Limit | no | no |  |  |

## Sublist `lineitems` (Line Item)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `line` | select | Line # | no |  |  |
| `order` | select | Order # | no |  |  |
