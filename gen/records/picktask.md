# Pick Task (`picktask`)
<!-- id: record:picktask -->

| Fact | Value |
|---|---|
| Internal ID | `picktask` |
| Category | transaction |
| Client scripts | yes |
| Server scripts | yes |
| Searchable | yes |
| Tags | `no_copy`, `no_create`, `supports_delete`, `supports_search`, `supports_update` |

## Record facts

- `script_types`: `client`, `server`
- `requires_feature`: `Warehouse Management` [`real_name`]
- `created_by`: `wave_transactions_only`
- `subrecords`: `inventorydetail`
- `read_only_fields`: `wave`, `location`, `item`, `total_picked`, `total_remaining`
- `purpose`: `picking_and_staging_details_for_released_orders`
- `effect`: `item_fulfillments_generated_from_pick_tasks`
- `script_functions`: `update`, `delete`, `search`
- `not_supported`: `create`, `copy`
- `default_behavior`: `assigned_picker_from_wave`
- `caveat`: `picker_listed_only_if_warehouse_location_matches_wave`
- `displayed_on`: `multiple_units_of_measure_accounts` [`units`]
- `option_meanings`: `pending:wave_pending_release`, `ready:wave_released` [`status`]
- `effect`: `status_follows_line_items_after_release`
- `allowed_values_listed_in`: `mobile_device_entries` [`picked_quantity`, `staging_bin`, `inventory_detail`]
- `ui_effect`: `transaction_number_links_fulfillment_for_done_lines`
- `configured_by`: `multiple_shipping_routes_feature` [`ship_to`, `ship_via`]
- `displayed_on`: `failed_lines_with_allow_retry_yes` [`override_fulfillment_creation`]
- `configured_by`: `show_additional_pick_task_columns_preference` [`user_field_1`, `user_field_2`]

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `item` | select | Item | no |  |  | `purpose`: `item`; `value_source`: `system_displayed` |
| `location` | select | Location | no |  |  | `purpose`: `warehouse_location`; `value_source`: `system_displayed` |
| `memo` | textarea | Notes | no |  |  | `purpose`: `memo` |
| `name` | integer | Pick Task # | no |  |  | `purpose`: `pick_task_number`; `default_behavior`: `auto_generated_per_wave` |
| `picker` | select | Assigned Picker | no |  |  | `purpose`: `picker` |
| `recommendedbin` | select | Recommended Bin | no |  |  | `purpose`: `recommended_bin`; `value_source`: `first_sorted_bin_with_item` |
| `status` | select | Status | no |  |  | `purpose`: `pick_task_status`; `value_source`: `pick_task_line_statuses` |
| `totalpickedquantity` | float | Total Picked Quantity | no |  |  | `purpose`: `total_picked_quantity`; `value_source`: `system_displayed` |
| `totalremainingquantity` | float | Total Remaining Quantity | no |  |  | `purpose`: `total_remaining_quantity`; `value_source`: `system_displayed` |
| `units` | select | Units | no |  |  | `purpose`: `unit_of_measure`; `value_source`: `system_displayed` |
| `wave` | select | Wave # | no |  |  | `purpose`: `wave_transaction_number`; `value_source`: `system_displayed` |
| `externalid` | text | External ID | no |  |  |  |
| `inventorynumber` | select | Serial/Lot Number | no |  |  |  |

## Sublist `pickactions` (Line Items)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `enddate` | date | End Date | no |  |  |
| `linenumber` | select | Line # | no |  |  |
| `ordernumber` | select | Order # | no |  |  |
| `picktask` | select | Pick Task | no |  |  |
| `quantity` | float | Quantity | no |  |  |
| `stagingbin` | select | Staging Bin | no |  |  |
| `startdate` | date | Start Date | no |  |  |
| `status` | select | Status | no |  |  |
| `transactionnumber` | select | Transaction # | no |  |  |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `externalid` | select | External ID |  |
| `internalid` | select | Internal ID |  |
| `inventorynumber` | select | Serial/Lot Number |  |
| `item` | select | Item |  |
| `location` | select | Location |  |
| `memo` | text | Memo |  |
| `name` | integer | Pick Task # |  |
| `picker` | select | Assigned Picker |  |
| `quantity` | float | Line Item Quantity |  |
| `recommendedbin` | select | Recommended Bin |  |
| `stagingbin` | select | Line Item Staging Bin |  |
| `status` | select | Pick Task Status |  |
| `totalpickedquantity` | float | Total Picked Quantity |  |
| `totalremainingquantity` | float | Total Remaining Quantity |  |
| `units` | select | Units |  |
| `wavename` | select | Wave # |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `externalid` | select | External ID |  |
| `internalid` | select | Internal ID |  |
| `inventorynumber` | select | Serial/Lot Number |  |
| `item` | select | Item |  |
| `location` | select | Location |  |
| `memo` | text | Memo |  |
| `name` | integer | Pick Task # |  |
| `picker` | select | Assigned Picker |  |
| `quantity` | float | Line Item Quantity |  |
| `recommendedbin` | select | Recommended Bin |  |
| `stagingbin` | select | Line Item Staging Bin |  |
| `status` | select | Pick Task Status |  |
| `totalpickedquantity` | float | Total Picked Quantity |  |
| `totalremainingquantity` | float | Total Remaining Quantity |  |
| `units` | select | Units |  |
| `wavename` | text | Wave # |  |
