# Serialized Build/Assembly Item (`serializedassemblyitem`)
<!-- id: record:serializedassemblyitem -->

| Fact | Value |
|---|---|
| Internal ID | `serializedassemblyitem` |
| Category | item |
| Client scripts | yes |
| Server scripts | yes |
| Searchable | yes |
| Search data via | Item |
| Tags | `custom_fields_supported`, `scripting_full` |

## Record facts

- `script_types`: `client`, `server`
- `script_functions`: `create`, `update`, `copy`, `delete`, `search`
- `has_pricing_sublist`: `true` (refs `topic:records_guide.pricing_sublist`)
- `purpose`: `assemblies_tracked_by_serial_number`
- `applies_to`: `sales_transactions`, `inventory_adjustments`
- `not_supported`: `purchase_transactions`

## Body fields

| Field ID | Type | Label | Required | submitField | Read-only | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `alternatedemandsourceitem` | select | Alternate Source Item | no | no |  |  | `purpose`: `alternate_item_for_demand_history`; `default_behavior`: `own_item_history` (when `blank`); `constraint`: `same_item_type`; `value_examples`: `item_b_history_used_for_item_a` |
| `assetaccount` | select | Asset Account | no | no |  |  | `purpose`: `inventory_asset_account`; `allowed_values_listed_in`: `asset_accounts`; `ui_effect`: `new_option_creates_record` |
| `atpmethod` | select | Default ATP Method | yes | no |  |  | `purpose`: `default_atp_method`; `options`: `discrete_atp`, `cumulative_atp_look_ahead`; `option_meanings`: `discrete:available_date_per_supply_order`, `cumulative:cumulative_supply_within_horizon`; `value_source`: `future_dated_non_posting_orders` |
| `autoleadtime` | checkbox | Auto-Calculate | no | yes |  |  | `purpose`: `auto_calculate_lead_time`; `true_means`: `average_of_three_latest_purchase_orders`; `caveat`: `first_receipt_per_order_used`; `caveat`: `not_weighted_by_units_received`; `caveat`: `orders_without_receipts_ignored`; `default_behavior`: `inventory_management_setup_value` (when `cleared_and_blank`); `doc_inconsistency`: `autoleadtime_first_receipt_vs_leadtime_last_receipt` [`needs_human_review`] |
| `autopreferredstocklevel` | checkbox | Auto-Calculate | no | yes |  |  | `purpose`: `auto_calculate_preferred_stock_level`; `value_source`: `daily_demand_times_days_supply` (when `auto_calculate`); `effect`: `drives_order_items_replenishment_quantity`; `default_behavior`: `inventory_management_setup_value` (when `not_set`); `value_kind`: `quantity_in_stock_after_order` |
| `autoreorderpoint` | checkbox | Auto-Calculate | no | yes |  |  | `purpose`: `auto_calculate_reorder_point`; `value_source`: `lead_time_days_times_demand` (when `no_safety_stock`); `value_source`: `lead_plus_safety_days_times_demand` (when `safety_stock_days`); `value_source`: `lead_days_times_demand_plus_safety_qty` (when `safety_stock_quantity`); `configured_by`: `inventory_level_warnings_preference` [`reminder`]; `false_means`: `reorder_point_entered_manually` |
| `availabletopartners` | checkbox | Available to Adv. Partners | no | no |  |  | `purpose`: `visible_to_advanced_partner_center`; `applies_to`: `advanced_partner_center_roles`, `custom_roles_with_item_access` |
| `averagecost` | currency | Average Cost | no | no |  |  | `purpose`: `average_cost_across_locations`; `value_source`: `calculated_weighted_average`; `value_unit`: `stock_units` (when `multiple_units_of_measure`); `caveat`: `per_location_cost_on_locations_sublist` |
| `billexchratevarianceacct` | select | Bill Exchange Rate Variance Account | no | no |  |  | `purpose`: `exchange_rate_variance_account`; `effect`: `account_changes_logged_in_system_notes` |
| `billingschedule` | select | Billing Schedule | no | no |  |  | `purpose`: `billing_schedule`; `effect`: `default_schedule_on_order_lines`; `requires`: `form_customized_to_show_line_schedules`; `allowed_values_listed_in`: `billing_schedule_records`; `ui_effect`: `new_option_creates_record` |
| `billpricevarianceacct` | select | Bill Price Variance Account | no | no |  |  | `purpose`: `bill_price_variance_account`; `effect`: `account_changes_logged_in_system_notes` |
| `billqtyvarianceacct` | select | Bill Quantity Variance Account | no | no |  |  | `purpose`: `bill_quantity_variance_account`; `effect`: `account_changes_logged_in_system_notes` |
| `buildentireassembly` | checkbox | Build Sub-Assemblies | no | no |  |  | `purpose`: `build_entire_assembly`; `effect`: `work_orders_build_subassemblies_by_default`; `effect`: `subassemblies_marked_phantom`; `can`: `override_on_work_order_or_item_source` |
| `buildtime` | float | Work Order Variable Lead Time | no | yes |  |  | `purpose`: `work_order_lead_time`; `value_unit`: `days`; `applies_to`: `one_base_unit` |
| `class` | select | Class | no | yes |  |  | `purpose`: `item_class`; `allowed_values_listed_in`: `class_records`; `ui_effect`: `new_option_creates_record` |
| `cogsaccount` | select | COGS Account | no | no |  |  | `purpose`: `cogs_account`; `allowed_values_listed_in`: `cogs_accounts`; `ui_effect`: `new_option_creates_record` |
| `consumptionunit` | select | Consumption Units | no | no |  |  | `purpose`: `default_inventory_unit`; `requires_feature`: `Multiple Units of Measure` [`real_name`] |
| `copydescription` | checkbox | Copy from Sales Order | no | no |  |  | `purpose`: `copy_sales_description_to_purchase`; `true_means`: `purchase_description_from_sales_order` |
| `cost` | currency2 | Purchase Price | no | no |  |  | `purpose`: `purchase_price`; `default_behavior`: `latest_purchase_price` (when `blank`); `value_unit`: `preferred_vendor_currency`; `value_unit`: `base_currency` (when `no_preferred_vendor`) |
| `costcategory` | select | Cost Category | yes | no |  |  | `purpose`: `cost_category`; `doc_inconsistency`: `help_describes_costing_method` |
| `costestimate` | currency | Item Defined Cost | no | no |  |  | `purpose`: `item_defined_cost` |
| `costestimatetype` | select | Cost Estimate Type | no | no |  |  | `purpose`: `cost_basis_for_estimated_gross_profit`; `options`: `Item Defined Cost`, `Average Cost`, `Last Purchase Price`, `Purchase Price`, `Preferred Vendor Rate`, `Derived from member items`; `requires_feature`: `Multi-Vendor` (when `preferred_vendor_rate`) [`real_name`]; `stage_order`: `preferred_vendor_rate`, `purchase_price`, `purchase_order_rate` (when `preferred_vendor_rate`); `applies_to`: `kits` (when `derived_from_member_items`); `effect`: `drives_transaction_estimated_gross_profit`; `option_meanings`: `item_defined_cost:costestimate_field`, `average_cost:average_unit_cost`, `last_purchase_price:latest_po_receipt_price`, `purchase_price:cost_field_or_latest_po_price`; `caveat`: `derived_uses_current_kit_definition`; `caveat`: `po_rate_used_for_special_orders_and_drop_ships` |
| `costingmethod` | select | Costing Method | no | no |  |  | `purpose`: `costing_method`; `options`: `Average`, `Specific`; `option_meanings`: `average:weighted_average`, `specific:exact_cost_per_serial_or_lot` |
| `costingmethoddisplay` | text | Costing Method | no | no |  |  | `purpose`: `costing_method_display`; `options`: `Average`, `Specific`; `option_meanings`: `average:weighted_average`, `specific:exact_cost_per_serial_or_lot` |
| `countryofmanufacture` | select | Manufacturer Country | no | yes |  |  | `purpose`: `country_of_manufacture`; `use_when`: `customs_on_shipping` |
| `createrevenueplanson` | select | Create Revenue Plans On | no | no |  |  | `purpose`: `revenue_plan_creation_event`; `options`: `Revenue Arrangement Creation`, `Billing`, `Fulfillment`, `Project Progress`; `requires_feature`: `Advanced Shipping` (when `fulfillment_option`) [`real_name`]; `requires_feature`: `Projects` (when `project_progress_option`) [`real_name`]; `recommendation`: `match_rule_amount_source`; `option_meanings`: `revenue_arrangement_creation:rule_source_event_amount`, `billing:rule_source_event_percent_amount`, `fulfillment:rule_source_event_percent_quantity`, `project_progress:rule_source_percent_complete`; `applies_to`: `sales_orders`, `cash_sales`, `invoices`, `credit_memos`, `cash_refunds` (when `billing_option`) |
| `currency` | text | Currency | no | no |  |  | `purpose`: `purchase_price_currency`; `value_source`: `preferred_vendor_currency`; `default_behavior`: `base_currency` (when `no_preferred_vendor`) |
| `customform` | select | Custom Form | no | no |  |  | `purpose`: `entry_form`; `allowed_values_listed_in`: `standard_and_custom_entry_forms`; `ui_effect`: `new_option_creates_record` |
| `custreturnvarianceaccount` | select | Customer Return Variance Account | no | no |  |  | `purpose`: `customer_return_variance_account`; `default_behavior`: `variance_posts_to_cogs_account` (when `blank`); `value_examples`: `rma_5_received_at_4_posts_1_variance`; `effect`: `separates_return_cogs_from_sales_cogs` |
| `defaultrevision` | text | Default Revision | no | no |  |  | `purpose`: `default_revision`; `value_source`: `system_displayed` |
| `deferredrevenueaccount` | select | Deferred Revenue Account | no | no |  |  | `purpose`: `deferred_revenue_account`; `applies_when`: `revenue_recognition_or_arm` |
| `deferrevrec` | checkbox | Hold Revenue Recognition | no | no |  |  | `purpose`: `defer_revenue_recognition`; `true_means`: `plans_created_on_hold` |
| `demandmodifier` | percent | Expected Demand Change | no | yes |  |  | `purpose`: `expected_demand_change_percent`; `default_behavior`: `inventory_management_setup_value` (when `not_set`); `value_examples`: `10_percent_added_to_previous_sales` |
| `demandtimefence` | integer | Demand Time Fence | no | no |  |  | `purpose`: `demand_time_fence`; `allowed_range`: `0`, `365` days; `default_behavior`: `default_demand_time_fence_setting` |
| `department` | select | Department | no | yes |  |  | `purpose`: `item_department`; `allowed_values_listed_in`: `department_records`; `ui_effect`: `new_option_creates_record` |
| `description` | textarea | Description | no | no |  |  | `purpose`: `item_description`; `displayed_on`: `sales_forms_description_column`; `ui_effect`: `text_enhance_button_may_appear` |
| `directrevenueposting` | checkbox | Direct Revenue Posting | no | no |  |  | `purpose`: `bypass_advanced_revenue_management`; `true_means`: `posts_directly_to_revenue_no_arrangement`; `ui_effect`: `disables_deferred_revenue_account`; `constraint`: `all_transaction_items_must_match`; `constraint`: `all_kit_members_must_match`; `cannot`: `change_after_used_with_arm`; `default_value`: `false` |
| `displayname` | text | Display Name/Code | no | no |  |  | `purpose`: `display_name`; `max_length`: `250` characters; `displayed_on`: `sales_forms_item_column` (when `basic_printing`); `default_behavior`: `item_name_on_printed_forms` (when `blank`) |
| `distributioncategory` | select | Distribution Category | no | no |  |  | `purpose`: `distribution_category`; `effect`: `network_transfers_in_demand_planning`; `constraint`: `associated_with_item_subsidiary` |
| `distributionnetwork` | select | Distribution Network | no | no |  |  | `purpose`: `distribution_network`; `constraint`: `category_blank_when_network_blank`; `constraint`: `associated_with_item_subsidiary` |
| `dontshowprice` | checkbox | Don't Show Price | no | no |  |  | `purpose`: `hide_price_online`; `use_when`: `advertised_not_sold_or_sold_in_combination` |
| `dropshipexpenseaccount` | select | Dropship Expense Account | no | no |  |  | `purpose`: `drop_ship_expense_account`; `requires_feature`: `Automated Intercompany Drop Ship` [`real_name`]; `default_behavior`: `accounting_preference_default_expense_account`; `constraint`: `account_eliminates_intercompany` |
| `effectivebomcontrol` | select | Effective BOM Control | yes | no |  |  | `purpose`: `effective_bom_control`; `options`: `Effective Date`, `Revision Control`; `cannot`: `leave_revision_control_once_selected` |
| `enforceminqtyinternally` | checkbox | Enforce Internally | no | no |  |  | `purpose`: `enforce_minimum_quantity_internally`; `false_means`: `minimum_applies_to_web_store_only` |
| `excludefromsitemap` | checkbox | Exclude from Sitemap | no | no |  |  | `purpose`: `exclude_from_site_map` |
| `featureddescription` | textarea | Featured Description | no | no |  |  | `purpose`: `featured_description`; `displayed_on`: `web_store_home_page`; `max_length`: `999` characters; `accepts_forms`: `text`, `basic_html`; `ui_effect`: `text_enhance_button_may_appear` |
| `froogleproductfeed` | text |  | no | no |  |  | `purpose`: `include_in_froogle_export`; `false_means`: `excluded_from_feed_export` |
| `futurehorizon` | posinteger | Supply Chain Future Horizon | no | no |  |  | `purpose`: `future_horizon_days`; `default_value`: `30`; `value_unit`: `days`; `max_value`: `365` days; `effect`: `includes_future_orders_beyond_snapshot`; `value_examples`: `30_includes_orders_30_days_past_due` |
| `gainlossaccount` | select | Gain/Loss Account | no | no |  |  | `purpose`: `transfer_gain_loss_account`; `applies_when`: `use_item_cost_as_transfer_cost_disabled`; `constraint`: `differs_from_asset_and_cogs_accounts`; `allowed_values`: `income`, `other_income`, `expense`, `other_expense`; `caveat`: `any_account_with_expand_account_lists`; `default_behavior`: `item_income_account` (when `blank_or_use_income_account`) |
| `handlingcost` | currency | Handling Cost | no | no |  |  | `purpose`: `handling_cost`; `value_unit`: `dollars`; `requires`: `per_item_shipping_item`; `effect`: `charges_calculated_on_sales_transactions`; `allowed_values_listed_in`: `shipping_item_records` |
| `includechildren` | checkbox | Include Children | no | no |  |  | `purpose`: `share_item_with_child_subsidiaries`; `constraint`: `item_settings_compatible_across_subsidiaries`; `value_examples`: `shared_items_need_shared_income_and_asset_accounts` |
| `incomeaccount` | select | Income Account | no | no |  |  | `purpose`: `income_account`; `ui_effect`: `hidden_from_sales_item_list_when_blank` |
| `intercocogsaccount` | select | Intercompany COGS Account | no | no |  |  | `purpose`: `intercompany_cogs_account` |
| `intercoincomeaccount` | select | Intercompany Income Account | no | no |  |  | `purpose`: `intercompany_income_account`; `requires_feature`: `OneWorld` [`real_name`]; `constraint`: `account_eliminates_intercompany`; `applies_to`: `inventory_items`, `non_inventory_resale`, `other_charge_resale`, `service_resale`; `caveat`: `elimination_impossible_if_account_flag_cleared` |
| `internalid` | integer | Internal ID | no | no |  |  | `purpose`: `internal_id`; `use_when`: `smart_tags_in_site_templates`; `doc_inconsistency`: `help_text_truncated` |
| `isdonationitem` | checkbox | Variable Amount | no | no |  |  | `purpose`: `variable_price_item`; `true_means`: `customer_enters_price`; `use_when`: `donations` |
| `isdropshipitem` | checkbox | Drop Ship Item | no | no |  |  | `purpose`: `drop_ship_item`; `effect`: `defaults_drop_ship_on_sales_orders`; `effect`: `approved_sale_creates_linked_purchase_order`; `mutually_exclusive_members`: `isdropshipitem`, `isspecialorderitem` |
| `isinactive` | checkbox | Inactive | no | no |  |  | `purpose`: `inactivate_item`; `ui_effect`: `hidden_from_lists_unless_show_all` |
| `isonline` | checkbox | Display in Web Site | no | no |  |  | `purpose`: `available_online`; `requires`: `checked_to_sell_online`; `caveat`: `other_charge_only_at_checkout_not_catalog` |
| `isphantom` | checkbox | Phantom | no | no |  |  | `purpose`: `default_phantom_item_source`; `effect`: `applied_to_boms_and_work_orders`; `can`: `override_on_bom_or_work_order` |
| `isspecialorderitem` | checkbox | Special Order Item | no | no |  |  | `purpose`: `special_order_item`; `effect`: `defaults_special_order_on_sales`; `effect`: `approved_sale_creates_linked_purchase_order`; `constraint`: `fulfill_after_linked_po_received`; `requires`: `preferred_vendor_and_purchase_price`; `mutually_exclusive_members`: `isdropshipitem`, `isspecialorderitem` |
| `isspecialworkorderitem` | checkbox | Special Work Order Item | no | no |  |  | `purpose`: `default_create_work_order`; `effect`: `create_wo_checked_on_sales_order_lines` |
| `isstorepickupallowed` | checkbox | Store Pickup Allowed | no | no | yes |  | `purpose`: `store_pickup_allowed_somewhere`; `value_source`: `locations_sublist_allow_store_pickup`; `effect`: `cleared_on_save_when_no_location_allows` |
| `issueproduct` | select | Product | no | no |  |  | `purpose`: `issue_management_product`; `allowed_values_listed_in`: `issue_product_records`; `value_kind`: `product_with_versions_and_builds` |
| `itemid` | text | Item Name/Number | yes | no |  |  | `purpose`: `item_name`; `max_length`: `250` characters; `displayed_on`: `transaction_lists`; `displayed_on`: `sales_forms_item_column` (when `no_display_name`); `displayed_on`: `purchase_forms_item_column` (when `no_vendor_name`) |
| `itemprocessfamily` | select | Item Process Family | no | no |  |  | `purpose`: `item_process_family`; `effect`: `identifies_items_stored_together` |
| `itemprocessgroup` | select | Item Process Group | no | no |  |  | `purpose`: `item_process_group`; `comparable_to`: `finer_than_item_process_family` |
| `itemrevenuecategory` | select | Item Revenue Category | no | no |  |  | `purpose`: `item_revenue_category`; `use_when`: `fair_value_price_list` |
| `itemtype` | text |  | no | no |  |  | `purpose`: `item_type_for_label_printing`; `special_values`: `All`; `doc_inconsistency`: `item_record_help_describes_label_printing` |
| `lastpurchaseprice` | currency | Last Purchase Price | no | no |  |  | `purpose`: `last_purchase_price`; `value_source`: `latest_positive_inventory_transaction`; `caveat`: `excludes_returns_and_unbuilds`; `caveat`: `later_entry_wins_same_day`; `value_scope`: `any_location` (when `multi_location_inventory`); `caveat`: `highest_price_wins_same_day_locations`; `value_unit`: `purchase_units` (when `multiple_units_of_measure`); `ui_effect`: `inventory_subtab_links_last_transaction_per_location` |
| `leadtime` | integer | Lead Time | no | yes |  |  | `purpose`: `lead_time_days`; `value_unit`: `days`; `value_source`: `average_of_three_latest_purchase_orders` (when `auto_calculate`); `caveat`: `order_analysis_interval_when_over_three_orders`; `caveat`: `last_receipt_per_order_used`; `caveat`: `not_weighted_by_units_received`; `caveat`: `orders_without_receipts_ignored`; `default_behavior`: `inventory_management_setup_value` (when `blank_and_not_auto`); `doc_inconsistency`: `autoleadtime_first_receipt_vs_leadtime_last_receipt` [`needs_human_review`] |
| `location` | select | Location | no | yes |  |  | `purpose`: `item_location`; `requires_feature`: `Multi-Location Inventory` (when `tracking_inventory_by_location`) [`real_name`]; `effect`: `limits_role_access_to_item`; `value_examples`: `role_limited_to_location_one_items`; `use_instead`: `locations_sublist` (when `tracking_inventory_per_location`); `ui_effect`: `new_option_creates_record` |
| `manufacturer` | text | Manufacturer | no | no |  |  | `purpose`: `manufacturer_name` |
| `manufactureraddr1` | text | Manufacturer Address | no | yes |  |  | `purpose`: `manufacturer_address`; `use_when`: `international_shipping_forms` |
| `manufacturercity` | text | Manufacturer City | no | yes |  |  | `purpose`: `manufacturer_city`; `use_when`: `international_shipping_forms` |
| `manufacturerstate` | text | Manufacturer State | no | yes |  |  | `purpose`: `manufacturer_state`; `use_when`: `international_shipping_forms` |
| `manufacturertariff` | text | Manufacturer Tariff | no | yes |  |  | `purpose`: `hs_or_hts_tariff_code`; `allowed_range`: `6`, `10` characters |
| `manufacturertaxid` | text | Manufacturer Tax Id | no | yes |  |  | `purpose`: `manufacturer_tax_id` |
| `manufacturerzip` | text | Manufacturer Zip | no | yes |  |  | `purpose`: `manufacturer_postal_code`; `use_when`: `international_shipping_forms` |
| `matchbilltoreceipt` | checkbox | Match Bill To Receipt | no | no |  |  | `purpose`: `default_match_bill_to_receipt`; `default_value`: `false`; `true_means`: `variances_from_vendor_bill_lines`; `false_means`: `variances_from_purchase_order_lines`; `effect`: `defaults_line_match_bill_to_receipt`; `can`: `change_on_purchase_order_lines` |
| `matrixitemnametemplate` | textarea | Matrix Item Name Template | no | no |  |  | `purpose`: `matrix_item_name_template`; `value_kind`: `attribute_and_option_order_with_separators`; `effect`: `controls_matrix_item_name_display`; `value_examples`: `fabric_color_waist_length_location` |
| `maxdonationamount` | currency2 | Maximum Variable Amount | no | no |  |  | `purpose`: `maximum_donation_amount`; `applies_when`: `variable_amount_item` |
| `maximumquantity` | posinteger | Maximum Quantity | no | yes |  |  | `purpose`: `maximum_purchase_quantity`; `ui_effect`: `warning_above_maximum`; `effect`: `web_store_checkout_blocked_above_maximum`; `default_behavior`: `no_maximum` (when `blank`); `configured_by`: `site_builder_customize_text` [`warning_text`] |
| `metataghtml` | textarea | Meta Tag HTML | no | no |  |  | `purpose`: `meta_tag_html`; `displayed_on`: `web_store`; `value_kind`: `html_head_meta_tags` |
| `minimumquantity` | posinteger | Minimum Quantity | no | yes |  |  | `purpose`: `minimum_purchase_quantity`; `ui_effect`: `warning_below_minimum`; `effect`: `web_store_checkout_blocked_below_minimum`; `default_behavior`: `no_minimum` (when `blank`); `configured_by`: `site_builder_customize_text` [`warning_text`] |
| `mpn` | text | MPN | no | no |  |  | `purpose`: `manufacturer_part_number` |
| `multmanufactureaddr` | checkbox | Multiple Manufacture Addresses | no | yes |  |  | `purpose`: `manufacturer_has_multiple_addresses` |
| `nextagcategory` | text | NexTag Category | no | no |  |  | `purpose`: `nextag_category`; `use_when`: `product_feed_export` |
| `nextagproductfeed` | text |  | no | no |  |  | `purpose`: `include_in_nextag_export`; `false_means`: `excluded_from_feed_export` |
| `nopricemessage` | text | No Price Message | no | no |  |  | `purpose`: `message_instead_of_price_online`; `applies_when`: `price_hidden_online`; `value_examples`: `call_for_price` |
| `offersupport` | checkbox | Offer Support | no | no |  |  | `purpose`: `offer_support`; `effect`: `selectable_on_case_records` |
| `outofstockbehavior` | select | Out Of Stock Behavior | no | no |  |  | `purpose`: `out_of_stock_behavior`; `options`: `Default`, `disallow_back_orders_show_message`, `allow_back_orders_show_message`, `remove_when_out_of_stock`, `allow_back_orders_no_message`; `option_meanings`: `default:web_site_out_of_stock_setting`; `effect`: `overrides_site_out_of_stock_behavior` |
| `outofstockmessage` | textarea | Out Of Stock Message | no | no |  |  | `purpose`: `custom_out_of_stock_message`; `effect`: `replaces_default_message` |
| `overallquantitypricingtype` | select | Calculate Quantity Discounts | no | no |  |  | `purpose`: `quantity_basis_for_pricing`; `options`: `By Line Quantity`, `By Overall Item Quantity`, `By Overall Parent Quantity`, `By Overall Schedule Quantity`; `option_meanings`: `line:quantity_on_the_line`, `item:all_lines_of_same_item`, `parent:all_items_with_same_parent`, `schedule:all_items_on_same_schedule`; `use_when`: `matrix_items_by_parent_quantity` |
| `pagetitle` | textarea | Page Title | no | no |  |  | `purpose`: `web_page_title`; `displayed_on`: `browser_tab` |
| `parent` | select | Subitem of | no | no |  |  | `purpose`: `parent_item_of_subitem` |
| `preferencecriterion` | select | Preference Criterion | no | yes |  |  | `purpose`: `nafta_preference_criterion` |
| `preferredlocation` | select | Preferred Location | no | no |  |  | `purpose`: `preferred_location`; `effect`: `fills_line_location_on_sales_orders`; `use_when`: `real_time_web_shipping_cost`; `caveat`: `blocks_automatic_location_assignment` |
| `preferredstockleveldays` | float | Days | no | yes |  |  | `purpose`: `preferred_stock_level`; `value_source`: `daily_demand_times_days_supply` (when `auto_calculate`); `effect`: `drives_order_items_replenishment_quantity`; `default_behavior`: `inventory_management_setup_value` (when `not_set`); `value_kind`: `quantity_in_stock_after_order`; `help_text_shared_with`: `assemblyitem.locations.preferredstocklevel`, `inventoryitem.locations.preferredstocklevel`, `lotnumberedassemblyitem.locations.preferredstocklevel`, `lotnumberedinventoryitem.locations.preferredstocklevel`, `serializedassemblyitem.locations.preferredstocklevel`, `serializedinventoryitem.locations.preferredstocklevel` |
| `pricinggroup` | select | Pricing Group | no | no |  |  | `purpose`: `pricing_group`; `use_when`: `customer_specific_group_price_levels`; `allowed_values_listed_in`: `accounting_list_pricing_group` |
| `printitems` | checkbox | Print Items | no | no |  |  | `purpose`: `print_member_items`; `true_means`: `member_lines_on_sales_and_purchase_forms`; `false_means`: `group_description_and_amount_only`; `caveat`: `assemblies_never_on_purchase_forms`; `doc_inconsistency`: `says_members_never_shown_on_printed_forms` |
| `prodpricevarianceacct` | select | Production Price Variance Account | no | no |  |  | `purpose`: `production_price_variance_account`; `purpose`: `variance_from_component_prices`; `value_examples`: `widgets_30_instead_of_20` |
| `prodqtyvarianceacct` | select | Production Quantity Variance Account | no | no |  |  | `purpose`: `production_quantity_variance_account`; `purpose`: `variance_from_component_quantities`; `value_examples`: `10_widgets_instead_of_8`; `doc_inconsistency`: `help_text_first_letter_missing` |
| `producer` | checkbox | Producer | no | no |  |  | `purpose`: `nafta_producer` |
| `productfeed` | select | Product Feed | no | no |  |  | `purpose`: `product_feed_sites`; `value_kind`: `multi_select`; `applies_to`: `kit_package`, `noninventory_for_sale`, `noninventory_for_resale`, `assembly_lot_or_serial`, `inventory_lot_or_serial`; `requires`: `feeds_uploaded_per_site_instructions` |
| `purchasedescription` | textarea | Purchase Description | no | no |  |  | `purpose`: `purchase_description`; `max_length`: `999` characters; `displayed_on`: `vendor_orders`; `recommendation`: `include_unit_of_measure` |
| `purchaseorderamount` | posfloat | Vendor Bill - Purchase Order Amount Tolerance | no | no |  |  | `purpose`: `tolerance_bill_vs_po_amount` |
| `purchaseorderquantity` | posfloat | Vendor Bill - Purchase Order Quantity Tolerance | no | no |  |  | `purpose`: `tolerance_bill_vs_po_quantity` |
| `purchaseorderquantitydiff` | posfloat | Vendor Bill - Purchase Order Quantity Difference | no | no |  |  | `purpose`: `tolerance_bill_vs_po_quantity_difference` |
| `purchasepricevarianceacct` | select | Purchase Price Variance Account | no | no |  |  | `purpose`: `purchase_price_variance_account` |
| `purchaseunit` | select | Purchase Units | no | no |  |  | `purpose`: `purchase_unit` |
| `quantitypricingschedule` | select | Quantity Pricing Schedule | no | no |  |  | `purpose`: `quantity_pricing_schedule`; `effect`: `sets_usemarginalrates_and_calculate_discounts`; `allowed_values_listed_in`: `quantity_pricing_schedule_records` |
| `receiptamount` | posfloat | Vendor Bill - Item Receipt Amount Tolerance | no | no |  |  | `purpose`: `tolerance_bill_vs_receipt_amount` |
| `receiptquantity` | posfloat | Vendor Bill - Item Receipt Quantity Tolerance | no | no |  |  | `purpose`: `tolerance_bill_vs_receipt_quantity` |
| `receiptquantitydiff` | posfloat | Vendor Bill - Item Receipt Quantity Difference | no | no |  |  | `purpose`: `tolerance_bill_vs_receipt_quantity_difference` |
| `relateditemsdescription` | textarea | Related Items Description | no | no |  |  | `purpose`: `related_items_description`; `max_length`: `999` characters; `accepts_forms`: `text`, `basic_html` |
| `reordermultiple` | posinteger | Reorder Multiple | no | no |  |  | `purpose`: `reorder_multiple`; `effect`: `order_items_suggests_multiples`; `value_unit`: `base_units` (when `multiple_units_of_measure`); `value_examples`: `1000_suggests_1000_or_2000_not_1500` |
| `rescheduleindays` | integer | Reschedule In Days | no | no |  |  | `purpose`: `max_days_order_can_be_advanced`; `allowed_range`: `1`, `90` days; `default_behavior`: `blank`; `caveat`: `blank_means_no_reschedule_recommendations`; `value_examples`: `10_allows_10_days_earlier_not_11`; `doc_inconsistency`: `blank_note_says_later_date_copied_from_rescheduleoutdays` [`needs_human_review`] |
| `rescheduleoutdays` | integer | Reschedule Out Days | no | no |  |  | `purpose`: `max_days_order_can_be_delayed`; `allowed_range`: `1`, `180` days; `default_behavior`: `blank`; `caveat`: `blank_means_no_reschedule_recommendations`; `value_examples`: `10_allows_10_days_later_not_11` |
| `revenueallocationgroup` | select | Revenue Allocation Group | no | no |  |  | `purpose`: `revenue_allocation_group`; `use_when`: `groupsum_in_fair_value_formulas`; `effect`: `default_on_revenue_element`; `can`: `change_on_revenue_element` |
| `revenuerecognitionrule` | select | Revenue Recognition Rule | no | no |  |  | `purpose`: `default_revenue_rule`; `recommendation`: `match_create_revenue_plans_on` |
| `revrecforecastrule` | select | Rev Rec Forecast Rule | yes | no |  |  | `purpose`: `forecast_revenue_rule`; `default_behavior`: `same_preference_as_revenue_rule`; `not_supported_for`: `percent_complete_rules` |
| `revrecschedule` | select | Revenue Recognition Template | no | no |  |  | `purpose`: `default_revenue_recognition_template`; `requires`: `deferredrevenueaccount` |
| `safetystocklevel` | float | Safety Stock Level | no | no |  |  | `purpose`: `safety_stock`; `accepts_forms`: `quantity`, `days`; `effect`: `used_in_reorder_point_calculation`; `default_behavior`: `inventory_management_setup_value` (when `not_set`); `help_text_shared_with`: `assemblyitem.safetystockleveldays`, `inventoryitem.safetystockleveldays`, `lotnumberedassemblyitem.safetystockleveldays`, `lotnumberedinventoryitem.safetystockleveldays`, `serializedassemblyitem.safetystockleveldays`, `serializedinventoryitem.safetystockleveldays` |
| `safetystockleveldays` | integer | Days | no | yes |  |  | `purpose`: `safety_stock`; `accepts_forms`: `quantity`, `days`; `effect`: `used_in_reorder_point_calculation`; `default_behavior`: `inventory_management_setup_value` (when `not_set`); `help_text_shared_with`: `assemblyitem.safetystocklevel`, `inventoryitem.safetystocklevel`, `itemlocationconfiguration.safetystocklevel`, `lotnumberedassemblyitem.safetystocklevel`, `lotnumberedinventoryitem.safetystocklevel`, `serializedassemblyitem.safetystocklevel`, `serializedinventoryitem.safetystocklevel` |
| `saleunit` | select | Sale Units | no | no |  |  | `purpose`: `sales_unit` |
| `schedulebcode` | select | Schedule B Code | no | yes |  |  | `purpose`: `schedule_b_unit_of_measure`; `requires_together`: `schedulebcode`, `schedulebquantity`; `use_when`: `shipping_export_declarations`; `value_examples`: `5_kilograms_times_2_items_is_10_kilograms` |
| `schedulebnumber` | text | Schedule B Number | no | yes |  |  | `purpose`: `schedule_b_number` |
| `schedulebquantity` | posinteger | Schedule B Quantity | no | yes |  |  | `purpose`: `schedule_b_quantity_per_item`; `requires_together`: `schedulebcode`, `schedulebquantity`; `use_when`: `shipping_export_declarations`; `value_examples`: `5_kilograms_times_2_items_is_10_kilograms` |
| `scrapacct` | text | Scrap Account | no | yes |  |  | `purpose`: `scrap_expense_account`; `required_when`: `wip_checked_for_any_location` |
| `searchkeywords` | textarea | Search Keywords | no | no |  |  | `purpose`: `web_store_search_keywords`; `caveat`: `weighted_equal_to_item_name` |
| `seasonaldemand` | checkbox | Seasonal Demand | no | yes |  |  | `purpose`: `seasonal_demand`; `true_means`: `demand_from_yearly_changes`; `false_means`: `average_daily_sales_over_period`; `configured_by`: `order_analysis_interval_setting`; `effect`: `used_for_reorder_point_and_stock_level`; `value_source`: `average_sales_quantity_per_day` |
| `shipindividually` | checkbox | Ships Individually | no | yes |  |  | `purpose`: `ships_alone`; `effect`: `affects_package_count_and_rate` |
| `shippackage` | select | Package | no | yes |  |  | `purpose`: `default_package_type`; `ui_effect`: `fills_package_on_fulfillments`; `effect`: `used_for_real_time_shipping_rates`; `allowed_values_listed_in`: `shipping_package_types`; `effect`: `used_for_package_count` |
| `shippingcost` | currency | Shipping Cost | no | no |  |  | `purpose`: `shipping_cost`; `value_unit`: `dollars`; `requires`: `per_item_shipping_item`; `effect`: `charges_calculated_on_sales_transactions`; `allowed_values_listed_in`: `shipping_item_records` |
| `shoppingdotcomcategory` | text | Shopping.com Category | no | no |  |  | `purpose`: `shopping_com_category`; `use_when`: `product_feed_export`; `applies_to`: `kit_package`, `noninventory_for_sale`, `noninventory_for_resale`, `assembly_lot_or_serial`, `inventory_lot_or_serial` |
| `shoppingproductfeed` | text |  | no | no |  |  | `purpose`: `include_in_shopping_com_export`; `false_means`: `excluded_from_feed_export` |
| `shopzillacategoryid` | integer | Shopzilla Category ID | no | no |  |  | `purpose`: `shopzilla_category`; `use_when`: `product_feed_export`; `applies_to`: `kit_package`, `noninventory_for_sale`, `noninventory_for_resale`, `assembly_lot_or_serial`, `inventory_lot_or_serial` |
| `shopzillaproductfeed` | text |  | no | no |  |  | `purpose`: `include_in_shopzilla_export`; `false_means`: `excluded_from_feed_export` |
| `showdefaultdonationamount` | checkbox | Show Default Amount | no | no |  |  | `purpose`: `suggest_online_price_for_donation`; `true_means`: `online_price_shown_then_any_amount_allowed` |
| `sitemappriority` | select | Sitemap Priority | no | no |  |  | `purpose`: `sitemap_priority`; `allowed_range`: `0.0`, `1.0`; `default_value`: `Auto`; `value_source`: `site_hierarchy_position` (when `auto`); `value_examples`: `tab:1.0`, `category_on_tab:0.5`, `item_in_category:0.3`; `applies_to`: `item_records`, `category_records`, `tab_records` |
| `softdescriptor` | select | Soft Descriptor | no | no |  |  | `purpose`: `credit_card_soft_descriptor`; `displayed_on`: `customer_card_statement`; `allowed_values_listed_in`: `credit_card_soft_descriptors` |
| `stockdescription` | text | Stock Description | no | no |  |  | `purpose`: `stock_description`; `max_length`: `21` characters; `value_examples`: `New`, `Refurbished`, `Ships 2-3 days` |
| `stockunit` | select | Stock Units | no | no |  |  | `purpose`: `stock_unit` |
| `storedescription` | textarea | Store Description | no | no |  |  | `purpose`: `store_description`; `displayed_on`: `web_store_item_lists`; `accepts_forms`: `text`, `basic_html`; `configured_by`: `web_site_cart_store_description_setting` [`cart_display`]; `ui_effect`: `text_enhance_button_may_appear` |
| `storedetaileddescription` | textarea | Detailed Description | no | no |  |  | `purpose`: `store_detailed_description`; `displayed_on`: `web_store_item_detail`; `ui_effect`: `text_enhance_button_may_appear` |
| `storedisplayimage` | select | Item Display Image | no | no |  |  | `purpose`: `store_display_image`; `value_source`: `file_cabinet_image`; `caveat`: `auto_resize_only_under_5_megapixels` |
| `storedisplayname` | text | Store Display Name | no | no |  |  | `purpose`: `store_display_name`; `max_length`: `250` characters |
| `storedisplaythumbnail` | select | Item Display Thumbnail | no | no |  |  | `purpose`: `store_thumbnail_image`; `value_source`: `file_cabinet_image`; `displayed_on`: `web_store_before_item_click`; `caveat`: `auto_resize_only_under_5_megapixels` |
| `storeitemtemplate` | select | Item Drilldown Template | no | no |  |  | `purpose`: `store_item_template`; `allowed_values_listed_in`: `item_template_records`; `configured_by`: `site_templates_body_default_item_template` |
| `subsidiary` | select | Subsidiary | no | no |  |  | `purpose`: `item_subsidiaries`; `value_kind`: `multi_select`; `constraint`: `item_usable_only_on_its_subsidiaries_transactions`; `value_examples`: `only_wolfe_us_items_on_wolfe_us_orders` |
| `supplyreplenishmentmethod` | select | Replenishment Method | no | yes |  |  | `purpose`: `replenishment_method`; `options`: `Reorder Point`, `Time Phased`; `default_value`: `Reorder Point` (when `new_item`); `effect`: `time_phased_disables_aim_demand_fields`; `effect`: `time_phased_clears_and_locks_auto_calculate` |
| `supplytimefence` | integer | Planning Time Fence | no | no |  |  | `purpose`: `planning_time_fence`; `allowed_range`: `0`, `365` days; `default_behavior`: `default_planning_time_fence_setting` |
| `taxschedule` | select | Tax Schedule | yes | no |  |  | `purpose`: `tax_schedule`; `allowed_values_listed_in`: `tax_schedule_records` |
| `totalvalue` | currency | Total Value | no | no |  |  | `purpose`: `total_inventory_value`; `value_source`: `purchase_price_times_quantity_on_hand` |
| `tracklandedcost` | checkbox | Track Landed Cost | no | yes |  |  | `purpose`: `track_landed_cost`; `requires`: `landed_cost_item_on_source_transactions`; `value_examples`: `bill_listed_as_landed_cost_source_only_with_landed_item` |
| `transferprice` | currency2 | Transfer Ship Price | no | no |  |  | `purpose`: `default_transfer_price`; `can`: `override_on_transfer_order`; `purpose`: `declared_value_for_shipping` (when `use_item_cost_as_transfer_cost`); `effect`: `used_for_cost_accounting` (when `item_cost_as_transfer_cost_off`); `default_behavior`: `zero_on_transfer_order` (when `blank`) |
| `unbuildvarianceaccount` | select | Unbuild Variance Account | no | no |  |  | `purpose`: `unbuild_variance_account`; `default_behavior`: `variance_posts_to_cogs_account` (when `blank`) |
| `unitstype` | select | Units Type | no | no |  |  | `purpose`: `units_type` |
| `upccode` | text | UPC Code | no | no |  |  | `purpose`: `upc_code`; `max_length`: `999` characters; `ui_effect`: `item_labels_print_upc_barcode`; `default_behavior`: `labels_print_item_name_code128` (when `blank`) |
| `urlcomponent` | textarea | URL Component | no | no |  |  | `purpose`: `url_component`; `caveat`: `case_sensitive`; `default_behavior`: `system_identifiers_in_url` (when `blank`) |
| `usebins` | checkbox | Use Bins | no | yes |  |  | `purpose`: `use_bins`; `requires`: `at_least_one_bin_on_bin_numbers_sublist`; `effect`: `preferred_bin_defaults_on_receipts_and_fulfillments`; `constraint`: `one_preferred_bin_per_location`; `allowed_values_listed_in`: `bin_records`; `can`: `many_items_per_bin_and_many_bins_per_item` |
| `usemarginalrates` | checkbox | Use Marginal Rates | no | no |  |  | `purpose`: `use_marginal_rates`; `true_means`: `discount_per_bracket`; `false_means`: `discount_applies_to_all_units`; `value_examples`: `first_100_full_price_next_50_at_5_percent` |
| `vendorname` | text | Vendor Name/Code | no | no |  |  | `purpose`: `vendor_item_name`; `max_length`: `60` characters; `displayed_on`: `purchase_forms_item_column`; `default_behavior`: `item_name_printed_on_purchase_forms` (when `blank`); `caveat`: `display_name_used_instead_of_item_name_when_set` (when `blank`) |
| `vendreturnvarianceaccount` | select | Vendor Return Variance Account | no | no |  |  | `purpose`: `vendor_return_variance_account`; `default_behavior`: `variance_posts_to_cogs_account` (when `blank`) |
| `vsoedeferral` | select | Deferral | no | no |  |  | `purpose`: `bundle_deferral`; `options`: `Defer Bundle Until Delivered`, `Defer Until Item Delivered`; `default_value`: `Defer Until Item Delivered`; `option_meanings`: `defer_bundle_until_delivered:whole_bundle_waits_for_item`, `defer_until_item_delivered:item_waits_for_itself`; `caveat`: `interacts_with_other_bundle_items_settings` |
| `vsoedelivered` | checkbox | Default as Delivered | no | no |  |  | `purpose`: `auto_mark_delivered`; `effect`: `delivered_status_when_added_to_transaction` |
| `vsoepermitdiscount` | select | Permit Discount | no | no |  |  | `purpose`: `vsoe_discount_handling`; `options`: `As Allowed`, `Never`; `caveat`: `vsoe_items_never_discount_with_residual_arm` |
| `vsoeprice` | currency2 | VSOE Price | no | no |  |  | `purpose`: `vsoe_price`; `can`: `change_per_order` |
| `vsoesopgroup` | select | Allocation Type | no | no |  |  | `purpose`: `revenue_allocation_type`; `options`: `Normal`, `Exclude`, `Software`; `option_meanings`: `exclude:discounted_amount_is_revenue`; `requires_one_of`: `vsoe_with_eitf_suiteapp`, `advanced_revenue_management` (when `software_option`); `option_meanings`: `normal:eitf_08_01_allocation`, `software:eitf_and_sop_97_2_or_fair_value_list`; `effect`: `software_reallocates_by_residual_when_estimates` |
| `weight` | float | Item Weight | no | no |  |  | `purpose`: `item_weight`; `value_kind`: `weight_with_selected_unit` |
| `wipacct` | text | WIP Account | no | yes |  |  | `purpose`: `wip_asset_account`; `required_when`: `wip_checked_for_any_location` |
| `wipvarianceacct` | text | WIP Cost Variance Account | no | yes |  |  | `purpose`: `wip_variance_expense_account`; `required_when`: `wip_checked_for_any_location` |
| `auctionquantity` | posinteger | Auction Quantity | no | no |  |  |  |
| `auctiontype` | select | Auction Type | no | no |  |  |  |
| `buildtimelotsize` | float | Work Order Lead Time Lot Size | no | no |  |  |  |
| `buyitnowprice` | currency | Buy It Now Price | no | yes |  |  |  |
| `conditionenabled` | text |  | no | no |  |  |  |
| `conditionhelpurl` | text |  | no | no |  |  |  |
| `costforpricing` | currency2 | Cost for Pricing | no | no |  | marked_new |  |
| `costunits` | text |  | no | no |  |  |  |
| `createddate` | datetime | Date Created | no | no |  |  |  |
| `displayinebaystore` | checkbox | eBay Store | no | yes |  |  |  |
| `ebayhandlingtime` | select | Handling Time | no | no |  |  |  |
| `ebayintlinsurancefee` | currency | Insurance Amount | no | no |  |  |  |
| `ebayintlpackagehandlingfee` | currency | Package & Handling Fee | no | no |  |  |  |
| `ebayintlshipinsurance` | select | Shipping Insurance | no | no |  |  |  |
| `ebayintlshippingitem1` | select | Shipping Item 1 | no | no |  |  |  |
| `ebayintlshippingitem2` | select | Shipping Item 2 | no | no |  |  |  |
| `ebayintlshippingitem3` | select | Shipping Item 3 | no | no |  |  |  |
| `ebayisintlcalculatedrate` | radio | Flat Rates | no | no |  |  |  |
| `ebayisirregularpackage` | checkbox | Irregular Package | no | no |  |  |  |
| `ebayitemdescription` | textarea | Item Description | no | no |  |  |  |
| `ebayitemlocdisplay` | text | Item Location | no | no |  |  |  |
| `ebayitemloczipcode` | text | Item Location Zip Code | no | no |  |  |  |
| `ebayitemlots` | integer | Lot Size | no | no |  |  |  |
| `ebayitemsubtitle` | text | Subtitle | no | yes |  |  |  |
| `ebayitemtitle` | text | Item Title | no | yes |  |  |  |
| `ebayitemweightamt` | posfloat | Item Weight | no | no |  |  |  |
| `ebaylayout` | select | Default Chart Type | no | no |  |  |  |
| `ebaypackagetype` | select | Package | no | no |  |  |  |
| `ebaypagecounter` | select | Page Counter | no | no |  |  |  |
| `ebayrelistingoption` | select | Relisting Option | no | no |  |  |  |
| `ebaytheme` | select | Theme | no | no |  |  |  |
| `ebaythemegroup` | select | Theme Group | no | no |  |  |  |
| `endauctionswhenoutofstock` | checkbox | End Auctions When Item is Out of Stock | no | yes |  |  |  |
| `externalid` | text | ExternalId | no | no |  |  |  |
| `fixedbuildtime` | float | Work Order Fixed Lead Time | no | no |  |  |  |
| `gallery` | checkbox | Gallery | no | no |  |  |  |
| `galleryfeatured` | checkbox | Featured First | no | no |  |  |  |
| `gifttypeexpressship` | checkbox | Express shipping | no | no |  |  |  |
| `gifttypegiftwrap` | checkbox | Gift wrap / gift card | no | no |  |  |  |
| `gifttypeshiptorecipient` | checkbox | Ship to gift recipient | no | no |  |  |  |
| `handlingcostunits` | text |  | no | no |  |  |  |
| `handlinggroup` | text | Handling | no | no |  |  |  |
| `imagesgroup` | text | Images | no | no |  |  |  |
| `imageslocation` | select | Images Location | no | no |  |  |  |
| `iscalculatedrate` | radio | Flat Rates | no | no |  |  |  |
| `isgcocompliant` | checkbox | Google Compliant | no | yes |  |  |  |
| `itemcondition` | select | Item Condition | no | no |  |  |  |
| `itemhandlingfee` | currency | Package & Handling Fee | no | no |  |  |  |
| `iteminsurancefee` | currency | Insurance Amount | no | no |  |  |  |
| `itemnumberoptions` | select | Item Number Options | no | no |  |  |  |
| `itemoptions` | select | Item Options | no | no |  |  |  |
| `itemshipinsurance` | select | Shipping Insurance | no | no |  |  |  |
| `lastmodifieddate` | datetime | Last Modified | no | no |  |  |  |
| `listimmediate` | radio | Start listing when submitted | no | no |  |  |  |
| `listingduration` | select | Duration | no | no |  |  |  |
| `listingstartdate` | date | Start Date | no | no |  |  |  |
| `listingstarttime` | select | Start Time | no | no |  |  |  |
| `minimumquantityunits` | text |  | no | no |  |  |  |
| `numactivelistings` | integer | Active Listings | no | yes |  |  |  |
| `numcurrentlylisted` | integer | Quantity Currently Listed | no | yes |  |  |  |
| `packageheight` | posinteger | Height | no | no |  |  |  |
| `packagelength` | posinteger | Length | no | no |  |  |  |
| `packagewidth` | posinteger | Width | no | no |  |  |  |
| `planningitemcategory` | select | Planning Item Category | no | no |  |  |  |
| `primarycatdisplayname` | text |  | no | no |  |  |  |
| `primarycategory` | integer | Primary Category | no | yes |  |  |  |
| `quantityreorderunits` | text |  | no | no |  |  |  |
| `refundgivenas` | select | Refund will be given as | no | no |  |  |  |
| `reserveprice` | currency | Reserve Price | no | yes |  |  |  |
| `returnpolicy` | select | Return Policy | no | no |  |  |  |
| `returnpolicydetails` | textarea | Details | no | no |  |  |  |
| `returnshippingpaidby` | select | Return Shipping will be paid by | no | no |  |  |  |
| `returnswithin` | select | Item must be returned within | no | no |  |  |  |
| `safetystocklevelunits` | text |  | no | no |  |  |  |
| `secondarycatdisplayname` | text |  | no | no |  |  |  |
| `secondarycategory` | integer | Secondary Category | no | no |  |  |  |
| `sellonebay` | checkbox | Sell on eBay | no | yes |  |  |  |
| `shipasia` | checkbox | Asia | no | no |  |  |  |
| `shipaustralia` | checkbox | Australia | no | no |  |  |  |
| `shipcanada` | checkbox | Canada | no | no |  |  |  |
| `shipeurope` | checkbox | Europe | no | no |  |  |  |
| `shipgermany` | checkbox | Germany | no | no |  |  |  |
| `shipjapan` | checkbox | Japan | no | no |  |  |  |
| `shipmexico` | checkbox | Mexico | no | no |  |  |  |
| `shipnorthsouthamerica` | checkbox | North and South America | no | no |  |  |  |
| `shippingcostunits` | text |  | no | no |  |  |  |
| `shippingdomesticmethodsgroup` | text | Shipping Methods | no | no |  |  |  |
| `shippingdomgroup` | text | Domestic Shipping Settings | no | no |  |  |  |
| `shippingintlgroup` | text | International Shipping Settings | no | no |  |  |  |
| `shippingintlgroup1` | text | Shipping Method 1 | no | no |  |  |  |
| `shippingintlgroup2` | text | Shipping Method 2 | no | no |  |  |  |
| `shippingintlgroup3` | text | Shipping Method 3 | no | no |  |  |  |
| `shippingitem1` | select | Shipping Item 1 | no | no |  |  |  |
| `shippingitem2` | select | Shipping Item 2 | no | no |  |  |  |
| `shippingitem3` | select | Shipping Item 3 | no | no |  |  |  |
| `shippinglocationsgroup` | text | Ship-to Locations | no | no |  |  |  |
| `shippingpackaginggroup` | text | Packaging | no | no |  |  |  |
| `shippingrate1` | currency | Rate | no | no |  |  |  |
| `shippingrate2` | currency | Rate | no | no |  |  |  |
| `shippingrate3` | currency | Rate | no | no |  |  |  |
| `shipuk` | checkbox | United Kingdom | no | no |  |  |  |
| `shipworldwide` | checkbox | Worldwide | no | no |  |  |  |
| `showasgift` | checkbox | Show as Gift | no | no |  |  |  |
| `standardimages` | radio | Standard | no | no |  |  |  |
| `startingprice` | currency | Starting Price | no | yes |  |  |  |
| `storecatdisplayname` | text |  | no | no |  |  |  |
| `storecatdisplayname2` | text |  | no | no |  |  |  |
| `storecategory` | integer | Store Category | no | no |  |  |  |
| `storecategory2` | integer | Secondary Store Category | no | no |  |  |  |
| `supersizeimages` | checkbox | Supersize Pictures | no | no |  |  |  |
| `taxable` | checkbox | Taxable | no | no |  |  |  |
| `templatesgroup` | text | Templates | no | no |  |  |  |
| `transferpriceunits` | text |  | no | no |  |  |  |
| `weightunit` | select |  | no | no |  |  |  |
| `weightunits` | text |  | no | no |  |  |  |
| `willship` | radio | Will Ship to the US and the following: | no | no |  |  |  |
| `yahooproductfeed` | text |  | no | no |  |  |  |

