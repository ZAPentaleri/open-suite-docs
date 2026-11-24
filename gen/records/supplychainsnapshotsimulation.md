# Supply Chain Snapshot Simulation (`supplychainsnapshotsimulation`)
<!-- id: record:supplychainsnapshotsimulation -->

| Fact | Value |
|---|---|
| Internal ID | `supplychainsnapshotsimulation` |
| Category | item |
| Client scripts | yes |
| Server scripts | yes |
| Searchable | yes |
| Tags | `supports_create`, `supports_delete`, `supports_read`, `supports_search`, `supports_update` |

## Record facts

- `script_types`: `client`, `server`
- `requires_feature`: `Supply Chain Control Tower` [`real_name`]
- `script_functions`: `create`, `update`, `delete`, `search`
- `not_scriptable`: `item_type`
- `search_only_sublists`: `systemnotes`
- `purpose`: `simulate_transaction_effect_on_inventory`
- `configured_by`: `enable_features_items_and_inventory_subtab` [`supply_chain_control_tower`]
- `ui_location`: `lists_supply_chain_supply_chain_snapshot_simulations`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `demandlocation` | select | Demand Location | no |  |  | `purpose`: `demand_location` |
| `demandlocationdate` | date | Demand Date | no |  |  | `purpose`: `demand_date` |
| `demandlocationsubsidiary` | select | Demand Subsidiary | no |  |  | `purpose`: `demand_subsidiary` |
| `isforreschedule` | checkbox | For Reschedule | no |  |  | `purpose`: `for_reschedule` |
| `itemnamenumber` | select | Item Name/Number | yes |  |  | `purpose`: `item_name`; `value_source`: `original_snapshot` |
| `itemtype` | select | Item Type | no |  |  | `purpose`: `item_type`; `value_source`: `original_snapshot` |
| `memo` | textarea | Memo | no |  |  | `purpose`: `memo` |
| `owner` | select | Owner | no |  |  | `purpose`: `simulation_owner` |
| `quantity` | integer | Quantity | yes |  |  | `purpose`: `simulation_quantity` |
| `status` | select | Status | yes |  |  | `purpose`: `simulation_status`; `allowed_values`: `Authorized`, `Implemented`, `Rejected`, `Under Evaluation` |
| `supplylocation` | select | Supply Location | no |  |  | `purpose`: `supply_location` |
| `supplylocationdate` | date | Supply Date | no |  |  | `purpose`: `supply_date` |
| `supplylocationsubsidiary` | select | Supply Subsidiary | no |  |  | `purpose`: `supply_subsidiary` |
| `transactiontype` | select | Transaction Type | yes |  |  | `purpose`: `simulation_transaction_type`; `allowed_values`: `Purchase Order`, `Sales Order`, `Transfer Order`, `Work Order` |
| `customform` | select | Custom Form | no |  |  |  |
| `id` | integer | Simulation Number | no |  |  |  |
| `isrecommendation` | checkbox |  | no |  |  |  |
| `scompid` | text |  | no |  |  |  |
| `templatestored` | checkbox |  | no |  |  |  |
