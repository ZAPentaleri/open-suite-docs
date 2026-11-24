# Fulfillment Request (`fulfillmentrequest`)
<!-- id: record:fulfillmentrequest -->

| Fact | Value |
|---|---|
| Internal ID | `fulfillmentrequest` |
| Category | transaction |
| Client scripts | yes |
| Server scripts | yes |
| Search data via | Transaction |
| Tags | `custom_fields_supported`, `no_copy`, `no_create` |

## Record facts

- `script_types`: `client`, `server`
- `script_functions`: `read`, `edit`, `transform`, `delete`, `search`
- `requires_feature`: `Fulfillment Request` [`real_name`]
- `create_via`: `transform_from_salesorder`
- `rule`: `one_request_per_fulfillment_choice_and_location`
- `settable_statuses`: `A:New`, `B:In Progress`, `C:Cancelled`
- `system_statuses`: `D:Picked`, `I:Packed`, `E:Partially Fulfilled`, `G:Fulfilled with Exceptions`, `F:Fulfilled`, `R:Rejected`
- `exception_types`: `1:picking`, `2:packing`, `3:picking_up`, `4:shipping`
- `exception_requires`: `quantity`, `exception_type`, `exception_reason`
- `purpose`: `demand_to_fulfill_sales_order_at_location`
- `not_supported`: `create`, `copy`
- `requires`: `fulfillment_choice_and_location_on_each_so_line`
- `options`: `ship`, `store_pickup` [`request_type_from_fulfillment_choice`]
- `caveat`: `mixed_choices_or_locations_in_one_request_throws`
- `effect`: `automation_creates_needed_requests`
- `value_examples`: `two_lines_ship_and_pickup_need_two_requests`
- `caveat`: `exception_reasons_differ_per_exception_type`

## Body fields

| Field ID | Type | Label | Required | submitField | Read-only | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `assignee` | select | Assignee | no |  |  |  | `purpose`: `assignee`; `allowed_values`: `active_employees` |
| `createdfrom` | select | Created From | no |  |  |  | `purpose`: `created_from_sales_order`; `value_source`: `system_displayed` |
| `entity` | select | Customer | yes |  |  |  | `purpose`: `destination_customer`; `value_source`: `system_displayed` |
| `fulfillmenttype` | select | Fulfillment Type | no |  |  |  | `purpose`: `fulfillment_type`; `allowed_values`: `Ship`, `Store Pickup`; `option_meanings`: `ship:creates_item_fulfillment`, `store_pickup:creates_store_pickup_fulfillment` |
| `location` | select | Fulfillment Location | no |  | yes |  | `purpose`: `fulfillment_location`; `value_source`: `sales_order_line_location` |
| `memo` | text | Memo | no |  |  |  | `purpose`: `request_memo`; `comparable_to`: `line_request_note`; `value_examples`: `reason_for_rejected_line` |
| `pickuphold` | select | Pickup Hold | no |  |  |  | `purpose`: `pickup_hold_status`; `allowed_values`: `Customer Feedback Requested`, `Customer Feedback Received`, `Instruction Acknowledged`; `option_meanings`: `feedback_requested:exception_needs_customer_input`, `feedback_received:instructions_set`, `acknowledged:store_reviewed_after_feedback` |
| `trandate` | date | Date | yes |  |  |  | `purpose`: `request_creation_date`; `supported_in`: `list_sort` |
| `tranid` | text | Fulfillment Number | no |  |  |  | `purpose`: `request_number`; `default_behavior`: `highest_number_plus_one`; `can`: `override` |
| `transtatus` | select | Status | yes |  |  |  | `purpose`: `fulfillment_request_status`; `allowed_values`: `New`, `In Progress`, `Cancelled`, `Picked`, `Packed`, `Partially Fulfilled`, `Fulfilled`; `allowed_values`: `New`, `In Progress` (when `creating_request`); `available_only_when`: `cancelled_after_creation_before_picking` [`cancelled`]; `available_only_when`: `new_before_any_line_picked` [`new`]; `option_meanings`: `new:new_for_location`, `in_progress:acknowledged_or_partly_processed`, `cancelled:discarded_not_fulfilled` |
| `customform` | select | Custom Form | no |  |  |  |  |
| `expirationdate` | date | Expiration Date | no |  |  |  |  |
| `ispickupemailnotificationsent` | checkbox | Pickup Email Notification Sent | no |  |  |  |  |
| `prevdate` | date | Previous Effective Date | no |  |  |  |  |
| `status` | text | Status | no |  |  |  |  |
| `subsidiary` | select |  | no |  |  |  |  |

## Sublist `fulfillmentexception` (Fulfillment Exceptions)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `exceptioncomments` | textarea | Exception Comments | no |  |  |
| `exceptionquantity` | float | Exception Quantity | yes |  |  |
| `exceptionreason` | select | Exception Reason | yes |  |  |
| `exceptiontype` | select | Exception Type | yes |  |  |
| `itemline` | integer | Item | yes |  |  |
| `line` | integer |  | no |  |  |
| `quantityrequested` | float | Quantity Requested | no |  |  |

## Sublist `item` (Items)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `acknowledgefulfillinstruction` | checkbox | Acknowledge Instruction | no |  |  |
| `class` | select | Class | no |  |  |
| `department` | select | Department | no |  |  |
| `fulfillmentinstruction` | select | Instruction | no |  |  |
| `item` | select | Item | no |  |  |
| `itemdescription` | text | Description | no |  |  |
| `itemreceive` | checkbox | Fulfill | no |  |  |
| `line` | integer |  | no |  |  |
| `location` | select | Location | no |  |  |
| `onhand` | float | On Hand | no |  |  |
| `options` | float | Options | no |  |  |
| `quantity` | posfloat | Quantity | no |  |  |
| `quantityfulfilled` | posfloat | Fulfilled | no |  |  |
| `quantitypacked` | posfloat | Packed | no |  |  |
| `quantitypicked` | posfloat | Picked | no |  |  |
| `quantityrejected` | float | Rejected | no |  |  |
| `quantityremaining` | float | Quantity Remaining | no |  |  |
| `quantityremainingdisplay` | float | Remaining | no |  |  |
| `requestnote` | textarea | Request Note | no |  |  |
| `units` | integer |  | no |  |  |
| `unitsdisplay` | text | Units | no |  |  |