## Sublist `itemvendor` (Vendors)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `vendor` | select | Vendor | yes |  | `purpose`: `preferred_vendor`; `allowed_values_listed_in`: `vendor_records`; `ui_effect`: `new_option_creates_record` |
| `itemvendorprice` | summary | Purchase Prices | no |  |  |
| `preferredvendor` | checkbox | Preferred | no |  |  |
| `purchaseprice` | rate | Purchase Price | no |  |  |
| `schedule` | select | Schedule | no |  |  |
| `subsidiary` | text | Subsidiary | no |  |  |
| `vendorcode` | text | Code | no |  |  |
| `vendorcurrencyname` | text | Currency | no |  |  |

## Sublist `locations` (Locations)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `location` | integer |  | no |  | `purpose`: `item_location`; `caveat`: `role_restriction_only_without_multi_location`; `allowed_values_listed_in`: `location_records`; `ui_effect`: `new_option_creates_record`; `help_text_shared_with`: `assemblyitem.location`, `inventoryitem.location`, `lotnumberedinventoryitem.location`, `serializedinventoryitem.location` |
| `preferredstocklevel` | float | Preferred Stock Level | no |  | `purpose`: `preferred_stock_level`; `value_source`: `daily_demand_times_days_supply` (when `auto_calculate`); `effect`: `drives_order_items_replenishment_quantity`; `default_behavior`: `inventory_management_setup_value` (when `not_set`); `value_kind`: `quantity_in_stock_after_order`; `help_text_shared_with`: `assemblyitem.preferredstockleveldays`, `inventoryitem.preferredstockleveldays`, `lotnumberedassemblyitem.preferredstockleveldays`, `lotnumberedinventoryitem.preferredstockleveldays`, `serializedassemblyitem.preferredstockleveldays`, `serializedinventoryitem.preferredstockleveldays` |
| `reorderpoint` | float | Build Point | no |  | `purpose`: `reorder_point`; `value_source`: `lead_time_days_times_demand` (when `no_safety_stock`); `value_source`: `lead_plus_safety_days_times_demand` (when `safety_stock_days`); `value_source`: `lead_days_times_demand_plus_safety_qty` (when `safety_stock_quantity`); `doc_inconsistency`: `help_text_truncated`; `false_means`: `reorder_point_entered_manually` |
| `backwardconsumptiondays` | integer | Backward Consumption Days | no |  |  |
| `buildtime` | float | Work Order Variable Lead Time | no |  |  |
| `buildtimelotsize` | float | Work Order Lead Time Lot Size | no |  |  |
| `defaultreturncost` | currency2 | Default Return Cost | no |  |  |
| `demandsource` | select | Demand Source | no |  |  |
| `demandtimefence` | integer | Demand Time Fence | no |  |  |
| `fixedbuildtime` | float | Work Order Fixed Lead Time | no |  |  |
| `fixedlotsize` | posfloat | Fixed Lot Size | no |  |  |
| `forwardconsumptiondays` | integer | Forward Consumption Days | no |  |  |
| `iswip` | checkbox | WIP | no |  |  |
| `locationallowstorepickup` | checkbox | Allow Store Pickup | no |  |  |
| `periodiclotsizedays` | integer | Periods of Supply Increment | no |  |  |
| `periodiclotsizetype` | select | Periods of Supply Type | no |  |  |
| `rescheduleindays` | integer | Reschedule In Days | no |  |  |
| `rescheduleoutdays` | integer | Reschedule Out Days | no |  |  |
| `supplylotsizingmethod` | select | Lot Sizing Method | no |  |  |
| `supplytimefence` | integer | Planning Time Fence | no |  |  |
| `supplytype` | select | Supply Type | no |  |  |

