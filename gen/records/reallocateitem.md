# Reallocate Items (`reallocateitem`)
<!-- id: record:reallocateitem -->

| Fact | Value |
|---|---|
| Internal ID | `reallocateitem` |
| Category | item |

## Record facts

- `script_types`: `user_event_only`
- `user_events`: `beforeLoad`, `beforeSubmit`, `afterSubmit`
- `script_functions`: `copy`, `search`
- `not_supported`: `create`, `update`, `delete`
- `auto_commit_when`: `sales_order_created_or_approved`, `backorder_goods_received`, `assembly_work_order_output`
- `not_supported`: `load`
- `doc_inconsistency`: `supported_records_table_id_capitalized_reallocateItem` [`needs_human_review`]

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `item` | select | Item | yes | no |  | `purpose`: `item_to_reallocate`; `allowed_values_listed_in`: `item_records`; `ui_effect`: `new_option_creates_record` |
| `location` | select | Location | yes | no |  | `purpose`: `transaction_location`; `allowed_values_listed_in`: `location_records`; `ui_effect`: `new_option_creates_record` |
| `quantitycommitted` | float | Quantity Committed | no | no |  | `purpose`: `quantity_committed`; `value_source`: `system_displayed` |
| `quantityonhand` | float | Quantity On Hand | no | no |  | `purpose`: `quantity_on_hand`; `value_source`: `system_displayed` |
| `quantitypicked` | float | Quantity Picked | no | no |  | `purpose`: `quantity_picked`; `requires_feature`: `Pick, Pack and Ship` [`real_name`]; `value_scope`: `per_location` (when `multi_location_inventory`) |
| `quantityrequired` | float | Quantity Required | no | no |  | `purpose`: `quantity_required`; `value_source`: `committed_plus_backorders` |
| `units` | text | Units | no | no |  | `purpose`: `unit_name`; `requires_feature`: `Multiple Units of Measure` [`real_name`] |
| `externalid` | text | ExternalId | no | no |  |  |

## Sublist `order` (Orders)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `commit` | checkbox | Commit | no |  |  |
| `numberedquantityremaining` | float | Numbered Quantity Remaining | no |  |  |
| `quantitycommitted` | posfloat | Quantity Committed | no |  |  |
| `quantitypicked` | float | Quantity Picked | no |  |  |
| `quantityremaining` | float | Quantity Remaining | no |  |  |

## Suspected source errors

> [!SOURCE-ERROR] `supported_records_table_id_capitalized_reallocateItem`
<!-- source-error: record:reallocateitem | - | supported_records_table_id_capitalized_reallocateItem -->
