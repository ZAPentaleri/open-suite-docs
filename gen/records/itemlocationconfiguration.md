# Item Location Configuration (`itemlocationconfiguration`)
<!-- id: record:itemlocationconfiguration -->

| Fact | Value |
|---|---|
| Internal ID | `itemlocationconfiguration` |
| Category | list |
| Client scripts | no |
| Server scripts | yes |
| Searchable | yes |
| Tags | `custom_fields_supported`, `supports_create`, `supports_delete`, `supports_read`, `supports_search`, `supports_update` |

## Record facts

- `script_types`: `client`, `server`
- `not_supported`: `copy`, `transform`
- `script_functions`: `create`, `read`, `edit`, `delete`, `search`
- `requires_feature`: `Multi-Location Inventory`, `Advanced Inventory Management`, `Advanced Item Location Configuration` [`real_name`]
- `script_functions`: `create`, `read`, `edit`, `delete`, `search`
- `not_supported`: `copy`, `transform`
- `purpose`: `item_attributes_per_location`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `advancewarningthreshold` | integer | Firm/Release Advance Warning Threshold | no |  |  | `purpose`: `planned_order_warning_threshold` |
| `atpleadtime` | integer | ATP Lead Time | no |  |  | `purpose`: `atp_lead_time`; `effect`: `ship_date_when_no_inventory` |
| `backwardconsumptiondays` | integer | Backward Consumption Days | no |  |  | `purpose`: `forecast_consumption_window_days`; `applies_when`: `forecast_consumption_demand_source`; `stage_order`: `backward_window`, `forward_window`; `constraint`: `only_sales_orders_and_invoices_consume`; `not_supported_for`: `transfer_orders`, `work_orders`; `help_text_shared_with`: `itemlocationconfiguration.forwardconsumptiondays` |
| `buildtime` | float | Work Order Variable Lead Time | no |  |  | `purpose`: `production_build_time`; `value_unit`: `days`; `applies_to`: `one_base_unit`; `can`: `auto_calculate_production_lead_time` |
| `cost` | currency2 | Standard Cost | no |  |  | `purpose`: `purchase_price`; `default_behavior`: `latest_purchase_price` (when `blank`); `value_unit`: `preferred_vendor_currency`; `value_unit`: `base_currency` (when `no_preferred_vendor`) |
| `costinglotsize` | float | Costing Lot Size | no |  |  | `purpose`: `costing_lot_size`; `default_value`: `1`; `min_value`: `0.01` |
| `customform` | select | Custom Form | no |  |  | `purpose`: `entry_form`; `allowed_values_listed_in`: `standard_and_custom_entry_forms`; `ui_effect`: `new_option_creates_record` |
| `defaultreturncost` | currency2 | Default Return Cost | no |  |  | `purpose`: `default_return_cost`; `effect`: `defaults_item_receipt_override_rate`; `can`: `change_on_receipt` |
| `demandsource` | select | Demand Source | no |  |  | `purpose`: `demand_source`; `options`: `Forecast from Demand Plan`, `Entered and Planned Orders`, `Order and Forecast`, `Forecast Consumption`; `option_meanings`: `demand_plan:item_demand_plan_only`, `entered_and_planned:open_orders_by_ship_date`, `order_and_forecast:forecast_plus_orders`, `forecast_consumption:forecast_minus_orders` |
| `demandtimefence` | integer | Demand Time Fence | no |  |  | `purpose`: `demand_time_fence`; `allowed_range`: `0`, `365` days; `default_behavior`: `default_demand_time_fence_setting` |
| `fixedlotsize` | posfloat | Fixed Lot Size | no |  |  | `purpose`: `fixed_lot_size`; `applies_when`: `lot_sizing_fixed_lot_size` |
| `forwardconsumptiondays` | integer | Forward Consumption Days | no |  |  | `purpose`: `forecast_consumption_window_days`; `applies_when`: `forecast_consumption_demand_source`; `stage_order`: `backward_window`, `forward_window`; `constraint`: `only_sales_orders_and_invoices_consume`; `not_supported_for`: `transfer_orders`, `work_orders`; `help_text_shared_with`: `itemlocationconfiguration.backwardconsumptiondays` |
| `inventorycosttemplate` | select | Inventory Cost Template | no |  |  | `purpose`: `inventory_cost_template` |
| `invtclassification` | select | Classification | no |  |  | `purpose`: `inventory_count_classification`; `value_source`: `system_displayed` |
| `invtcountinterval` | posinteger | Count Interval | no |  |  | `purpose`: `count_interval_days`; `value_unit`: `days`; `value_examples`: `30_day_intervals` |
| `isautolocassignmentallowed` | checkbox | Auto Loc. Assignment Allowed | no |  |  | `purpose`: `allow_automatic_location_assignment`; `default_value`: `true`; `false_means`: `location_never_auto_assigned`; `doc_inconsistency`: `says_clear_box_to_reenable` |
| `isautolocassignmentsuspended` | checkbox | Auto Loc. Assignment Suspended | no |  |  | `purpose`: `suspend_automatic_location_assignment`; `use_when`: `inventory_discrepancies_at_location`; `false_means`: `assignment_allowed_again` |
| `item` | select | Item | yes |  |  | `purpose`: `target_items_for_attribute_copy`; `value_kind`: `multi_select`; `doc_inconsistency`: `inventory_detail_help_describes_attribute_copy`; `caveat`: `extraneous_fields_copied_but_unused` |
| `latedemandthreshold` | float | High Impact Late Demand Threshold | no |  |  | `purpose`: `late_demand_threshold` |
| `leadtime` | integer | Lead Time | no |  |  | `purpose`: `lead_time_days`; `value_unit`: `days`; `value_source`: `average_of_three_latest_purchase_orders` (when `auto_calculate`); `caveat`: `order_analysis_interval_when_over_three_orders`; `caveat`: `last_receipt_per_order_used`; `caveat`: `not_weighted_by_units_received`; `caveat`: `orders_without_receipts_ignored`; `default_behavior`: `inventory_management_setup_value` (when `blank_and_not_auto`) |
| `location` | select |  | yes |  |  | `purpose`: `item_location`; `requires_feature`: `Multi-Location Inventory` (when `tracking_inventory_by_location`) [`real_name`]; `effect`: `limits_role_access_to_item`; `value_examples`: `role_limited_to_location_one_items`; `use_instead`: `locations_sublist` (when `tracking_inventory_per_location`); `ui_effect`: `new_option_creates_record` |
| `locationallowstorepickup` | checkbox | Allow Store Pickup | no |  |  | `purpose`: `allow_store_pickup`; `default_value`: `true` |
| `locationstorepickupbufferstock` | integer | Buffer Stock | no |  |  | `purpose`: `store_pickup_buffer_stock`; `effect`: `reduces_quantity_available_for_pickup`; `constraint`: `pickup_orders_need_available_above_zero`; `applies_to`: `all_items_at_location`; `help_text_shared_with`: `location.storepickupbufferstock` |
| `memo` | textarea | Memo | no |  |  | `purpose`: `memo`; `supported_in`: `search` |
| `minimumrescheduledays` | integer | Minimum Reschedule Days | no |  |  | `purpose`: `minimum_reschedule_days` |
| `name` | text | Name | yes |  |  | `purpose`: `name`; `doc_inconsistency`: `help_says_item_name` |
| `nextinvtcountdate` | date | Next Count Date | no |  |  | `purpose`: `next_inventory_count_date` |
| `pastdueforecastdays` | integer | Past Due Forecast Days | no |  |  | `purpose`: `past_due_forecast_search_days` |
| `periodiclotsizedays` | integer | Periods of Supply Increment | no |  |  | `purpose`: `periods_of_supply_increment`; `allowed_range`: `1`, `90`; `default_value`: `1`; `editable_only_when`: `periodic_lot_size_type_interval` |
| `periodiclotsizetype` | select | Periods of Supply Type | no |  |  | `purpose`: `periodic_lot_size_type`; `doc_inconsistency`: `help_describes_supply_type` |
| `preferredstocklevel` | float | Preferred Stock Level | no |  |  | `purpose`: `preferred_stock_level`; `value_source`: `daily_demand_times_days_supply` (when `auto_calculate`); `default_behavior`: `inventory_management_setup_value` (when `not_set`) |
| `reorderpoint` | float | Reorder Point | no |  |  | `purpose`: `reorder_point`; `value_source`: `lead_time_days_times_demand` (when `no_safety_stock`); `value_source`: `lead_plus_safety_days_times_demand` (when `safety_stock_days`); `value_source`: `lead_days_times_demand_plus_safety_qty` (when `safety_stock_quantity`); `configured_by`: `inventory_level_warnings_preference` |
| `rescheduleindays` | integer | Reschedule In Days | no |  |  | `purpose`: `max_days_order_can_be_advanced`; `allowed_range`: `1`, `90` days; `default_behavior`: `blank`; `caveat`: `blank_means_no_reschedule_recommendations`; `value_examples`: `10_allows_10_days_earlier_not_11` |
| `rescheduleoutdays` | integer | Reschedule Out Days | no |  |  | `purpose`: `max_days_order_can_be_delayed`; `allowed_range`: `1`, `180` days; `default_behavior`: `blank`; `caveat`: `blank_means_no_reschedule_recommendations`; `value_examples`: `10_allows_10_days_later_not_11` |
| `safetystocklevel` | float | Safety Stock Level | no |  |  | `purpose`: `safety_stock`; `accepts_forms`: `quantity`, `days`; `effect`: `used_in_reorder_point_calculation`; `default_behavior`: `inventory_management_setup_value` (when `not_set`); `help_text_shared_with`: `assemblyitem.safetystockleveldays`, `inventoryitem.safetystockleveldays`, `lotnumberedassemblyitem.safetystockleveldays`, `lotnumberedinventoryitem.safetystockleveldays`, `serializedassemblyitem.safetystockleveldays`, `serializedinventoryitem.safetystockleveldays` |
| `subsidiary` | select |  | yes |  |  | `purpose`: `item_subsidiaries`; `value_kind`: `multi_select`; `constraint`: `item_usable_only_on_its_subsidiaries_transactions`; `value_examples`: `only_wolfe_us_items_on_wolfe_us_orders` |
| `supplylotsizingmethod` | select | Lot Sizing Method | no |  |  | `purpose`: `lot_sizing_method`; `options`: `Lot For Lot`, `Fixed Lot Size`, `Periods of Supply`; `option_meanings`: `lot_for_lot:exact_daily_projection`, `fixed_lot_size:fixed_or_multiple`, `periods_of_supply:aggregated_orders_per_period`; `caveat`: `consolidated_bom_reused_on_lower_levels` |
| `supplytimefence` | integer | Planning Time Fence | no |  |  | `purpose`: `planning_time_fence`; `allowed_range`: `0`, `365` days; `default_behavior`: `default_planning_time_fence_setting` |
| `supplytype` | select | Supply Type | no |  |  | `purpose`: `supply_type`; `options`: `Purchase`, `Assembly`; `configured_by`: `allow_purchase_of_assembly_items` [`assembly_items`] |
| `externalid` | text | ExternalId | no |  |  |  |
| `id` | integer | Internal ID | no |  |  |  |
| `iswip` | checkbox | WIP | no |  |  |  |
| `poreceiptcost` | currency2 | PO Receipt Cost | no |  |  |  |
| `savedleadtime` | integer |  | no |  |  |  |
| `savedpreferredstocklevel` | float |  | no |  |  |  |
| `savedreorderpoint` | float | Reorder Point | no |  |  |  |
| `templatestored` | checkbox |  | no |  |  |  |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `advancewarningthreshold` | integer | Firm/Release Advance Warning Threshold |  |
| `atpleadtime` | integer | ATP Lead Time |  |
| `backwardconsumptiondays` | integer | Backward Consumption Days |  |
| `buildtime` | float | Work Order Lead Time |  |
| `cost` | currency2 | Standard Cost |  |
| `costinglotsize` | float | Costing Lot Size |  |
| `defaultreturncost` | currency2 | Default Return Cost |  |
| `demandsource` | select | Demand Source |  |
| `demandtimefence` | integer | Demand Time Fence |  |
| `externalid` | select | External ID |  |
| `externalidstring` | text | External ID (Text) |  |
| `fixedlotsize` | float | Fixed Lot Size |  |
| `forwardconsumptiondays` | integer | Forward Consumption Days |  |
| `internalid` | select | Internal ID |  |
| `internalidnumber` | integer | Internal ID (Number) |  |
| `inventorycosttemplate` | select | Inventory Cost Template |  |
| `invtclassification` | select | Inventory Classification |  |
| `invtcountinterval` | integer | Count Interval |  |
| `isautolocassignmentallowed` | checkbox | Auto Loc. Assignment Allowed |  |
| `isautolocassignmentsuspended` | checkbox | Auto Loc. Assignment Suspended |  |
| `iswip` | checkbox | WIP |  |
| `item` | select | Item |  |
| `latedemandthreshold` | float | High Impact Late Demand Threshold |  |
| `leadtime` | integer | Lead Time |  |
| `location` | select | Location |  |
| `locationallowstorepickup` | checkbox | Allow Store Pickup |  |
| `locationstorepickupbufferstock` | integer | Buffer Stock |  |
| `memo` | text | Memo |  |
| `minimumrescheduledays` | integer | Minimum Reschedule Days |  |
| `name` | text | Name |  |
| `nextinvtcountdate` | date | Next Count Date |  |
| `pastdueforecastdays` | integer | Past Due Forecast Days |  |
| `periodiclotsizedays` | integer | Periods of Supply Increment |  |
| `periodiclotsizetype` | select | Periods of Supply Type |  |
| `poreceiptcost` | currency2 | PO Receipt Cost |  |
| `preferredstocklevel` | float | Preferred Stock Level |  |
| `reorderpoint` | float | Reorder Point |  |
| `rescheduleindays` | integer | Reschedule In Days |  |
| `rescheduleoutdays` | integer | Reschedule Out Days |  |
| `safetystocklevel` | float | Safety Stock Level |  |
| `subsidiary` | select | Subsidiary |  |
| `supplylotsizingmethod` | select | Lot Sizing Method |  |
| `supplytimefence` | integer | Planning Time Fence |  |
| `supplytype` | select | Supply Type |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `advancewarningthreshold` | integer | Firm/Release Advance Warning Threshold |  |
| `atpleadtime` | integer | ATP Lead Time |  |
| `backwardconsumptiondays` | integer | Backward Consumption Days |  |
| `buildtime` | float | Work Order Lead Time |  |
| `cost` | currency2 | Standard Cost |  |
| `costinglotsize` | float | Costing Lot Size |  |
| `defaultreturncost` | currency2 | Default Return Cost |  |
| `demandsource` | text | Demand Source |  |
| `demandtimefence` | integer | Demand Time Fence |  |
| `externalid` | select | External ID |  |
| `fixedlotsize` | float | Fixed Lot Size |  |
| `forwardconsumptiondays` | integer | Forward Consumption Days |  |
| `internalid` | select | Internal ID |  |
| `inventorycosttemplate` | text | Inventory Cost Template |  |
| `invtclassification` | text | Inventory Classification |  |
| `invtcountinterval` | integer | Inventory Count Interval |  |
| `isautolocassignmentallowed` | checkbox | Auto Loc. Assignment Allowed |  |
| `isautolocassignmentsuspended` | checkbox | Auto Loc. Assignment Suspended |  |
| `iswip` | checkbox | WIP |  |
| `item` | select | Item |  |
| `latedemandthreshold` | float | High Impact Late Demand Threshold |  |
| `leadtime` | integer | Lead Time |  |
| `location` | select | Location |  |
| `locationallowstorepickup` | checkbox | Allow Store Pickup |  |
| `locationstorepickupbufferstock` | integer | Buffer Stock |  |
| `memo` | text | Memo |  |
| `minimumrescheduledays` | integer | Minimum Reschedule Days |  |
| `name` | text | Name |  |
| `nextinvtcountdate` | date | Next Inventory Count Date |  |
| `pastdueforecastdays` | integer | Past Due Forecast Days |  |
| `periodiclotsizedays` | integer | Periods of Supply Increment |  |
| `periodiclotsizetype` | text | Periods of Supply Type |  |
| `poreceiptcost` | currency2 | PO Receipt Cost |  |
| `preferredstocklevel` | float | Preferred Stock Level |  |
| `reorderpoint` | float | Reorder Point |  |
| `rescheduleindays` | integer | Reschedule In Days |  |
| `rescheduleoutdays` | integer | Reschedule Out Days |  |
| `safetystocklevel` | float | Safety Stock Level |  |
| `subsidiary` | select | Subsidiary |  |
| `supplylotsizingmethod` | text | Lot Sizing Method |  |
| `supplytimefence` | integer | Planning Time Fence |  |
| `supplytype` | text | Supply Type |  |

## Suspected source errors

> [!SOURCE-ERROR] `says_clear_box_to_reenable` (on `isautolocassignmentallowed`)
<!-- source-error: record:itemlocationconfiguration | isautolocassignmentallowed | says_clear_box_to_reenable -->

> [!SOURCE-ERROR] `inventory_detail_help_describes_attribute_copy` (on `item`)
<!-- source-error: record:itemlocationconfiguration | item | inventory_detail_help_describes_attribute_copy -->

> [!SOURCE-ERROR] `help_says_item_name` (on `name`)
<!-- source-error: record:itemlocationconfiguration | name | help_says_item_name -->

> [!SOURCE-ERROR] `help_describes_supply_type` (on `periodiclotsizetype`)
<!-- source-error: record:itemlocationconfiguration | periodiclotsizetype | help_describes_supply_type -->
