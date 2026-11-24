# Items (`items`)
<!-- id: connect:items -->

| Fact | Value |
|---|---|
| Table | `items` |
| Record | `item` |
| Primary key | `item_id` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `allow_drop_ship` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `drop_ship_item` |
| `alt_demand_source_item_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `alternative_demand_source_item` |
| `asset_account_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `asset_account_id` |
| `atp_lead_time` | NUMBER |  | 39 | 0 |  |  | `purpose`: `atp_lead_time` |
| `atp_method` | VARCHAR2 | 150 |  |  |  |  | `purpose`: `atp_method` |
| `available_to_partners` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `available_to_partners` |
| `averagecost` | NUMBER |  | 30 | 15 |  |  | `purpose`: `average_cost` |
| `backward_consumption_days` | NUMBER |  | 39 | 0 |  |  | `purpose`: `backward_consumption_days` |
| `bill_exch_rate_var_account_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `bill_exchange_rate_variance_account` |
| `bill_price_variance_account_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `bill_price_variance_account` |
| `bill_qty_variance_account_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `bill_quantity_variance_account` |
| `build_sub_assemblies` | VARCHAR2 | 1 |  |  |  |  | `purpose`: `build_sub_assemblies` |
| `class_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `class_id` |
| `consumption_unit_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `consumption_unit_id` |
| `cost_0` | NUMBER |  | 30 | 15 |  |  | `purpose`: `cost` |
| `cost_category` | VARCHAR2 | 50 |  |  |  |  | `purpose`: `cost_category` |
| `cost_estimate_type` | VARCHAR2 | 200 |  |  |  |  | `purpose`: `cost_estimate_type` |
| `costing_method` | VARCHAR2 | 16 |  |  |  |  | `purpose`: `costing_method` |
| `country_of_manufacture` | VARCHAR2 | 6 |  |  |  |  | `purpose`: `country_of_manufacture` |
| `create_plan_on_event_type` | VARCHAR2 | 128 |  |  |  |  | `purpose`: `create_revenue_plan_on_event_type` |
| `created` | TIMESTAMP |  |  |  |  |  | `purpose`: `date_created`; `time_zone`: `gmt` |
| `current_on_order_count` | NUMBER |  | 21 | 11 |  |  | `purpose`: `quantity_on_order` |
| `custreturn_variance_account_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `customer_return_variance_account` |
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `date_last_modified` |
| `date_of_last_transaction` | TIMESTAMP |  |  |  |  |  | `purpose`: `date_of_last_transaction` |
| `default_return_cost` | NUMBER |  | 30 | 15 |  |  | `purpose`: `default_return_cost` |
| `deferred_expense_account_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `deferred_expense_account` |
| `deferred_revenue_account_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `deferred_revenue_account` |
| `demand_source` | VARCHAR2 | 50 |  |  |  |  | `purpose`: `demand_source` |
| `demand_time_fence` | NUMBER |  | 39 | 0 |  |  | `purpose`: `demand_time_fence` |
| `department_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `department_id` |
| `deposit` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `deposit` |
| `displayname` | VARCHAR2 | 4000 |  |  |  |  | `purpose`: `store_display_name` |
| `distribution_category` | VARCHAR2 | 60 |  |  |  |  | `purpose`: `distribution_category` |
| `distribution_network` | VARCHAR2 | 100 |  |  |  |  | `purpose`: `distribution_network` |
| `dropship_expense_account_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `dropship_expense_account_id` |
| `effective_bom_control_type` | VARCHAR2 | 150 |  |  |  |  | `purpose`: `effective_bom_lot_type` |
| `expense_account_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `expense_account` |
| `featureddescription` | VARCHAR2 | 65534 |  |  |  |  | `purpose`: `featured_description` |
| `featureditem` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `featured_item` |
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
| `income_account_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `income_account` |
| `interco_expense_account_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `intercompany_expense_account_id` |
| `interco_income_account_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `intercompany_income_account_id` |
| `invt_count_classification` | NUMBER |  | 39 | 0 |  |  | `purpose`: `inventory_count_classification` |
| `invt_count_interval` | NUMBER |  | 39 | 0 |  |  | `purpose`: `inventory_count_interval` |
| `is_cont_rev_handling` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `item_contingent_revenue_handling`; `doc_inconsistency`: `item_column_help_says_book` |
| `is_enforce_min_qty_internally` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `enforce_minimum_quantity_on_all_orders` |
| `is_hold_rev_rec` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `hold_revenue_recognition_plan` |
| `is_moss` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `moss_taxation_required` |
| `is_phantom` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `item_produces_phantom_income` |
| `is_special_order_item` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `special_order_item` |
| `isinactive` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `item_is_inactive` |
| `isonline` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `display_in_web_site` |
| `istaxable` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `taxable` |
| `item_defined_cost` | NUMBER |  | 30 | 15 |  |  | `purpose`: `item_defined_cost` |
| `item_extid` | VARCHAR2 | 255 |  |  |  |  | `purpose`: `item_external_id` |
| `item_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `item_id` |
| `item_revenue_category` | VARCHAR2 | 250 |  |  |  |  | `purpose`: `item_revenue_category` |
| `last_cogs_correction` | TIMESTAMP |  |  |  |  |  | `purpose`: `last_cogs_correction` |
| `last_invt_count_date` | TIMESTAMP |  |  |  |  |  | `purpose`: `last_inventory_count_days` |
| `last_purchase_price` | NUMBER |  | 39 | 0 |  |  | `purpose`: `last_purchase_price` |
| `location_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `location_id` |
| `lot_numbered_item` | VARCHAR2 | 1 |  |  |  |  | `purpose`: `lot_numbered_item` |
| `lot_sizing_method` | VARCHAR2 | 50 |  |  |  |  | `purpose`: `lot_sizing_method` |
| `manufacturer` | VARCHAR2 | 200 |  |  |  |  | `purpose`: `manufacturer` |
| `manufacturing_charge_item` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `manufacturing_charge_item` |
| `match_bill_to_receipt` | VARCHAR2 | 1 |  |  |  |  | `purpose`: `match_bill_to_receipt` |
| `matrix_type` | VARCHAR2 | 6 |  |  |  |  | `purpose`: `matrix_item_type`; `options`: `parent`, `child`; `caveat`: `empty_for_non_matrix_items` |
| `maximum_quantity` | NUMBER |  | 39 | 0 |  |  | `purpose`: `maximum_quantity` |
| `metataghtml` | VARCHAR2 | 65534 |  |  |  |  | `purpose`: `meta_tag_html_field` |
| `minimum_quantity` | NUMBER |  | 39 | 0 |  |  | `purpose`: `minimum_quantity` |
| `modified` | TIMESTAMP |  |  |  |  |  | `purpose`: `modified`; `time_zone`: `gmt` |
| `mpn` | VARCHAR2 | 100 |  |  |  |  | `purpose`: `mpn` |
| `name` | VARCHAR2 | 250 |  |  |  |  | `purpose`: `name` |
| `next_invt_count_date` | TIMESTAMP |  |  |  |  |  | `purpose`: `next_inventory_count_days` |
| `ns_lead_time` | NUMBER |  | 39 | 0 |  |  | `purpose`: `netsuite_lead_time` |
| `offersupport` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `offer_support` |
| `onspecial` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `on_special` |
| `overhead_type` | VARCHAR2 | 100 |  |  |  |  | `purpose`: `overhead_type` |
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
| `purchaseorderamount` | NUMBER |  | 20 | 2 |  |  | `purpose`: `purchase_order_amount` |
| `purchaseorderquantity` | NUMBER |  | 18 | 8 |  |  | `purpose`: `purchase_order_quantity` |
| `purchaseorderquantitydiff` | NUMBER |  | 18 | 8 |  |  | `purpose`: `purchase_order_quantity_difference` |
| `quantityavailable` | NUMBER |  | 39 | 0 |  |  | `purpose`: `quantity_available` |
| `quantitybackordered` | NUMBER |  | 39 | 0 |  |  | `purpose`: `quantity_back_ordered` |
| `quantityonhand` | NUMBER |  | 21 | 11 |  |  | `purpose`: `quantity_on_hand` |
| `receiptamount` | NUMBER |  | 20 | 2 |  |  | `purpose`: `receipt_amount` |
| `receiptquantity` | NUMBER |  | 18 | 8 |  |  | `purpose`: `receipt_quantity` |
| `receiptquantitydiff` | NUMBER |  | 18 | 8 |  |  | `purpose`: `receipt_quantity_difference` |
| `reorder_multiple` | NUMBER |  | 39 | 0 |  |  | `purpose`: `re_order_multiple` |
| `reorderpoint` | NUMBER |  | 21 | 11 |  |  | `purpose`: `reorder_point` |
| `replenishment_method` | VARCHAR2 | 50 |  |  |  |  | `purpose`: `replenishment_method` |
| `resalable` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `resalable` |
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
| `serialized_item` | VARCHAR2 | 1 |  |  |  |  | `purpose`: `serialized_item` |
| `shippingcost` | NUMBER |  | 30 | 15 |  |  | `purpose`: `shipping_cost` |
| `special_work_order_item` | VARCHAR2 | 1 |  |  |  |  | `purpose`: `special_work_order_item` |
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
| `type_name` | VARCHAR2 | 4000 |  |  |  |  | `purpose`: `item_type_name`; `constraint`: `filter_value_must_match_ui_item_type_name_exactly`; `allowed_values_listed_in`: `item_type_choices_on_new_item_ui`; `caveat`: `ui_names_may_differ_with_advanced_bom` |
| `unbuild_variance_account_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `un_build_variance_account_id` |
| `units_type_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `item_units_type` |
| `upc_code` | VARCHAR2 | 180 |  |  |  |  | `purpose`: `upc_code` |
| `use_component_yield` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `use_component_yield` |
| `vendor_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `vendor_id` |
| `vendorname` | VARCHAR2 | 250 |  |  |  |  | `purpose`: `vendor_name` |
| `vendreturn_variance_account_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `vendor_return_variance_account` |
| `vsoe_deferral` | VARCHAR2 | 28 |  |  |  |  | `purpose`: `vsoe_deferral` |
| `vsoe_delivered` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `vsoe_delivered` |
| `vsoe_discount` | VARCHAR2 | 12 |  |  |  |  | `purpose`: `vsoe_discount` |
| `vsoe_price` | NUMBER |  | 30 | 15 |  |  | `purpose`: `vsoe_price` |
| `weight` | NUMBER |  | 21 | 11 |  |  | `purpose`: `item_weight` |
| `weight_in_user_defined_unit` | NUMBER |  | 39 | 0 |  |  | `purpose`: `weight_in_user_defined_unit` |
| `weight_unit_index` | NUMBER |  | 39 | 0 |  |  | `purpose`: `weight_unit_index` |
| `wip_account_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `wip_account_id` |
| `wip_cost_variance_account_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `wip_cost_variance_account` |
| `work_order_lead_time` | NUMBER |  | 39 | 0 |  |  | `purpose`: `work_order_lead_time` |

