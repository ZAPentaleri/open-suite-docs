# Pick Strategy (`pickstrategy`)
<!-- id: record:pickstrategy -->

| Fact | Value |
|---|---|
| Internal ID | `pickstrategy` |
| Category | transaction |
| Client scripts | yes |
| Server scripts | yes |
| Searchable | yes |
| Tags | `scripting_full` |

## Record facts

- `script_types`: `client`, `server`
- `requires_feature`: `Warehouse Management` [`real_name`]
- `rule`: `customer_field_only_for_sales_orders`
- `location_filter`: `use_warehouse_management_locations`
- `purpose`: `filters_for_preferred_bin_in_pick_tasks`
- `effect`: `bins_sorted_when_wave_saved_or_released`
- `configured_by`: `inventory_status_feature` [`inventory_status_field`]
- `script_functions`: `create`, `copy`, `update`, `delete`, `search`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `customer` | select | Customer | no |  |  | `purpose`: `customer_criterion`; `applies_to`: `sales_orders` |
| `description` | textarea | Description | no |  |  | `purpose`: `description` |
| `inventorystatus` | select | Inventory Status | no |  |  | `purpose`: `bin_inventory_status_criterion` |
| `invtclassification` | select | Item Classification | no |  |  | `purpose`: `item_classification_criterion`; `caveat`: `optional_field` |
| `isinactive` | checkbox | Inactive | no |  |  | `purpose`: `inactivate_pick_strategy`; `ui_effect`: `hidden_from_lists_and_not_selectable` |
| `item` | select | Item | no |  |  | `purpose`: `item_criteria`; `options`: `Item Process Group`, `Item Process Family`, `Item`, `Units`; `help_text_shared_with`: `pickstrategy.itemprocessfamily`, `pickstrategy.itemprocessgroup`, `pickstrategy.units` |
| `itemprocessfamily` | select | Item Process Family | no |  |  | `purpose`: `item_criteria`; `options`: `Item Process Group`, `Item Process Family`, `Item`, `Units`; `help_text_shared_with`: `pickstrategy.item`, `pickstrategy.itemprocessgroup`, `pickstrategy.units` |
| `itemprocessgroup` | select | Item Process Group | no |  |  | `purpose`: `item_criteria`; `options`: `Item Process Group`, `Item Process Family`, `Item`, `Units`; `help_text_shared_with`: `pickstrategy.item`, `pickstrategy.itemprocessfamily`, `pickstrategy.units` |
| `location` | select | Location | yes |  |  | `purpose`: `transaction_location_criterion` |
| `name` | text | Name | yes |  |  | `purpose`: `pick_strategy_name`; `constraint`: `unique` |
| `ordertype` | select | Order Type | no |  |  | `purpose`: `order_type_criterion` |
| `priority` | float | Sequence Number | yes |  |  | `purpose`: `strategy_sequence_number` |
| `trantype` | select | Transaction Type | yes |  |  | `purpose`: `transaction_type_criterion` |
| `units` | select | Units | no |  |  | `purpose`: `item_criteria`; `options`: `Item Process Group`, `Item Process Family`, `Item`, `Units`; `help_text_shared_with`: `pickstrategy.item`, `pickstrategy.itemprocessfamily`, `pickstrategy.itemprocessgroup` |
| `descriptiontranslated` | textarea | Description (Translated) | no |  |  |  |
| `externalid` | text | External ID | no |  |  |  |
| `nametranslated` | text | Name (Translated) | no |  |  |  |

## Sublist `zones` (Zone)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `pickstrategy` | select | Pick Strategy | no |  |  |
| `priority` | float | Priority | no |  |  |
| `zone` | select | Zone | no |  |  |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `description` | textarea | Description |  |
| `descriptiontranslated` | textarea | Description (Translated) |  |
| `externalid` | select | External ID |  |
| `externalidstring` | text | External ID (Text) |  |
| `internalid` | select | Internal ID |  |
| `internalidnumber` | integer | Internal ID (Number) |  |
| `inventorystatus` | select | Inventory Status |  |
| `isinactive` | checkbox | Inactive |  |
| `item` | select | Item |  |
| `location` | select | Location |  |
| `name` | text | Name |  |
| `nametranslated` | text | Name (Translated) |  |
| `trantype` | select | Transaction Type |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `description` | textarea | Description |  |
| `descriptiontranslated` | textarea | Description (Translated) |  |
| `externalid` | select | External ID |  |
| `internalid` | select | Internal ID |  |
| `inventorystatus` | select | Inventory Status |  |
| `isinactive` | checkbox | Inactive |  |
| `item` | select | Item |  |
| `location` | select | Location |  |
| `name` | text | Name |  |
| `nametranslated` | text | Name (Translated) |  |
| `trantype` | select | Transaction Type |  |
| `unit` | select | Units |  |
