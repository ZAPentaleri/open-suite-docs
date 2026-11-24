# Planning Workbench View (`planningview`)
<!-- id: record:planningview -->

| Fact | Value |
|---|---|
| Internal ID | `planningview` |

## Record facts

- `requires_feature`: `Material Requirements Planning` [`real_name`]
- `script_types`: `client`, `server`
- `script_functions`: `create`, `read`, `edit`, `copy`, `delete`, `search`, `transform`
- `doc_inconsistency`: `sample_describes_inventory_count`
- `purpose`: `planning_workbench_view_with_filters_and_item_search`
- `value_examples`: `item_location`, `preferred_supplier`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `action` | multiselect | Action Type | no |  |  | `purpose`: `action_type_filter`; `value_kind`: `multi_select`; `options`: `Cancel`, `Cancel Firm`, `Lead Time Threshold`, `Reschedule In`, `Reschedule In Firm`, `Reschedule Out`, `Reschedule Out Firm`, `Time Fence Threshold` |
| `demand` | multiselect | Demand Type | no |  |  | `purpose`: `demand_type_filter` |
| `exception` | multiselect | Exception Type | no |  |  | `purpose`: `exception_type_filter`; `value_kind`: `multi_select`; `options`: `insufficient_lead_time`, `item_with_no_item_location_configuration`, `negative_starting_balance`, `negative_time_fence_balance`, `past_due_demand`, `past_due_supply` |
| `item` | select | Item | no |  |  | `purpose`: `item_filter` |
| `name` | text | Name | yes |  |  | `purpose`: `view_name`; `value_examples`: `Purchasing View`, `Scheduling View` |
| `planningitemcategory` | select | Planning Item Category | no |  |  | `purpose`: `planning_item_category_filter` |
| `relativeenddays` | integer | Relative End Date (Days) | no |  |  | `purpose`: `relative_end_days`; `value_examples`: `30_shows_next_30_days` |
| `savedsearch` | select | Saved Search | no |  |  | `purpose`: `item_saved_search_filter`; `allowed_values_listed_in`: `item_saved_searches` |
| `supply` | multiselect | Supply Type | no |  |  | `purpose`: `supply_type_filter`; `value_kind`: `multi_select`; `options`: `Blanket Purchase Order`, `Inbound Shipment`, `On-Hand Quantity`, `Planned Purchase Order`, `Planned Transfer Order`, `Planned Work Order`, `Purchase Order`, `Supply Change Order`, `Transfer Order`, `Work Order`; `doc_inconsistency`: `help_says_exception_types` |
| `supplyreplenishmenttype` | select | Replenishment Method | no |  |  | `purpose`: `replenishment_method_filter`; `options`: `Master Production Scheduling`, `Material Requirements Planning` |
| `externalid` | text | External ID | no |  |  |  |
| `id` | integer | Internal ID | no |  |  |  |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `externalid` | select | External ID |  |
| `externalidstring` | text | External ID (Text) |  |
| `id` | select | Internal ID |  |
| `internalid` | select | Internal ID |  |
| `internalidnumber` | integer | Internal ID (Number) |  |
| `item` | select | Item |  |
| `name` | text | Name |  |
| `planningitemcategory` | select | Planning Item Category |  |
| `relativeenddays` | integer | Relative End Days |  |
| `supplyreplenishmenttype` | select | Supply Replenishment Type |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `externalid` | select | External ID |  |
| `id` | select | Internal ID |  |
| `internalid` | select | Internal ID |  |
| `item` | select | Item |  |
| `name` | text | Name |  |
| `planningitemcategory` | select | Planning Item Category |  |
| `relativeenddays` | integer | Relative End Days |  |
| `supplyreplenishmenttype` | select | Supply Replenishment Type |  |

## Suspected source errors

> [!SOURCE-ERROR] `sample_describes_inventory_count`
<!-- source-error: record:planningview | - | sample_describes_inventory_count -->

> [!SOURCE-ERROR] `help_says_exception_types` (on `supply`)
<!-- source-error: record:planningview | supply | help_says_exception_types -->
