# Work Order Close (`workorderclose`)
<!-- id: record:workorderclose -->

| Fact | Value |
|---|---|
| Internal ID | `workorderclose` |
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
- `requires`: `assembly_scrap_wip_and_wip_variance_accounts`
- `ui_location`: `transactions_manufacturing_close_work_order`
- `configured_by`: `enable_features_items_and_inventory_subtab` [`manufacturing_work_in_process`]
- `raises_error`: `invalid_work_order_or_not_wip_or_closed` (when `work_order_not_wip`)
- `doc_inconsistency`: `intro_says_soap_and_prerequisites_name_issue_record`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `class` | select | Class | no | no |  | `purpose`: `class`; `doc_inconsistency`: `class_help_says_sales_channel`; `value_examples`: `Reseller`, `Medical`, `Recurring Business` |
| `department` | select | Department | no | no |  | `purpose`: `department`; `value_examples`: `Marketing`, `Accounting`, `Sales` |
| `item` | select | Assembly | yes | no |  | `purpose`: `assembled_item`; `value_source`: `system_displayed` |
| `location` | select | Location | yes | no |  | `purpose`: `build_location` |
| `memo` | text | Memo | no | yes |  | `purpose`: `memo`; `supported_in`: `search` |
| `orderquantity` | float | Work Order Quantity | no | no |  | `purpose`: `work_order_quantity` |
| `postingperiod` | select | Posting Period | no | no |  | `purpose`: `posting_period` |
| `quantity` | posfloat | Built Quantity | no | no |  | `purpose`: `quantity_built` |
| `scrapquantity` | float | Quantity Scrapped | no | no |  | `purpose`: `quantity_scrapped` |
| `trandate` | date | Date | yes | no |  | `purpose`: `close_date` |
| `tranid` | text | Reference # | no | no |  | `purpose`: `reference_number` |
| `createddate` | datetime |  | no | no |  |  |
| `customform` | select | Custom Form | yes | no |  |  |
| `externalid` | text | ExternalId | no | no |  |  |
| `lastmodifieddate` | datetime |  | no | no |  |  |
| `manufacturingrouting` | integer |  | no | no |  |  |
| `revision` | select | Revision | no | no |  |  |
| `revisionmemo` | text | Revision Memo | no | no |  |  |
| `subsidiary` | select | Subsidiary | no | no |  |  |
| `taxperiod` | text |  | no | no |  |  |
| `total` | currency | Projected Value | no | no |  |  |

## Sublist `component` (Components)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `item` | text |  | no |  |  |
| `linenumber` | integer |  | no |  |  |
| `orderquantity` | float | Work Order Quantity | no |  |  |

## Sublist `routingitem` (Routing Items)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `item` | text |  | no |  |  |
| `unitcost` | float |  | no |  |  |

## Tabs

| Tab ID | Label |
|---|---|
| `components` | Components |
| `custom` | Custom |
| `history` | History |
| `routingitems` | Routing Items |
| `workflow` | Workflow |

## Suspected source errors

> [!SOURCE-ERROR] `intro_says_soap_and_prerequisites_name_issue_record`
<!-- source-error: record:workorderclose | - | intro_says_soap_and_prerequisites_name_issue_record -->

> [!SOURCE-ERROR] `class_help_says_sales_channel` (on `class`)
<!-- source-error: record:workorderclose | class | class_help_says_sales_channel -->
