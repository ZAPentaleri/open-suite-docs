# Inventory Transfer (`inventorytransfer`)
<!-- id: record:inventorytransfer -->

| Fact | Value |
|---|---|
| Internal ID | `inventorytransfer` |
| Category | transaction |
| Client scripts | no |
| Server scripts | yes |
| Searchable | yes |
| Search data via | Transaction |
| Tags | `custom_fields_supported`, `scripting_full` |

## Record facts

- `script_types`: `server`
- `script_functions`: `create`, `update`, `copy`, `delete`, `search`, `transform`
- `requires_feature`: `Locations`, `Multi-Location Inventory` [`real_name`]
- `doc_inconsistency`: `functions_sentence_names_inventory_detail`
- `purpose`: `move_inventory_between_two_locations_in_one_step`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `class` | select | Class | no | no |  | `purpose`: `transaction_class`; `allowed_values_listed_in`: `class_records`; `ui_effect`: `new_option_creates_record` |
| `department` | select | Department | no | no |  | `purpose`: `transaction_department`; `allowed_values_listed_in`: `department_records`; `ui_effect`: `new_option_creates_record` |
| `location` | select | From Location | yes | no |  | `purpose`: `from_location`; `effect`: `inventory_reduced_here`; `allowed_values_listed_in`: `location_records` |
| `memo` | text | Memo | no | yes |  | `purpose`: `memo`; `supported_in`: `search` |
| `postingperiod` | select | Posting Period | no | no |  | `purpose`: `posting_period`; `cannot`: `post_to_closed_period` |
| `subsidiary` | select | Subsidiary | yes | no |  | `purpose`: `subsidiary` |
| `trandate` | date | Date | yes | no |  | `purpose`: `transaction_date`; `default_behavior`: `current_date` |
| `tranid` | text | Ref No. | no | no |  | `purpose`: `reference_number` |
| `transferlocation` | select | To Location | yes | no |  | `purpose`: `to_location`; `effect`: `inventory_increased_here`; `allowed_values_listed_in`: `location_records` |
| `createddate` | datetime |  | no | no |  |  |
| `externalid` | text | ExternalId | no | no |  |  |
| `lastmodifieddate` | datetime |  | no | no |  |  |
| `total` | currency |  | no | no |  |  |

## Sublist `inventory` (Adjustments)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `adjustqtyby` | posfloat | Qty to Transfer | no |  |  |
| `description` | text | Description | no |  |  |
| `inventorydetail` | summary | Inventory Detail | no |  |  |
| `item` | select | Item | yes |  |  |
| `line` | text |  | no |  |  |
| `quantityonhand` | float | Qty On Hand | no |  |  |
| `units` | select | Units | no |  |  |

## Suspected source errors

> [!SOURCE-ERROR] `functions_sentence_names_inventory_detail`
<!-- source-error: record:inventorytransfer | - | functions_sentence_names_inventory_detail -->
