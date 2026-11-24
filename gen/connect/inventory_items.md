# Inventory_items (`inventory_items`)
<!-- id: connect:inventory_items -->

| Fact | Value |
|---|---|
| Table | `inventory_items` |
| Record | `inventoryitem` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `allow_drop_ship` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `drop_ship_item` |
| `alt_demand_source_item_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `alternative_demand_source_item` |
| `asset_account_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `asset_account_id` |
| `atp_lead_time` | NUMBER |  | 39 | 0 |  |  | `purpose`: `atp_lead_time` |
| `atp_method` | VARCHAR2 | 150 |  |  |  |  | `purpose`: `atp_method` |
| `averagecost` | NUMBER |  | 30 | 15 |  |  | `purpose`: `average_cost` |
| `backward_consumption_days` | NUMBER |  | 39 | 0 |  |  | `purpose`: `backward_consumption_days` |
| `bill_exch_rate_var_account_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `bill_exchange_rate_variance_account` |
| `bill_price_variance_account_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `bill_price_variance_account` |
| `bill_qty_variance_account_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `bill_quantity_variance_account` |
| `conversion_rate` | NUMBER |  | 18 | 8 |  |  | `purpose`: `unused_column`; `caveat`: `column_returns_no_results`; `avoid_when`: `writing_queries` |
| `cost_0` | NUMBER |  | 30 | 15 |  |  | `purpose`: `cost` |
| `cost_accounting_status` | VARCHAR2 | 4000 |  |  |  |  | `purpose`: `cost_accounting_status` |
| `cost_category` | VARCHAR2 | 50 |  |  |  |  | `purpose`: `cost_category` |
| `costing_method` | VARCHAR2 | 16 |  |  |  |  | `purpose`: `costing_method` |
| `create_plan_on_event_type` | VARCHAR2 | 128 |  |  |  |  | `purpose`: `create_revenue_plan_on_event_type` |
| `created` | TIMESTAMP |  |  |  |  |  | `purpose`: `created`; `time_zone`: `gmt` |
| `current_on_order_count` | NUMBER |  | 21 | 11 |  |  | `purpose`: `current_on_order_count` |
| `custreturn_variance_account_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `customer_return_variance_account` |
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `date_last_modified` |
| `date_of_last_transaction` | TIMESTAMP |  |  |  |  |  | `purpose`: `date_of_last_transaction` |
| `default_return_cost` | NUMBER |  | 30 | 15 |  |  | `purpose`: `default_return_cost` |
| `demand_source` | VARCHAR2 | 50 |  |  |  |  | `purpose`: `demand_source` |
| `demand_time_fence` | NUMBER |  | 39 | 0 |  |  | `purpose`: `demand_time_fence` |
| `deposit` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `deposit` |
| `displayname` | VARCHAR2 | 4000 |  |  |  |  | `purpose`: `store_display_name` |
| `distribution_category` | VARCHAR2 | 60 |  |  |  |  | `purpose`: `distribution_category` |
| `distribution_network` | VARCHAR2 | 100 |  |  |  |  | `purpose`: `distribution_network` |
| `dropship_expense_account_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `dropship_expense_account_id` |
| `expense_account_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `expense_account` |
| `featureddescription` | VARCHAR2 | 65534 |  |  |  |  | `purpose`: `featured_description` |
| `fixed_lot_size` | NUMBER |  | 39 | 0 |  |  | `purpose`: `fixed_lot_size` |
| `forward_consumption_days` | NUMBER |  | 39 | 0 |  |  | `purpose`: `forward_consumption_days` |
| `fraud_risk` | VARCHAR2 | 128 |  |  |  |  | `purpose`: `fraud_risk` |
| `full_name` | VARCHAR2 | 550 |  |  |  |  | `purpose`: `full_name` |
| `fx_adjustment_account_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `fx_adjustment_account` |
| `gain_loss_account_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `gain_loss_account_id` |
| `handling_cost` | NUMBER |  | 30 | 15 |  |  | `purpose`: `handling_cost` |
| `hazmat` | VARCHAR2 | 1 |  |  |  |  | `purpose`: `hazmat` |
| `hazmat_hazard_class` | VARCHAR2 | 50 |  |  |  |  | `purpose`: `hazmat_hazard_class` |
| `hazmat_id` | VARCHAR2 | 50 |  |  |  |  | `purpose`: `hazmat_id` |
| `hazmat_item_units` | VARCHAR2 | 20 |  |  |  |  | `purpose`: `hazmat_item_units` |
| `hazmat_item_units_qty` | NUMBER |  | 39 | 0 |  |  | `purpose`: `hazmat_item_units_quantity` |
| `hazmat_packing_group` | VARCHAR2 | 10 |  |  |  |  | `purpose`: `hazmat_packing_group` |
| `hazmat_shipping_name` | VARCHAR2 | 100 |  |  |  |  | `purpose`: `hazmat_shipping_name` |
| `include_child_subsidiaries` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `include_child_subsidiaries` |
| `income_account_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `income_account_id` |
| `interco_expense_account_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `intercompany_expense_account_id` |
| `interco_income_account_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `intercompany_income_account_id` |
| `invt_count_classification` | NUMBER |  | 39 | 0 |  |  | `purpose`: `inventory_count_classification` |
| `invt_count_interval` | NUMBER |  | 39 | 0 |  |  | `purpose`: `inventory_count_interval` |
| `is_catch_weight_enabled` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `unused_column`; `caveat`: `column_returns_no_results`; `avoid_when`: `writing_queries` |
| `is_cont_rev_handling` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `item_contingent_revenue_handling` |
| `is_hold_rev_rec` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `hold_revenue_recognition_plan` |
| `is_moss` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `moss_taxation_used` |
| `isinactive` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `item_is_inactive` |
| `isonline` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `display_in_web_site` |
| `istaxable` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `taxable` |
| `item_extid` | VARCHAR2 | 255 |  |  |  |  | `purpose`: `item_external_id` |
| `item_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `item_id` |
| `item_revenue_category` | VARCHAR2 | 250 |  |  |  |  | `purpose`: `item_revenue_category` |
| `last_cogs_correction` | TIMESTAMP |  |  |  |  |  | `purpose`: `last_cogs_correction` |
| `last_invt_count_date` | TIMESTAMP |  |  |  |  |  | `purpose`: `last_inventory_count_date` |
| `last_purchase_price` | NUMBER |  | 39 | 0 |  |  | `purpose`: `last_purchase_price` |
| `lot_numbered_item` | VARCHAR2 | 1 |  |  |  |  | `purpose`: `lot_numbered_item` |
| `lot_sizing_method` | VARCHAR2 | 50 |  |  |  |  | `purpose`: `lot_sizing_method` |
| `match_bill_to_receipt` | VARCHAR2 | 1 |  |  |  |  | `purpose`: `match_bill_to_receipt` |
| `matrix_type` | VARCHAR2 | 6 |  |  |  |  | `purpose`: `matrix_item_type`; `options`: `parent`, `child`; `caveat`: `empty_for_non_matrix_items` |
| `modified` | TIMESTAMP |  |  |  |  |  | `purpose`: `modified`; `time_zone`: `gmt` |
| `name` | VARCHAR2 | 250 |  |  |  |  | `purpose`: `name` |
| `next_invt_count_date` | TIMESTAMP |  |  |  |  |  | `purpose`: `next_inventory_count_date` |
| `ns_lead_time` | NUMBER |  | 39 | 0 |  |  | `purpose`: `netsuite_lead_time` |
| `offersupport` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `offer_support` |
| `onspecial` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `on_special` |
| `parent_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `subitem_of` |
| `payment_method_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `payment_method_id` |
| `periodic_lot_size_days` | NUMBER |  | 39 | 0 |  |  | `purpose`: `periodic_lot_size_days` |
| `periodic_lot_size_type` | VARCHAR2 | 50 |  |  |  |  | `purpose`: `periodic_lot_size_type` |
| `pref_purchase_tax_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `preferred_purchase_tax_id` |
| `pref_sale_tax_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `preferred_sales_tax_id` |
| `pref_stock_level` | NUMBER |  | 39 | 0 |  |  | `purpose`: `preferred_stock_level` |
| `prices_include_tax` | VARCHAR2 | 1 |  |  |  |  | `purpose`: `prices_include_tax` |
| `pricing_group_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `pricing_group_id` |
| `print_sub_items` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `print_sub_items` |
| `prod_price_var_account_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `production_price_variance_account` |
| `prod_qty_var_account_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `production_quantity_variance_account` |
| `purchase_price_var_account_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `purchase_price_variance_account` |
| `purchase_unit_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `purchase_unit_id` |
| `purchasedescription` | VARCHAR2 | 4000 |  |  |  |  | `purpose`: `purchase_description` |
| `quantityavailable` | NUMBER |  | 39 | 0 |  |  | `purpose`: `quantity_available` |
| `quantitybackordered` | NUMBER |  | 39 | 0 |  |  | `purpose`: `quantity_back_ordered` |
| `quantityonhnad` | NUMBER |  | 21 | 11 |  |  | `purpose`: `quantity_on_hand` |
| `reorder_multiple` | NUMBER |  | 39 | 0 |  |  | `purpose`: `re_order_multiple` |
| `reorderpoint` | NUMBER |  | 21 | 11 |  |  | `purpose`: `reorder_point` |
| `replenishment_method` | VARCHAR2 | 50 |  |  |  |  | `purpose`: `replenishment_method` |
| `reschedule_in_days` | NUMBER |  | 39 | 0 |  |  | `purpose`: `reschedule_in_days` |
| `reschedule_out_days` | NUMBER |  | 39 | 0 |  |  | `purpose`: `reschedule_out_days` |
| `rev_rec_forecast_rule_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `revenue_recognition_forecast_rule` |
| `rev_rec_rule_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `revenue_recognition_rule_id` |
| `revenue_allocation_group` | VARCHAR2 | 60 |  |  |  |  | `purpose`: `revenue_allocation_group` |
| `round_up_as_component` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `round_up_as_component` |
| `safety_stock_days` | NUMBER |  | 39 | 0 |  |  | `purpose`: `safety_stock_days` |
| `safety_stock_level` | NUMBER |  | 39 | 0 |  |  | `purpose`: `safety_stock_level` |
| `sale_unit_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `sale_unit_id` |
| `salesdescription` | VARCHAR2 | 4000 |  |  |  |  | `purpose`: `sales_description` |
| `salesprice` | VARCHAR2 | 42 |  |  |  |  | `purpose`: `sales_price` |
| `scrap_account_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `scrap_account_id` |
| `secondary_units_type_id` | NUMBER |  | 39 | 0 | `units_type`.`units_type_id` | foreign_key | `purpose`: `unused_column`; `caveat`: `column_returns_no_results`; `avoid_when`: `writing_queries` |
| `serialized_item` | VARCHAR2 | 1 |  |  |  |  | `purpose`: `serialized_item` |
| `shipping_cost` | NUMBER |  | 30 | 15 |  |  | `purpose`: `shipping_cost` |
| `specialsdescription` | VARCHAR2 | 65534 |  |  |  |  | `purpose`: `specials_description` |
| `stock_unit_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `stock_unit_id` |
| `storedescription` | VARCHAR2 | 4000 |  |  |  |  | `purpose`: `store_description` |
| `storedetaileddescription` | VARCHAR2 | 65534 |  |  |  |  | `purpose`: `detailed_description` |
| `storedisplayname` | VARCHAR2 | 500 |  |  |  |  | `purpose`: `store_display_name` |
| `subtype` | VARCHAR2 | 10 |  |  |  |  | `purpose`: `subtype` |
| `supply_time_fence` | NUMBER |  | 39 | 0 |  |  | `purpose`: `supply_time_fence` |
| `supply_type` | VARCHAR2 | 50 |  |  |  |  | `purpose`: `supply_type` |
| `tax_item_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `tax_item_id` |
| `totalvalue` | NUMBER |  | 29 | 11 |  |  | `purpose`: `total_value` |
| `transferprice` | NUMBER |  | 30 | 15 |  |  | `purpose`: `transfer_price` |
| `units_type_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `item_units_type` |
| `upc_code` | VARCHAR2 | 180 |  |  |  |  | `purpose`: `upc_code` |
| `upper_warning_limit` | NUMBER |  | 18 | 8 |  |  | `purpose`: `unused_column`; `caveat`: `column_returns_no_results`; `avoid_when`: `writing_queries` |
| `use_component_yield` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `use_component_yield` |
| `vendor_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `vendor_id` |
| `vendorname` | VARCHAR2 | 250 |  |  |  |  | `purpose`: `vendor_name` |
| `vendreturn_variance_account_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `vendor_return_variance_account` |
| `vsoe_deferral` | VARCHAR2 | 28 |  |  |  |  | `purpose`: `vsoe_deferral` |
| `vsoe_delivered` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `vsoe_delivered` |
| `vsoe_discount` | VARCHAR2 | 12 |  |  |  |  | `purpose`: `vsoe_discount` |
| `vsoe_price` | NUMBER |  | 30 | 15 |  |  | `purpose`: `vsoe_price` |
| `weight` | NUMBER |  | 21 | 11 |  |  | `purpose`: `weight` |
| `weight_in_user_defined_unit` | NUMBER |  | 39 | 0 |  |  | `purpose`: `weight_in_user_defined_limit` |
| `weight_unit_index` | NUMBER |  | 39 | 0 |  |  | `purpose`: `weight_unit_index` |
| `wip_account_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `wip_account_id` |
| `wip_cost_variance_account_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `wip_cost_variance_account` |
| `work_order_lead_time` | NUMBER |  | 39 | 0 |  |  | `purpose`: `work_order_lead_time` |
| `lower_warning_limit` | NUMBER |  | 18 | 8 |  |  |  |

## Foreign keys

| Key | Columns | References table | Referenced columns |
|---|---|---|---|
| `inventory_items_units_type_fk` | `secondary_units_type_id` | `units_type` | `units_type_id` |
