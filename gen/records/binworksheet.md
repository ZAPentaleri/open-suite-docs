# Bin Worksheet (`binworksheet`)
<!-- id: record:binworksheet -->

| Fact | Value |
|---|---|
| Internal ID | `binworksheet` |
| Category | transaction |
| Client scripts | no |
| Server scripts | yes |
| Search data via | Transaction |
| Tags | `custom_fields_supported`, `dynamic_mode_only`, `no_copy`, `no_update` |

## Record facts

- `not_supported`: `copy`, `update`
- `record_modes`: `dynamic_only`
- `client_supported`: `false`
- `script_functions`: `create`, `delete`, `search`
- `not_supported`: `copy`, `update`
- `rule`: `item_listed_when_onhand_exceeds_bin_onhand`
- `behavior_depends_on`: `basic_vs_advanced_bin_management`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `location` | select | Location | yes | no |  | `purpose`: `location` |
| `memo` | text | Memo | no | yes |  | `purpose`: `memo`; `doc_inconsistency`: `help_describes_deletion_reason_comment` |
| `subsidiary` | select | Subsidiary | yes | no |  | `purpose`: `subsidiary`; `requires_feature`: `OneWorld` [`real_name`]; `default_behavior`: `current_user_subsidiary`; `allowed_values`: `vendor_assigned_subsidiaries` (when `shared_vendor`); `ui_effect`: `item_and_classification_fields_resourced` |
| `trandate` | date | Date | yes | no |  | `purpose`: `transaction_date`; `default_behavior`: `current_date` |
| `createddate` | datetime |  | no | no |  |  |
| `externalid` | text | ExternalId | no | no |  |  |
| `lastmodifieddate` | datetime |  | no | no |  |  |

## Sublist `item` (Items)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `description` | text | Description | no |  |  |
| `inventorydetail` | summary | Inventory Detail | no |  |  |
| `item` | text | Item | no |  |  |
| `itemname` | text | Item | no |  |  |
| `itemonhand` | float | Total | no |  |  |
| `line` | text |  | no |  |  |
| `quantity` | posfloat | Quantity | no |  |  |

## Suspected source errors

> [!SOURCE-ERROR] `help_describes_deletion_reason_comment` (on `memo`)
<!-- source-error: record:binworksheet | memo | help_describes_deletion_reason_comment -->
