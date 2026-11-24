# Inventory Status Change (`inventorystatuschange`)
<!-- id: record:inventorystatuschange -->

| Fact | Value |
|---|---|
| Internal ID | `inventorystatuschange` |
| Category | transaction |
| Client scripts | yes |
| Server scripts | yes |
| Searchable | yes |
| Search data via | Transaction |
| Tags | `custom_fields_supported`, `scripting_full` |

## Record facts

- `script_types`: `client`, `server`
- `script_functions`: `create`, `read`, `edit`, `copy`, `delete`, `search`, `transform`
- `requires_feature`: `Inventory Status` [`real_name`]
- `sublists`: `inventory`
- `script_functions`: `create`, `read`, `edit`, `copy`, `delete`, `search`, `transform`
- `no_validation`: `previousstatus_equals_revisedstatus`
- `configured_by`: `enable_features_items_and_inventory_subtab` [`inventory_status`]
- `difference`: `ui_rejects_same_previous_and_revised_status_script_does_not`
- `value_examples`: `move_10_good_widgets_to_inspection_status`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `location` | select | Location | yes | no |  | `purpose`: `location` |
| `memo` | text | Memo | no | yes |  | `purpose`: `memo` |
| `previousstatus` | select | Previous Status | yes | no |  | `purpose`: `previous_inventory_status` |
| `revisedstatus` | select | Revised Status | yes | no |  | `purpose`: `revised_inventory_status` |
| `externalid` | text | ExternalId | no | no |  |  |
