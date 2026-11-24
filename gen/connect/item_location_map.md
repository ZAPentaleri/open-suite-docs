# Item_location_map (`item_location_map`)
<!-- id: connect:item_location_map -->

| Fact | Value |
|---|---|
| Table | `item_location_map` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `atp_lead_time` | NUMBER |  | 39 | 0 |  |  | `purpose`: `apt_lead_time`; `doc_inconsistency`: `apt_likely_means_atp` |
| `available_count` | NUMBER |  | 39 | 0 |  |  | `purpose`: `available_count` |
| `average_cost` | NUMBER |  | 30 | 15 |  |  | `purpose`: `average_cost` |
| `backward_consumption_days` | NUMBER |  | 39 | 0 |  |  | `purpose`: `backward_consumption_days` |
| `cost_0` | NUMBER |  | 30 | 15 |  |  | `purpose`: `cost` |
| `cost_accounting_status` | VARCHAR2 | 4000 |  |  |  |  | `purpose`: `cost_accounting_status` |
| `costing_lot_size` | NUMBER |  | 39 | 0 |  |  | `purpose`: `costing_lot_size` |
| `default_return_cost` | NUMBER |  | 30 | 15 |  |  | `purpose`: `default_return_cost` |
| `demand_source` | VARCHAR2 | 50 |  |  |  |  | `purpose`: `demand_source` |
| `demand_time_fence` | NUMBER |  | 39 | 0 |  |  | `purpose`: `demand_time_fence` |
| `fixed_lot_size` | NUMBER |  | 39 | 0 |  |  | `purpose`: `fixed_lot_size` |
| `forward_consumption_days` | NUMBER |  | 39 | 0 |  |  | `purpose`: `forward_consumption_days` |
| `in_transit_count` | NUMBER |  | 18 | 8 |  |  | `purpose`: `in_transit_count` |
| `inventory_cost_template` | NUMBER |  | 39 | 0 |  |  | `purpose`: `inventory_cost_template` |
| `invt_count_classification` | NUMBER |  | 39 | 0 |  |  | `purpose`: `inventory_count_classification` |
| `invt_count_interval` | NUMBER |  | 39 | 0 |  |  | `purpose`: `inventory_count_interval` |
| `item_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `item_id` |
| `last_invt_count_date` | TIMESTAMP |  |  |  |  |  | `purpose`: `last_inventory_count_date` |
| `last_purchase_price` | NUMBER |  | 39 | 0 |  |  | `purpose`: `last_purchase_price` |
| `location_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `location_id` |
| `lot_sizing_method` | VARCHAR2 | 50 |  |  |  |  | `purpose`: `lot_sizing_method` |
| `next_invt_count_date` | TIMESTAMP |  |  |  |  |  | `purpose`: `next_inventory_count_date` |
| `ns_lead_time` | NUMBER |  | 39 | 0 |  |  | `purpose`: `netsuite_lead_time` |
| `on_hand_count` | NUMBER |  | 21 | 11 |  |  | `purpose`: `on_hand_count` |
| `on_hand_value` | NUMBER |  | 29 | 11 |  |  | `purpose`: `on_hand_value` |
| `on_order_count` | NUMBER |  | 21 | 11 |  |  | `purpose`: `on_order_count` |
| `periodic_lot_size_days` | NUMBER |  | 39 | 0 |  |  | `purpose`: `periodic_lot_size_days` |
| `periodic_lot_size_type` | VARCHAR2 | 50 |  |  |  |  | `purpose`: `periodic_lot_size_type` |
| `pref_stock_level` | NUMBER |  | 39 | 0 |  |  | `purpose`: `preferred_stock_level` |
| `quantitybackordered` | NUMBER |  | 39 | 0 |  |  | `purpose`: `quantity_back_ordered` |
| `reorder_point` | NUMBER |  | 21 | 11 |  |  | `purpose`: `reorder_point` |
| `reschedule_in_days` | NUMBER |  | 39 | 0 |  |  | `purpose`: `reschedule_in_days` |
| `reschedule_out_days` | NUMBER |  | 39 | 0 |  |  | `purpose`: `reschedule_out_days` |
| `safety_stock_level` | NUMBER |  | 39 | 0 |  |  | `purpose`: `safety_stock_level` |
| `supply_time_fence` | NUMBER |  | 39 | 0 |  |  | `purpose`: `supply_time_fence` |
| `supply_type` | VARCHAR2 | 50 |  |  |  |  | `purpose`: `supply_type` |
| `wip` | VARCHAR2 | 1 |  |  |  |  | `purpose`: `wip` |

## Suspected source errors

> [!SOURCE-ERROR] `apt_likely_means_atp` (on `atp_lead_time`)
<!-- source-error: connect:item_location_map | atp_lead_time | apt_likely_means_atp -->
