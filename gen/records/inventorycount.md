# Inventory Count (`inventorycount`)
<!-- id: record:inventorycount -->

| Fact | Value |
|---|---|
| Internal ID | `inventorycount` |
| Category | transaction |
| Client scripts | yes |
| Server scripts | yes |
| Searchable | yes |
| Search data via | Transaction |
| Tags | `custom_fields_supported`, `scripting_full` |

## Record facts

- `record_actions`: `startcount:open_to_started`, `completecount:started_to_completed`, `approve:creates_adjustments_on_mismatch`, `reject`
- `governance_note`: `startcount:20`, `completecount:5`, `approve:60`
- `returns_detail`: `inventory_count_id`
- `governance_note`: `5` usage_units [`reject_action`]
- `effect`: `reject_returns_completed_count_to_open`
- `ui_location`: `start_count_complete_count_approve_reject_buttons`
- `doc_inconsistency`: `parameter_notes_name_allocate_action`
- `script_types`: `client`, `server`
- `script_functions`: `create`, `update`, `copy`, `delete`, `search`
- `user_events`: `beforeLoad`, `beforeSubmit`, `afterSubmit`
- `requires_feature`: `Inventory Count` [`real_name`]
- `related` (refs `N/action`)
- `cannot`: `update_status_field`
- `purpose`: `inventory_accuracy_and_asset_control`
- `configured_by`: `enable_features_items_and_inventory_subtab` [`inventory_count`]
- `ui_location`: `transactions_inventory_enter_inventory_count`
- `workaround`: `start_count_button_in_ui_sets_status`
- `workflow`: `create_count_in_dynamic_mode`, `beforeload_adds_inventory_detail_button`, `client_https_call_to_suitelet_or_restlet_then_refresh`, `suitelet_or_restlet_adds_inventory_detail_subrecord` (when `lot_or_serial_item_count_with_inventory_detail`)
- `doc_inconsistency`: `step_1_text_mentions_cost_revaluation_records`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `account` | select | Account | yes | no |  | `purpose`: `count_variance_account`; `recommendation`: `expense_account` |
| `class` | select | Class | no | no |  | `purpose`: `class`; `caveat`: `distinct_from_inventory_count_classification` |
| `department` | select | Department | no | no |  | `purpose`: `department` |
| `location` | select | Location | yes | no |  | `purpose`: `location_filter` |
| `memo` | text | Memo | no | yes |  | `purpose`: `memo`; `supported_in`: `search` |
| `subsidiary` | select | Subsidiary | yes | no |  | `purpose`: `subsidiary` |
| `trandate` | date | Date | yes | no |  | `purpose`: `transaction_date` |
| `tranid` | text | Ref No. | no | no |  | `purpose`: `reference_number` |
| `createddate` | datetime |  | no | no |  |  |
| `externalid` | text | ExternalId | no | no |  |  |
| `lastmodifieddate` | datetime |  | no | no |  |  |
| `status` | text |  | no | no |  |  |
| `statusRef` | text |  | no | no |  |  |

## Sublist `item` (Items)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `adjustedquantity` | float | Adjusted Quantity | no |  |  |
| `binnumber` | select | Bin | no |  |  |
| `countdetail` | summary | Count Detail | no |  |  |
| `countquantity` | float | Count Quantity | no |  |  |
| `item` | select | Item | yes |  |  |
| `memo` | text | Memo | no |  |  |
| `rate` | currency2 | Rate | no |  |  |
| `snapshotquantity` | float | Snapshot Quantity | no |  |  |
| `units` | select | Units | no |  |  |
| `viewadjustment` | text | Variance Detail | no |  |  |
| `viewsnapshot` | text | Snapshot Detail | no |  |  |

## Tabs

| Tab ID | Label |
|---|---|
| `history` | History |
| `workflow` | Workflow |

## Suspected source errors

> [!SOURCE-ERROR] `parameter_notes_name_allocate_action`
<!-- source-error: record:inventorycount | - | parameter_notes_name_allocate_action -->

> [!SOURCE-ERROR] `step_1_text_mentions_cost_revaluation_records`
<!-- source-error: record:inventorycount | - | step_1_text_mentions_cost_revaluation_records -->
