# Intercompany Transfer Order (`intercompanytransferorder`)
<!-- id: record:intercompanytransferorder -->

| Fact | Value |
|---|---|
| Internal ID | `intercompanytransferorder` |
| Category | transaction |
| Client scripts | yes |
| Server scripts | yes |
| Searchable | yes |
| Search data via | Transaction |
| Tags | `scripting_full` |

## Record facts

- `script_types`: `client`, `server`
- `script_functions`: `create`, `update`, `copy`, `delete`, `search`, `transform`
- `user_events`: `beforeLoad`, `beforeSubmit`, `afterSubmit`
- `user_events`: `beforeLoad`, `beforeSubmit`, `afterSubmit`
- `requires`: `oneworld`, `multi_location_inventory`
- `requires_preference_off`: `use_item_cost_as_transfer_cost`
- `purpose`: `move_inventory_between_subsidiary_locations`
- `ui_location`: `transactions_inventory_enter_intercompany_transfer_orders`
- `configured_by`: `use_item_cost_as_transfer_cost_preference_off`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `altshippingcost` | currency | Shipping Cost | no | no |  | `purpose`: `shipping_cost`; `value_source`: `system_displayed` |
| `class` | select | Class | no | no |  | `purpose`: `transaction_class`; `allowed_values_listed_in`: `class_records`; `ui_effect`: `new_option_creates_record` |
| `customform` | select | Custom Form | yes | no |  | `purpose`: `custom_form` |
| `department` | select | Department | no | no |  | `purpose`: `transaction_department`; `allowed_values_listed_in`: `department_records`; `ui_effect`: `new_option_creates_record` |
| `employee` | select | Employee | no | no |  | `purpose`: `employee`; `allowed_values_listed_in`: `employee_records`; `ui_effect`: `new_option_creates_record` |
| `firmed` | checkbox | Firmed | no | no |  | `purpose`: `firmed`; `true_means`: `lines_not_reallocatable`; `cannot`: `reschedule_or_cancel` (when `firmed`) |
| `incoterm` | select | Incoterm | yes | no |  | `purpose`: `incoterm`; `allowed_values`: `EXW`, `DAP` (when `creating_transfer_order`); `option_meanings`: `exw:ownership_at_shipping_point`, `dap:ownership_at_destination`; `effect`: `affects_accounting_and_inventory`; `can`: `choose_others_after_pending_approval_or_fulfillment` |
| `location` | select | From Location | no | no |  | `purpose`: `from_location`; `effect`: `inventory_decreased_on_fulfillment`; `required_when`: `approving_or_fulfilling`; `allowed_values_listed_in`: `location_records`; `ui_effect`: `new_option_creates_record`; `not_required_when`: `entering_transfer_order` |
| `memo` | text | Memo | no | yes |  | `purpose`: `memo`; `supported_in`: `search` |
| `orderstatus` | select | Status | yes | no |  | `purpose`: `order_status`; `options`: `Pending Approval`, `Pending Fulfillment`; `option_meanings`: `pending_fulfillment:skips_approval` |
| `shipaddress` | address | Ship To | no | no |  | `purpose`: `ship_to_address`; `default_behavior`: `destination_location_address`; `can`: `choose_other_or_new_address_in_selector` |
| `shipaddresslist` | select | Ship To Select | no | no |  | `purpose`: `shipping_address_selector`; `ui_effect`: `new_option_creates_record` |
| `shipcomplete` | checkbox | Ship Complete | no | no |  | `purpose`: `ship_complete_only`; `supported_in`: `bulk_fulfillment_filter` |
| `shipdate` | date | Ship Date | no | no |  | `purpose`: `ship_date`; `default_behavior`: `current_date` |
| `shipmethod` | select | Shipping Method | no | no |  | `purpose`: `ship_method`; `effect`: `fills_shipping_cost` (when `charge_for_shipping`); `allowed_values_listed_in`: `shipping_item_records`; `configured_by`: `charge_for_shipping_setting` |
| `shippingcost` | currency | Shipping Cost | no | no |  | `purpose`: `shipping_cost`; `value_source`: `calculated_from_ship_method`; `caveat`: `ups_real_time_splits_over_150_lb`; `value_source`: `shipping_item_rates` |
| `subsidiary` | select | Subsidiary | no | no |  | `purpose`: `source_subsidiary`; `effect`: `limits_from_locations`; `value_examples`: `subsidiary_b_limits_to_its_locations` |
| `subtotal` | currency | Subtotal | no | no |  | `purpose`: `subtotal`; `value_source`: `sum_of_lines` |
| `tosubsidiary` | select | To Subsidiary | no | no |  | `purpose`: `receiving_subsidiary`; `effect`: `limits_to_locations`; `value_examples`: `subsidiary_b_limits_to_its_locations` |
| `total` | currency | Total | no | no |  | `purpose`: `total_with_shipping_and_handling` |
| `trackingnumbers` | text | Additional Tracking # | no | no |  | `purpose`: `carrier_tracking_number`; `value_source`: `system_displayed` |
| `trandate` | date | Date | yes | no |  | `purpose`: `transaction_date`; `default_behavior`: `current_date` |
| `tranid` | text | Order # | no | no |  | `purpose`: `transfer_order_number`; `default_behavior`: `next_number` (when `auto_generated_numbering`) |
| `transactionnumber` | text | Transaction Number | no | no |  | `purpose`: `transaction_number`; `value_source`: `system_generated` |
| `transferlocation` | select | To Location | yes | no |  | `purpose`: `destination_location`; `effect`: `inventory_and_value_increased_on_receipt`; `effect`: `ship_to_address_defaults_from_location`; `allowed_values_listed_in`: `location_records`; `ui_effect`: `new_option_creates_record` |
| `useitemcostastransfercost` | checkbox | Use Item Cost As Transfer Cost | no | no |  | `purpose`: `use_item_cost_as_transfer_cost`; `true_means`: `transfer_price_declared_value_only`; `constraint`: `cannot_receive_more_than_fulfilled` (when `checked`); `false_means`: `transfer_price_is_receipt_item_cost`; `caveat`: `no_transfer_price_records_no_cost` (when `cleared`); `effect`: `cost_difference_posts_to_gain_loss_account` (when `cleared`); `default_behavior`: `item_record_transfer_price`; `cannot`: `change_after_approval`; `value_examples`: `fulfilled_10_of_20_cannot_receive_12`; `use_when`: `insurance_or_international_shipping` (when `checked`); `caveat`: `not_a_charge_and_no_costing_effect` (when `checked`); `editable_only_when`: `not_yet_approved` (when `approval_routing`) |
| `createddate` | datetime |  | no | no |  |  |
| `createdfrom` | select | Created From | no | no |  |  |
| `currency` | select | Currency | no | no |  |  |
| `currencyname` | text | Currency | no | no |  |  |
| `currencysymbol` | text | Currency Symbol | no | no |  |  |
| `entity` | select |  | no | no |  |  |
| `exchangerate` | currency2 | Exchange Rate | yes | no |  |  |
| `externalid` | text | ExternalId | no | no |  |  |
| `fob` | text | FOB | no | no |  |  |
| `isbasecurrency` | checkbox | Base Currency | no | no |  |  |
| `lastmodifieddate` | datetime |  | no | no |  |  |
| `linkedtrackingnumbers` | text | Tracking # | no | no |  |  |
| `nexus` | select | Nexus | no | no |  |  |
| `oldrevenuecommitment` | text |  | no | no |  |  |
| `otherrefnum` | text |  | no | no |  |  |
| `returntrackingnumbers` | text | Return Tracking # | no | no |  |  |
| `shipisresidential` | text | Shipping address is residential | no | no |  |  |
| `shipoverride` | text |  | no | no |  |  |
| `shippingaddress` | summary |  | no | no |  |  |
| `shippingcostoverridden` | text | Shipping Cost Overridden | no | no |  |  |
| `source` | text | Source | no | no |  |  |
| `status` | text | Status | no | no |  |  |
| `statusRef` | text |  | no | no |  |  |
| `updatecurrency` | text | Update Currency | no | no |  |  |

