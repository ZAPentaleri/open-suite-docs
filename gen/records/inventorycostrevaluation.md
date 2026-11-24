# Inventory Cost Revaluation (`inventorycostrevaluation`)
<!-- id: record:inventorycostrevaluation -->

| Fact | Value |
|---|---|
| Internal ID | `inventorycostrevaluation` |
| Category | transaction |
| Client scripts | yes |
| Server scripts | yes |
| Searchable | yes |
| Tags | `custom_fields_supported`, `scripting_full` |

## Record facts

- `script_functions`: `create`, `update`, `copy`, `delete`, `search`
- `user_events`: `beforeLoad`, `beforeSubmit`, `afterSubmit`
- `requires_feature`: `Standard Costing` [`real_name`]
- `user_events`: `beforeLoad`, `beforeSubmit`, `afterSubmit`
- `purpose`: `recalculate_standard_cost_item_value`
- `configured_by`: `enable_features_items_and_inventory_subtab` [`standard_costing`]
- `ui_location`: `transactions_inventory_revalue_inventory_cost`
- `script_types`: `client`, `server`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `account` | select | Adjustment Account | yes | no |  | `purpose`: `adjustment_account`; `effect`: `value_variances_post_here` |
| `class` | select | Class | no | no |  | `purpose`: `class` |
| `department` | select | Department | no | no |  | `purpose`: `department` |
| `item` | select | Item | yes | no |  | `purpose`: `revaluation_item`; `constraint`: `assembly_items_only` |
| `location` | select | Location | yes | no |  | `purpose`: `location` |
| `memo` | text | Memo | no | yes |  | `purpose`: `memo` |
| `postingperiod` | select | Posting Period | no | no |  | `purpose`: `posting_period` |
| `subsidiary` | select | Subsidiary | yes | no |  | `purpose`: `subsidiary`; `requires_feature`: `OneWorld` [`real_name`] |
| `trandate` | date | Date | yes | no |  | `purpose`: `revaluation_date`; `default_behavior`: `system_date` |
| `tranid` | text | Ref. No. | no | no |  | `purpose`: `reference_number` |
| `unitcost` | currency2 | New Unit Cost | no | no |  | `purpose`: `revised_unit_cost`; `value_source`: `system_displayed` |
| `createddate` | datetime |  | no | no |  |  |
| `customform` | select | Custom Form | yes | no |  |  |
| `externalid` | text | ExternalId | no | no |  |  |
| `inventoryvalue` | currency | New Inventory Value | no | no |  |  |
| `lastmodifieddate` | datetime |  | no | no |  |  |
| `taxperiod` | text |  | no | no |  |  |
| `total` | currency | Adjustment Amount | yes | no |  |  |

## Sublist `accountingbookdetail` (Accounting Books)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `accountingbook` | select | Secondary Book | no |  |  |
| `exchangerate` | currency2 | Exchange Rate | yes |  |  |

## Tabs

| Tab ID | Label |
|---|---|
| `costcomponents` | Cost Components |

## Search joins

| Join ID | Label | Joined record |
|---|---|---|
| `billingaddress` | Billing Address | Address |
