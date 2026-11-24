# Work Order Issue (`workorderissue`)
<!-- id: record:workorderissue -->

| Fact | Value |
|---|---|
| Internal ID | `workorderissue` |
| Category | transaction |
| Client scripts | no |
| Server scripts | yes |
| Search data via | Transaction |
| Tags | `custom_fields_supported`, `no_copy`, `no_create` |

## Record facts

- `script_types`: `server`
- `not_supported`: `copy`, `create`
- `script_functions`: `update`, `delete`, `search`
- `requires_feature`: `Work Orders`, `Manufacturing Work In Process` [`real_name`]
- `create_via`: `transform_from_workorder` (refs `N/record.transform`)
- `not_supported`: `create`, `copy`
- `requires`: `work_order_marked_wip_and_item_set_up_for_wip`
- `purpose`: `records_components_gathered_for_assembly`
- `ui_location`: `transactions_manufacturing_issue_components`
- `ui_effect`: `work_order_issue_components_button`
- `configured_by`: `enable_features_items_and_inventory_subtab` [`manufacturing_work_in_process`]
- `raises_error`: `invalid_work_order_or_not_wip_or_closed` (when `work_order_not_wip`)
- `doc_inconsistency`: `intro_says_soap_and_completion_records`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `class` | select | Class | no | no |  | `purpose`: `class`; `doc_inconsistency`: `class_help_says_sales_channel` |
| `department` | select | Department | no | no |  | `purpose`: `department` |
| `endoperation` | select | Ending Operation | no | no |  | `purpose`: `operation_range`; `requires_together`: `startoperation`, `endoperation`; `effect`: `default_component_quantity_for_operation_range`; `help_text_shared_with`: `workorderissue.startoperation` |
| `item` | select | Assembly | yes | no |  | `purpose`: `assembly_item`; `constraint`: `assembly_items_only` |
| `location` | select | Location | yes | no |  | `purpose`: `location` |
| `manufacturingrouting` | select | Manufacturing Routing | no | no |  | `purpose`: `manufacturing_routing`; `value_source`: `system_displayed` |
| `memo` | text | Memo | no | yes |  | `purpose`: `memo`; `supported_in`: `search` |
| `postingperiod` | select | Posting Period | no | no |  | `purpose`: `posting_period` |
| `revision` | select | Revision | no | no |  | `purpose`: `revision` |
| `startoperation` | select | Starting Operation | no | no |  | `purpose`: `operation_range`; `requires_together`: `startoperation`, `endoperation`; `effect`: `default_component_quantity_for_operation_range`; `help_text_shared_with`: `workorderissue.endoperation` |
| `trandate` | date | Date | yes | no |  | `purpose`: `transaction_date`; `default_behavior`: `current_date` |
| `tranid` | text | Reference # | no | no |  | `purpose`: `reference_number` |
| `createddate` | datetime |  | no | no |  |  |
| `customform` | select | Custom Form | yes | no |  |  |
| `externalid` | text | ExternalId | no | no |  |  |
| `lastmodifieddate` | datetime |  | no | no |  |  |
| `revisionmemo` | text | Revision Memo | no | no |  |  |
| `subsidiary` | select | Subsidiary | no | no |  |  |
| `taxperiod` | text |  | no | no |  |  |

## Sublist `component` (Components)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `componentinventorydetail` | summary | Inventory Detail | no |  |  |
| `item` | text |  | no |  |  |
| `linenumber` | integer |  | no |  |  |
| `operationsequencenumber` | text | Operation | no |  |  |
| `quantity` | float | Quantity | yes |  |  |
| `unitcost` | float |  | no |  |  |

## Tabs

| Tab ID | Label |
|---|---|
| `components` | Components |
| `custom` | Custom |
| `history` | History |
| `workflow` | Workflow |

## Suspected source errors

> [!SOURCE-ERROR] `intro_says_soap_and_completion_records`
<!-- source-error: record:workorderissue | - | intro_says_soap_and_completion_records -->

> [!SOURCE-ERROR] `class_help_says_sales_channel` (on `class`)
<!-- source-error: record:workorderissue | class | class_help_says_sales_channel -->