## Referenced by

| Key | Table | Columns | Columns here |
|---|---|---|---|
| `Assembly_boms_items_fk` | `assembly_boms` | `assembly_id` | `item_id` |
| `Bill_of_materials_items_fk` | `bill_of_materials` | `original_assembly_id` | `item_id` |
| `Billing_rate_cards_prices_items_fk` | `billing_rate_cards_prices` | `service_item_id` | `item_id` |
| `Billing_subscription_lines_items_fk` | `billing_subscription_lines` | `item_id` | `item_id` |
| `Bom_revision_components_items_fk` | `bom_revision_components` | `item_id` | `item_id` |
| `Budget_items_fk` | `budget` | `item_id` | `item_id` |
| `Campaignitem_items_fk` | `campaignitem` | `item_id` | `item_id` |
| `Customers_items_fk` | `customers` | `tax_item_id` | `item_id` |
| `Inbound_shipment_items_items_fk` | `inbound_shipment_items` | `item_id` | `item_id` |
| `Item_collection_item_map_items_fk` | `item_collection_item_map` | `item_id` | `item_id` |
| `Item_default_secondary_units_items_fk` | `item_default_secondary_units` | `item_id` | `item_id` |
| `Item_group_items_fk` | `item_group` | `parent_id` | `item_id` |
| `Item_price_history_items_fk` | `item_price_history` | `item_id` | `item_id` |
| `Item_subsidiary_map_items_fk` | `item_subsidiary_map` | `item_id` | `item_id` |
| `Item_vendor_map_items_fk` | `item_vendor_map` | `item_id` | `item_id` |
| `Item_vendor_pricing_items_fk` | `item_vendor_pricing` | `item_id` | `item_id` |
| `Notes_user_items_fk` | `notes_user` | `item_id` | `item_id` |
| `Opportunity_lines_items_fk` | `opportunity_lines` | `item_id` | `item_id` |
| `Opportunity_lines_items_fk_2` | `opportunity_lines` | `tax_item_id` | `item_id` |
| `Planned_standard_costs_items_fk` | `planned_standard_costs` | `item_id` | `item_id` |
| `Posting_account_activity_items_fk` | `posting_account_activity` | `item_id` | `item_id` |
| `Posting_account_activity_pe_items_fk` | `posting_account_activity_pe` | `item_id` | `item_id` |
| `Purchase_charge_rules_items_fk` | `purchase_charge_rules` | `mark_up_item_id` | `item_id` |
| `Standard_cost_components_items_fk` | `standard_cost_components` | `component_id` | `item_id` |
| `Subscription_co_lines_items_fk` | `subscription_co_lines` | `plan_item_id` | `item_id` |
| `Subscription_plan_lines_items_fk` | `subscription_plan_lines` | `item_id` | `item_id` |
| `Support_incidents_items_fk` | `support_incidents` | `item_id` | `item_id` |
| `System_notes_custom_items_fk` | `system_notes_custom` | `item_id` | `item_id` |
| `System_notes_items_fk` | `system_notes` | `item_id` | `item_id` |
| `Transaction_tax_detail_items_fk` | `transaction_tax_detail` | `tax_item_id` | `item_id` |

## Suspected source errors

> [!SOURCE-ERROR] `item_column_help_says_book` (on `is_cont_rev_handling`)
<!-- source-error: connect:items | is_cont_rev_handling | item_column_help_says_book -->