## Sublist `accountingbookdetail` (Accounting Books)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `accountingbook` | select | Accounting Book | no |  |  |
| `amortizationtemplate` | select | Amortization Template | no |  |  |
| `createrevenueplanson` | select | Create Revenue Plans On | no |  |  |
| `revenuerecognitionrule` | select | Revenue Recognition Rule | no |  |  |
| `revrecforecastrule` | select | Rev Rec Forecast Rule | no |  |  |
| `revrecschedule` | select | Revenue Recognition Template | no |  |  |
| `sameasprimaryamortization` | checkbox | Same as Primary Book Amortization | no |  |  |
| `sameasprimaryrevrec` | checkbox | Same as Primary Book Rev Rec | no |  |  |

## Sublist `billofmaterials` (Bill of Materials on Assembly Item)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `billofmaterials` | select | Bill of Materials | yes |  |  |
| `currentrevision` | text | Current Revision | no |  |  |
| `defaultforlocation` | multiselect | Default for Location | no |  |  |
| `inactive` | text | Inactive | no |  |  |
| `masterdefault` | checkbox | Master Default | no |  |  |
| `memo` | textarea | Memo | no |  |  |

## Sublist `binnumber` (Bin Numbers)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `binnumber` | select | Bin Number | yes |  |  |
| `location` | text |  | no |  |  |
| `locationactive` | text | Location Active | no |  |  |
| `onhand` | float | On Hand | no |  |  |
| `onhandavail` | float | Available | no |  |  |
| `preferredbin` | checkbox | Preferred (per Location) | no |  |  |

