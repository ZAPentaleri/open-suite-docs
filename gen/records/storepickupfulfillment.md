# Store Pickup Fulfillment (`storepickupfulfillment`)
<!-- id: record:storepickupfulfillment -->

| Fact | Value |
|---|---|
| Internal ID | `storepickupfulfillment` |
| Category | transaction |
| Client scripts | yes |
| Server scripts | yes |
| Search data via | Transaction |
| Tags | `custom_fields_supported`, `no_copy`, `no_create` |

## Record facts

- `script_types`: `client`, `server`
- `script_functions`: `read`, `edit`, `transform`, `delete`, `search`
- `requires_feature`: `Store Pickup` [`real_name`]
- `recommendation`: `use_with_fulfillment_request`
- `create_via`: `transform_from_fulfillmentrequest`, `transform_from_salesorder`
- `requires`: `line_fulfillment_choice_store_pickup`
- `purpose`: `fulfill_store_pickup_lines_at_store_location`
- `not_supported`: `create`, `copy`
- `benefit`: `fulfillment_request_manages_store_workload_and_exceptions`
- `use_instead`: `record:itemfulfillment` (when `lines_with_ship_choice`)
- `value_examples`: `sales_order_321654_request_221100`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `createdfrom` | select | Fulfillment of | no |  |  |  |
| `customform` | select | Custom Form | no |  |  |  |
| `entity` | select | Customer | no |  |  |  |
| `location` | select | Location | no |  |  |  |
| `memo` | text | Memo | no |  |  |  |
| `pickeddate` | date | Picked Date | no |  |  |  |
| `pickedupdate` | date | Picked Up Date | no |  |  |  |
| `pickupexpirationdate` | date | Expiration Date | no |  |  |  |
| `pickupstatus` | select | Status | yes |  |  |  |
| `requestedby` | select | Requested By | no |  |  |  |
| `status` | text | Status | no |  |  |  |
| `trandate` | date | Date | yes |  |  |  |
| `tranid` | text | Ref No. | no |  |  |  |

## Sublist `item` (Items)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `item` | select | Item | no |  |  |
| `itemdescription` | text | Description | no |  |  |
| `line` | integer |  | no |  |  |
| `location` | select | Location | no |  |  |
| `onhand` | float | On Hand | no |  |  |
| `quantity` | posfloat | Quantity | no |  |  |
| `quantityremaining` | float | Quantity Remaining | no |  |  |
| `units` | integer |  | no |  |  |
| `unitsdisplay` | text | Units | no |  |  |
