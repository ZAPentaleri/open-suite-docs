# Order Reservation (`orderreservation`)
<!-- id: record:orderreservation -->

| Fact | Value |
|---|---|
| Internal ID | `orderreservation` |
| Category | transaction |
| Client scripts | yes |
| Server scripts | yes |
| Searchable | yes |
| Search data via | Transaction |
| Tags | `no_transform`, `supports_copy`, `supports_create`, `supports_delete`, `supports_read`, `supports_search`, `supports_update` |

## Record facts

- `requires_feature`: `Sales Channel Allocation` [`real_name`]
- `script_functions`: `create`, `read`, `update`, `delete`, `copy`, `search`
- `not_supported`: `transform`
- `not_exposed`: `description`, `three_sublists`
- `read_only_sublists`: `supply_allocation`, `fulfillment`
- `purpose`: `reserve_supply_for_future_priority_demand`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `allocateddemand` | float | Allocated Demand | no |  |  | `purpose`: `allocated_demand`; `value_source`: `system_displayed` |
| `allocatedsupply` | float | Allocated Supply | no |  |  | `purpose`: `allocated_supply`; `value_source`: `system_displayed` |
| `approvalstatus` | select | Approval Status | no |  |  | `purpose`: `approval_status`; `applies_when`: `approval_routing` |
| `closed` | checkbox | Closed | no |  |  | `purpose`: `close_reservation`; `true_means`: `closed_without_reserving` |
| `commitmentfirm` | checkbox | Commitment Firm | no |  |  | `purpose`: `commitment_firm`; `true_means`: `supply_quantity_held` |
| `enddate` | date | End Date | yes |  |  | `purpose`: `reservation_end_date` |
| `item` | select | Item | yes |  |  | `purpose`: `reserved_item` |
| `location` | select | Location | yes |  |  | `purpose`: `reservation_location` |
| `name` | text | Name | yes |  |  | `purpose`: `reservation_name` |
| `nextapprover` | select | Next Approver | no |  |  | `purpose`: `next_approver`; `applies_when`: `approval_routing` |
| `orderallocationstrategy` | select | Order Allocation Strategy | no |  |  | `purpose`: `order_allocation_strategy` |
| `orderpriority` | float | Order Priority | no |  |  | `purpose`: `order_priority`; `value_source`: `sales_channel_priority`; `doc_inconsistency`: `help_names_wrong_field` |
| `quantity` | float | Quantity | yes |  |  | `purpose`: `reserved_quantity` |
| `quantitycommitted` | float | Quantity Committed | no |  |  | `purpose`: `quantity_committed`; `value_source`: `system_displayed` |
| `saleschannel` | select | Sales Channel | yes |  |  | `purpose`: `sales_channel` |
| `startdate` | date | Start Date | yes |  |  | `purpose`: `reservation_start_date` |
| `subsidiary` | select | Subsidiary | no |  |  | `purpose`: `subsidiary`; `requires_feature`: `OneWorld` [`real_name`] |
| `targetquantity` | float | Target Quantity | no |  |  | `purpose`: `target_quantity` |
| `tranid` | text | ID | no |  |  | `purpose`: `reservation_id`; `value_source`: `system_generated`; `can`: `override` |
| `transactiondate` | date | Transaction Date | yes |  |  | `purpose`: `transaction_date` |
| `units` | select | Units | no |  |  | `purpose`: `units_reserved` |
| `customform` | select | Custom Form | no |  |  |  |
| `externalid` | text | External ID | no |  |  |  |
| `id` | integer | Internal ID | no |  |  |  |
| `templatestored` | checkbox |  | no |  |  |  |
| `transactionnumber` | text | Transaction Number | no |  |  |  |

## Suspected source errors

> [!SOURCE-ERROR] `help_names_wrong_field` (on `orderpriority`)
<!-- source-error: record:orderreservation | orderpriority | help_names_wrong_field -->