## Sublist `hierarchyversions` (Hierarchy Versions)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `hierarchynode` | select | Hierarchy Node | no |  |  |
| `isincluded` | checkbox | Included In Version | no |  |  |

## Sublist `member` (Members)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `effectivedate` | date | Effective Date | no |  |  |
| `effectiverevision` | select | Effective Revision | no |  |  |
| `item` | select | Item | yes |  |  |
| `itemsource` | select | Item Source | no |  |  |
| `memberdescr` | textarea | Description | no |  |  |
| `memberunit` | text | Units | no |  |  |
| `obsoletedate` | date | Obsolete Date | no |  |  |
| `obsoleterevision` | select | Obsolete Revision | no |  |  |
| `quantity` | posfloat | Quantity | yes |  |  |
| `taxschedule` | select | Schedule | no |  |  |
| `weight` | text |  | no |  |  |

## Sublist `price1` (US Dollar)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `currency` | text | Currency | no |  |  |
| `discount` | percent | Default Discount % | no |  |  |
| `discountdisplay` | percent | Default Discount % | no |  |  |
| `pricelevel` | text | Price Level | no |  |  |

## Sublist `price2` (British pound)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `currency` | text | Currency | no |  |  |
| `discount` | percent | Default Discount % | no |  |  |
| `discountdisplay` | percent | Default Discount % | no |  |  |
| `pricelevel` | text | Price Level | no |  |  |

