# Inventory Adjustment (`inventoryadjustment`)
<!-- id: record:inventoryadjustment -->

| Fact | Value |
|---|---|
| Internal ID | `inventoryadjustment` |
| Category | transaction |
| Client scripts | yes |
| Server scripts | yes |
| Searchable | yes |
| Search data via | Transaction |
| Tags | `custom_fields_supported`, `scripting_full` |

## Record facts

- `script_types`: `client`, `server`
- `script_functions`: `create`, `update`, `copy`, `delete`, `search`
- `purpose`: `change_item_quantity_and_value_without_po`
- `value_examples`: `clerical_errors`, `cost_changes`, `theft`, `miscounts`
- `effect`: `preserves_lifo_fifo_costing_history`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `account` | select | Adjustment Account | yes | no |  | `purpose`: `adjustment_account`; `recommendation`: `expense_account`; `allowed_values_listed_in`: `account_records` |
| `adjlocation` | select | Adjustment Location | no | no |  | `purpose`: `adjustment_location`; `can`: `set_per_line_on_adjustments_sublist`; `allowed_values_listed_in`: `location_records`; `ui_effect`: `new_option_creates_record` |
| `class` | select | Class | no | no |  | `purpose`: `transaction_class`; `allowed_values_listed_in`: `class_records`; `ui_effect`: `new_option_creates_record` |
| `customer` | select | Customer | no | no |  | `purpose`: `customer_or_project_for_cost`; `use_when`: `project_cost_not_billed_as_line`; `value_examples`: `caterer_bottles_consumed` |
| `department` | select | Department | no | no |  | `purpose`: `transaction_department`; `allowed_values_listed_in`: `department_records`; `ui_effect`: `new_option_creates_record` |
| `estimatedtotalvalue` | currency | Estimated Total Value | no | no |  | `purpose`: `estimated_total_value_change`; `effect`: `recalculated_from_current_cost_on_submit` |
| `memo` | text | Memo | no | yes |  | `purpose`: `memo`; `displayed_on`: `registers`, `account_detail_report`; `help_text_shared_with`: `inventoryadjustment.inventory.memo` |
| `postingperiod` | select | Posting Period | no | no |  | `purpose`: `posting_period`; `cannot`: `post_to_closed_period` |
| `subsidiary` | select | Subsidiary | yes | no |  | `purpose`: `subsidiary`; `requires_feature`: `OneWorld` [`real_name`] |
| `trandate` | date | Date | yes | no |  | `purpose`: `adjustment_date`; `default_behavior`: `current_date` |
| `tranid` | text | Reference # | no | no |  | `purpose`: `reference_number`; `max_length`: `45` characters; `caveat`: `need_not_be_consecutive` |
| `cleared` | checkbox | Cleared | no |  |  |  |
| `cleareddate` | date | Date Cleared | no |  |  |  |
| `createddate` | datetime |  | no | no |  |  |
| `customform` | select | Custom Form | yes | no |  |  |
| `externalid` | text | ExternalId | no | no |  |  |
| `lastmodifieddate` | datetime |  | no | no |  |  |

## Sublist `inventory` (Adjustments)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `location` | select | Location | yes |  | `purpose`: `line_location`; `doc_inconsistency`: `adjustment_line_help_says_assembly`; `allowed_values_listed_in`: `location_records`; `ui_effect`: `new_option_creates_record` |
| `memo` | text | Memo | no |  | `purpose`: `memo`; `displayed_on`: `registers`, `account_detail_report`; `help_text_shared_with`: `inventoryadjustment.memo` |
| `adjustqtyby` | float | Adjust Qty. By | yes |  |  |
| `class` | select | Class | no |  |  |
| `costingmethod` | text |  | no |  |  |
| `currency` | text |  | no |  |  |
| `currentvalue` | currency | Current Value | no |  |  |
| `department` | select | Department | no |  |  |
| `description` | text | Description | no |  |  |
| `exchangerate` | float |  | no |  |  |
| `inventorydetail` | summary | Inventory Detail | no |  |  |
| `item` | select | Item | yes |  |  |
| `label` | label | Exchange Rate | no |  |  |
| `labelcurrency` | label | Currency | no |  |  |
| `labelfxunitcost` | label | Unit Cost in Foreign Currency | no |  |  |
| `line` | text |  | no |  |  |
| `newquantity` | float | New Quantity | no |  |  |
| `quantityonhand` | float | Qty. On Hand | no |  |  |
| `unitcost` | currency2 | Est. Unit Cost | no |  |  |
| `units` | select | Units | no |  |  |

## Sublist `accountingbookdetail` (Accounting Books)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `accountingbook` | select | Secondary Book | no |  |  |
| `exchangerate` | currency2 | Exchange Rate | yes |  |  |

## Tabs

| Tab ID | Label |
|---|---|
| `adjustments` | Adjustments |
| `custom` | Custom |
| `history` | History |
| `workflow` | Workflow |

## Suspected source errors

> [!SOURCE-ERROR] `adjustment_line_help_says_assembly` (on `inventory.location`)
<!-- source-error: record:inventoryadjustment | inventory.location | adjustment_line_help_says_assembly -->