## Sublist `accountingbookdetail` (Accounting Books)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `accountingbook` | select | Secondary Book | no |  |  |
| `exchangerate` | currency2 | Exchange Rate | yes |  |  |

## Sublist `item` (Items)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `amount` | currency | Amount | no |  |  |
| `billvariancestatus` | text |  | no |  |  |
| `catchupperiod` | select |  | no |  |  |
| `commitinventory` | select | Commit | no |  |  |
| `daysbeforeexpiration` | text |  | no |  |  |
| `deferrevrec` | checkbox |  | no |  |  |
| `description` | textarea | Description | no |  |  |
| `expectedreceiptdate` | date | Expected Receipt Date | no |  |  |
| `expectedshipdate` | date | Expected Ship Date | no |  |  |
| `id` | text |  | no |  |  |
| `inventorydetail` | summary | Inventory Detail | no |  |  |
| `isclosed` | checkbox | Closed | no |  |  |
| `isvsoebundle` | text |  | no |  |  |
| `item` | select | Item | yes |  |  |
| `itemsubtype` | text |  | no |  |  |
| `itemtype` | text |  | no |  |  |
| `line` | text |  | no |  |  |
| `linenumber` | integer |  | no |  |  |
| `matrixtype` | text |  | no |  |  |
| `options` | namevaluelist | Options | no |  |  |
| `printitems` | text |  | no |  |  |
| `quantity` | float | Quantity | no |  |  |
| `quantityavailable` | float |  | no |  |  |
| `quantitybackordered` | float | Back Ordered | no |  |  |
| `quantitycommitted` | float | Committed | no |  |  |
| `quantityfulfilled` | float | Fulfilled | no |  |  |
| `quantityreceived` | float | Received | no |  |  |
| `rate` | currency2 | Transfer Price | no |  |  |
| `units` | select | Units | no |  |  |

## Tabs

| Tab ID | Label |
|---|---|
| `address` | Address |
| `custom` | Custom |
| `history` | History |
| `items` | Items |
| `shipping` | Shipping |
| `workflow` | Workflow |

## Transform targets

| Target record | Default fields |
|---|---|
| `itemfulfillment` | `customform`, `nsi`, `shipgroup` |
| `itemreceipt` | `customform` |