## Sublist `price3` (Canadian Dollar)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `currency` | text | Currency | no |  |  |
| `discount` | percent | Default Discount % | no |  |  |
| `discountdisplay` | percent | Default Discount % | no |  |  |
| `pricelevel` | text | Price Level | no |  |  |

## Sublist `price4` (Euro)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `currency` | text | Currency | no |  |  |
| `discount` | percent | Default Discount % | no |  |  |
| `discountdisplay` | percent | Default Discount % | no |  |  |
| `pricelevel` | text | Price Level | no |  |  |

## Sublist `sitecategory` (Site Category)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `category` | select | Site Category | yes |  |  |
| `categorydescription` | text | Description | no |  |  |
| `isdefault` | checkbox | Preferred Category | no |  |  |
| `website` | select | Site | yes |  |  |

## Sublist `translations` (Item Translation)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `displayname` | text | Display Name | no |  |  |
| `featureddescription` | textarea | Featured Description | no |  |  |
| `language` | text | Language | no |  |  |
| `locale` | text |  | no |  |  |
| `nopricemessage` | textarea | No Price Message | no |  |  |
| `outofstockmessage` | textarea | Out Of Stock Message | no |  |  |
| `pagetitle` | textarea | Page Title | no |  |  |
| `salesdescription` | textarea | Sales Description | no |  |  |
| `storedescription` | textarea | Store Description | no |  |  |
| `storedetaileddescription` | textarea | Detailed Description | no |  |  |
| `storedisplayname` | text | Store Display Name | no |  |  |

