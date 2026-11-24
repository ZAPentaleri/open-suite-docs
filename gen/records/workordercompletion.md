# Work Order Completion (`workordercompletion`)
<!-- id: record:workordercompletion -->

| Fact | Value |
|---|---|
| Internal ID | `workordercompletion` |
| Category | transaction |
| Client scripts | no |
| Server scripts | yes |
| Search data via | Transaction |
| Tags | `custom_fields_supported`, `no_copy`, `no_create` |

## Record facts

- `requires_feature`: `Work Orders`, `Manufacturing Work In Process` [`real_name`]
- `create_via`: `transform_from_workorder` (refs `N/record.transform`)
- `not_supported`: `create`, `copy`
- `requires`: `work_order_marked_wip_and_item_set_up_for_wip`
- `requires`: `assembly_scrap_and_wip_accounts`
- `sublist_available_when`: `component:backflush_true`
- `purpose`: `records_assemblies_built_optionally_consuming_components`
- `use_when`: `backflush_when_earlier_issues_missed_materials`
- `configured_by`: `enable_features_items_and_inventory_subtab` [`manufacturing_work_in_process`]
- `ui_location`: `transactions_manufacturing_enter_completions`
- `ui_effect`: `work_order_has_enter_completions_and_backflush_buttons`
- `script_types`: `server`
- `script_functions`: `update`, `delete`, `search`
- `raises_error`: `invalid_work_order_or_not_wip_or_closed` (when `work_order_not_wip`)
- `doc_inconsistency`: `intro_says_soap_web_services`
- `doc_inconsistency`: `prerequisites_heading_names_work_order_issue`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `class` | select | Class | no | no |  | `purpose`: `class` |
| `completedquantity` | float | Completed Quantity | no | no |  | `purpose`: `completed_quantity`; `value_source`: `system_displayed` |
| `department` | select | Department | no | no |  | `purpose`: `department` |
| `endoperation` | select | Ending Operation | yes | no |  | `purpose`: `operation_range`; `requires_together`: `startoperation`, `endoperation`; `effect`: `default_component_quantity_for_operation_range`; `help_text_shared_with`: `workordercompletion.startoperation` |
| `inventorydetail` | summary | Inventory Detail | no | no |  | `purpose`: `inventory_detail_subrecord`; `requires`: `quantity_entered_first` |
| `isbackflush` | checkbox | Backflush | no | no |  | `purpose`: `backflush`; `effect`: `component_consumption_recorded_with_completion` |
| `item` | select | Assembly | yes | no |  | `purpose`: `assembly_item`; `constraint`: `assembly_items_only` |
| `location` | select | Location | yes | no |  | `purpose`: `location`; `value_source`: `system_displayed` |
| `manufacturingrouting` | select | Manufacturing Routing | no | no |  | `purpose`: `manufacturing_routing`; `value_source`: `system_displayed` |
| `memo` | text | Memo | no | yes |  | `purpose`: `memo`; `supported_in`: `search` |
| `orderquantity` | float | Work Order Quantity | no | no |  | `purpose`: `work_order_quantity`; `value_source`: `system_displayed` |
| `postingperiod` | select | Posting Period | no | no |  | `purpose`: `posting_period` |
| `quantity` | float | Quantity to Build | yes | no |  | `purpose`: `quantity_completed` |
| `revision` | select | Revision | no | no |  | `purpose`: `revision` |
| `scrapquantity` | float | Scrap Quantity | no | no |  | `purpose`: `quantity_scrapped` |
| `startoperation` | select | Starting Operation | yes | no |  | `purpose`: `operation_range`; `requires_together`: `startoperation`, `endoperation`; `effect`: `default_component_quantity_for_operation_range`; `help_text_shared_with`: `workordercompletion.endoperation` |
| `total` | currency | Projected Value | no | no |  | `purpose`: `projected_value`; `value_source`: `assembly_cost_not_components`; `caveat`: `components_shown_when_backflush` |
| `trandate` | date | Date | yes | no |  | `purpose`: `transaction_date`; `default_behavior`: `current_date` |
| `tranid` | text | Reference # | no | no |  | `purpose`: `reference_number` |
| `units` | select | Units | no | no |  | `purpose`: `unit_of_measure`; `value_source`: `system_displayed` |
| `createddate` | datetime |  | no | no |  |  |
| `customform` | select | Custom Form | yes | no |  |  |
| `externalid` | text | ExternalId | no | no |  |  |
| `lastmodifieddate` | datetime |  | no | no |  |  |
| `revisionmemo` | text | Revision Memo | no | no |  |  |
| `subsidiary` | select | Subsidiary | no | no |  |  |
| `taxperiod` | text |  | no | no |  |  |
| `unitcost` | float | Unit Cost | no | no |  |  |

## Sublist `accountingbookdetail` (Accounting Books)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `accountingbook` | select | Secondary Book | no |  |  |
| `exchangerate` | currency2 | Exchange Rate | yes |  |  |

## Sublist `component` (Components)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `componentinventorydetail` | summary | Inventory Detail | no |  |  |
| `item` | text |  | no |  |  |
| `linenumber` | integer |  | no |  |  |
| `operationsequencenumber` | text | Operation | no |  |  |
| `quantity` | float | Quantity | yes |  |  |
| `quantityper` | float | Quantity Per | no |  |  |
| `unitcost` | float |  | no |  |  |

## Sublist `operation` (Operations)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `completedquantity` | float | Completed Quantity | no |  |  |
| `inputquantity` | float | Input Quantity | no |  |  |
| `laborresources` | double | Labor Resources | no |  |  |
| `laborruntime` | float | Labor Run Time (Min) | no |  |  |
| `laborsetuptime` | float | Labor Setup Time (Min) | no |  |  |
| `machineresources` | double | Machine Resources | no |  |  |
| `machineruntime` | float | Machine Run Time (Min) | no |  |  |
| `machinesetuptime` | float | Machine Setup Time (Min) | no |  |  |
| `operationname` | text | Operation Name | no |  |  |
| `operationsequence` | integer | Operation Sequence | no |  |  |
| `predecessorcompletedquantity` | float | Predecessor Completed Quantity | no |  |  |
| `quantityremaining` | float | Quantity Remaining | no |  |  |
| `recordsetup` | checkbox | Record Setup Time | no |  |  |
| `runrate` | float |  | no |  |  |
| `setuptime` | float |  | no |  |  |
| `workcenter` | text | Manufacturing Work Center | no |  |  |

## Tabs

| Tab ID | Label |
|---|---|
| `components` | Components |
| `custom` | Custom |
| `history` | History |
| `operations` | Operations |
| `workflow` | Workflow |

## Suspected source errors

> [!SOURCE-ERROR] `intro_says_soap_web_services`
<!-- source-error: record:workordercompletion | - | intro_says_soap_web_services -->

> [!SOURCE-ERROR] `prerequisites_heading_names_work_order_issue`
<!-- source-error: record:workordercompletion | - | prerequisites_heading_names_work_order_issue -->
