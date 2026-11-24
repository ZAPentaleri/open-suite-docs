# Bin Transfer (`bintransfer`)
<!-- id: record:bintransfer -->

| Fact | Value |
|---|---|
| Internal ID | `bintransfer` |
| Category | transaction |
| Client scripts | no |
| Server scripts | yes |
| Searchable | yes |
| Search data via | Transaction |
| Tags | `custom_fields_supported`, `scripting_full` |

## Record facts

- `script_types`: `server`
- `script_functions`: `create`, `update`, `copy`, `delete`, `search`
- `subrecords`: `inventorydetail`
- `gl_impact`: `false`
- `purpose`: `move_items_between_bins_in_warehouse`
- `effect`: `updates_bin_quantity_on_hand_only`
- `value_examples`: `50_of_ab123_from_bin_3003_to_4004`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `location` | select | Location | yes | no |  | `purpose`: `transfer_location`; `effect`: `limits_selectable_bins`; `constraint`: `bins_within_one_location` |
| `memo` | text | Memo | no | yes |  | `purpose`: `memo`; `supported_in`: `search` |
| `subsidiary` | select | Subsidiary | yes | no |  | `purpose`: `subsidiary_of_bin_transfer`; `value_source`: `system_displayed` |
| `trandate` | date | Date | yes | no |  | `purpose`: `transfer_date` |
| `createddate` | datetime |  | no | no |  |  |
| `externalid` | text | ExternalId | no | no |  |  |
| `lastmodifieddate` | datetime |  | no | no |  |  |
| `total` | currency |  | no | no |  |  |

## Sublist `inventory` (Adjustments)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `description` | text | Description | no |  |  |
| `inventorydetail` | summary | Inventory Detail | no |  |  |
| `item` | select | Item | yes |  |  |
| `itemunitslabel` | text | Units | no |  |  |
| `line` | text |  | no |  |  |
| `preferredbin` | text | Preferred Bin | no |  |  |
| `quantity` | float | Quantity | yes |  |  |