## Tabs

| Tab ID | Label |
|---|---|
| `basic` | Basic |
| `custom` | Custom |
| `history` | History |
| `inventory` | Inventory |
| `inventorynumbers` | Inventory Numbers |
| `members` | Members |
| `pricing` | Pricing |
| `relitems` | Related Items |
| `specials` | Specials |
| `store` | Store |
| `translation` | Translation |
| `vsoepricing` | Rev Rec / Amort |
| `workflow` | Workflow |

## Suspected source errors

> [!SOURCE-ERROR] `autoleadtime_first_receipt_vs_leadtime_last_receipt` (on `autoleadtime`)
<!-- source-error: record:serializedassemblyitem | autoleadtime | autoleadtime_first_receipt_vs_leadtime_last_receipt -->

> [!SOURCE-ERROR] `help_describes_costing_method` (on `costcategory`)
<!-- source-error: record:serializedassemblyitem | costcategory | help_describes_costing_method -->

> [!SOURCE-ERROR] `help_text_truncated` (on `internalid`)
<!-- source-error: record:serializedassemblyitem | internalid | help_text_truncated -->

> [!SOURCE-ERROR] `item_record_help_describes_label_printing` (on `itemtype`)
<!-- source-error: record:serializedassemblyitem | itemtype | item_record_help_describes_label_printing -->

> [!SOURCE-ERROR] `autoleadtime_first_receipt_vs_leadtime_last_receipt` (on `leadtime`)
<!-- source-error: record:serializedassemblyitem | leadtime | autoleadtime_first_receipt_vs_leadtime_last_receipt -->

> [!SOURCE-ERROR] `says_members_never_shown_on_printed_forms` (on `printitems`)
<!-- source-error: record:serializedassemblyitem | printitems | says_members_never_shown_on_printed_forms -->

> [!SOURCE-ERROR] `help_text_first_letter_missing` (on `prodqtyvarianceacct`)
<!-- source-error: record:serializedassemblyitem | prodqtyvarianceacct | help_text_first_letter_missing -->

> [!SOURCE-ERROR] `blank_note_says_later_date_copied_from_rescheduleoutdays` (on `rescheduleindays`)
<!-- source-error: record:serializedassemblyitem | rescheduleindays | blank_note_says_later_date_copied_from_rescheduleoutdays -->

> [!SOURCE-ERROR] `help_text_truncated` (on `locations.reorderpoint`)
<!-- source-error: record:serializedassemblyitem | locations.reorderpoint | help_text_truncated -->
