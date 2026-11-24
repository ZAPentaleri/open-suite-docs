# Other Charge Item (`otherchargeitem`)
<!-- id: record:otherchargeitem -->

| Fact | Value |
|---|---|
| Internal ID | `otherchargeitem` |
| Category | item |
| Client scripts | no |
| Server scripts | no |
| Searchable | yes |
| Search data via | Item |
| Tags | `custom_fields_supported`, `scripting_full` |

## Record facts

- `initialization_default_value`: `subtype`, `sale`, `resale`, `purchase` (refs `N/record.create`, `N/record.copy`, `N/record.transform`, `N/record.load`)
- `script_functions`: `create`, `update`, `copy`, `delete`, `search`
- `script_types_unstated`: `true`
- `doc_inconsistency`: `full_level_but_client_and_server_unchecked` [`needs_human_review`]

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `amortizationperiod` | integer | Amortization Period | no | no |  | `purpose`: `number_of_amortization_periods`; `value_examples`: `60_amortizes_over_60_periods`; `value_source`: `bill_amortization_start_date` [`start`]; `default_behavior`: `bill_posting_date_starts` (when `no_start_date`) |
| `amortizationtemplate` | select | Amortization Template | no | no |  | `purpose`: `amortization_template`; `effect`: `default_on_purchase_transactions`; `can`: `override_on_transaction` |
| `availabletopartners` | checkbox | Available to Adv. Partners | no | no |  | `purpose`: `visible_to_advanced_partner_center`; `applies_to`: `advanced_partner_center_roles`, `custom_roles_with_item_access` |
| `billexchratevarianceacct` | select | Exchange Rate Variance Account | no |  |  | `purpose`: `exchange_rate_variance_account`; `effect`: `account_changes_logged_in_system_notes` |
| `billingschedule` | select | Billing Schedule | no | no |  | `purpose`: `billing_schedule`; `effect`: `default_schedule_on_order_lines`; `requires`: `form_customized_to_show_line_schedules`; `allowed_values_listed_in`: `billing_schedule_records`; `ui_effect`: `new_option_creates_record` |
| `billpricevarianceacct` | select | Price Variance Account | no |  |  | `purpose`: `bill_price_variance_account`; `effect`: `account_changes_logged_in_system_notes` |
| `billqtyvarianceacct` | select | Quantity Variance Account | no |  |  | `purpose`: `bill_quantity_variance_account`; `effect`: `account_changes_logged_in_system_notes` |
| `class` | select | Class | no | yes |  | `purpose`: `item_class`; `allowed_values_listed_in`: `class_records`; `ui_effect`: `new_option_creates_record` |
| `consumptionunit` | select | Consumption Units | no | no |  | `purpose`: `default_inventory_unit`; `requires_feature`: `Multiple Units of Measure` [`real_name`] |
| `contingentrevenuehandling` | checkbox | Eligible For Contingent Revenue Handling | no | no |  | `purpose`: `contingent_revenue_handling`; `effect`: `affects_revenue_allocation` |
| `cost` | currency2 | Purchase Price | no | no |  | `purpose`: `purchase_price`; `default_behavior`: `latest_purchase_price` (when `blank`); `value_unit`: `preferred_vendor_currency`; `value_unit`: `base_currency` (when `no_preferred_vendor`) |
| `costcategory` | select | Cost Category | yes | no |  | `purpose`: `cost_category`; `allowed_values_listed_in`: `accounting_list_cost_category` |
| `costestimate` | currency | Item Defined Cost | no | no |  | `purpose`: `item_defined_cost` |
| `costestimatetype` | select | Cost Estimate Type | no | no |  | `purpose`: `cost_basis_for_estimated_gross_profit`; `options`: `Item Defined Cost`, `Average Cost`, `Last Purchase Price`, `Purchase Price`, `Preferred Vendor Rate`, `Derived from member items`; `requires_feature`: `Multi-Vendor` (when `preferred_vendor_rate`) [`real_name`]; `stage_order`: `preferred_vendor_rate`, `purchase_price`, `purchase_order_rate` (when `preferred_vendor_rate`); `applies_to`: `kits` (when `derived_from_member_items`); `effect`: `drives_transaction_estimated_gross_profit`; `option_meanings`: `item_defined_cost:costestimate_field`, `average_cost:average_unit_cost`, `last_purchase_price:latest_po_receipt_price`, `purchase_price:cost_field_or_latest_po_price`; `caveat`: `derived_uses_current_kit_definition`; `caveat`: `po_rate_used_for_special_orders_and_drop_ships` |
| `createrevenueplanson` | select | Create Revenue Plans On | no | no |  | `purpose`: `revenue_plan_creation_event`; `options`: `Revenue Arrangement Creation`, `Billing`, `Fulfillment`, `Project Progress`; `requires_feature`: `Advanced Shipping` (when `fulfillment_option`) [`real_name`]; `requires_feature`: `Projects` (when `project_progress_option`) [`real_name`]; `recommendation`: `match_rule_amount_source`; `option_meanings`: `revenue_arrangement_creation:rule_source_event_amount`, `billing:rule_source_event_percent_amount`, `fulfillment:rule_source_event_percent_quantity`, `project_progress:rule_source_percent_complete`; `applies_to`: `sales_orders`, `cash_sales`, `invoices`, `credit_memos`, `cash_refunds` (when `billing_option`) |
| `currency` | text | Currency | no | no |  | `purpose`: `purchase_price_currency`; `value_source`: `preferred_vendor_currency`; `default_behavior`: `base_currency` (when `no_preferred_vendor`) |
| `customform` | select | Custom Form | no | no |  | `purpose`: `entry_form`; `allowed_values_listed_in`: `standard_and_custom_entry_forms`; `ui_effect`: `new_option_creates_record` |
| `deferralaccount` | select | Deferred Expense Account | no | no |  | `purpose`: `deferred_expense_account`; `constraint`: `template_needs_own_deferred_account_when_blank` |
| `deferredrevenueaccount` | select | Deferred Revenue Account | no | no |  | `purpose`: `deferred_revenue_account`; `applies_when`: `revenue_recognition_or_arm` |
| `deferrevrec` | checkbox | Hold Revenue Recognition | no | no |  | `purpose`: `defer_revenue_recognition`; `true_means`: `plans_created_on_hold` |
| `department` | select | Department | no | yes |  | `purpose`: `item_department`; `allowed_values_listed_in`: `department_records`; `ui_effect`: `new_option_creates_record` |
| `directrevenueposting` | checkbox | Direct Revenue Posting | no | no |  | `purpose`: `bypass_advanced_revenue_management`; `true_means`: `posts_directly_to_revenue_no_arrangement`; `ui_effect`: `disables_deferred_revenue_account`; `constraint`: `all_transaction_items_must_match`; `constraint`: `all_kit_members_must_match`; `cannot`: `change_after_used_with_arm`; `default_value`: `false` |
| `displayname` | text | Display Name/Code | no | no |  | `purpose`: `display_name`; `max_length`: `250` characters; `displayed_on`: `sales_forms_item_column` (when `basic_printing`); `default_behavior`: `item_name_on_printed_forms` (when `blank`) |
| `enforceminqtyinternally` | checkbox | Enforce Internally | no | no |  | `purpose`: `enforce_minimum_quantity_internally`; `false_means`: `minimum_applies_to_web_store_only` |
| `expenseaccount` | select | Expense Account | no | no |  | `purpose`: `expense_account` |
| `generateaccruals` | checkbox | Generate Accruals | no | no |  | `purpose`: `generate_accruals`; `effect`: `posts_accrual_to_gl` |
| `includechildren` | checkbox | Include Children | no | no |  | `purpose`: `share_item_with_child_subsidiaries`; `constraint`: `item_settings_compatible_across_subsidiaries`; `value_examples`: `shared_items_need_shared_income_and_asset_accounts` |
| `incomeaccount` | select | Income Account | no | no |  | `purpose`: `income_account`; `ui_effect`: `hidden_from_sales_item_list_when_blank` |
| `intercodefrevaccount` | select | Intercompany Deferred Revenue Account | no | no |  | `purpose`: `intercompany_deferred_revenue_account`; `requires_feature`: `OneWorld` [`real_name`]; `constraint`: `account_eliminates_intercompany` |
| `intercoexpenseaccount` | select | Intercompany Expense Account | no | no |  | `purpose`: `intercompany_expense_account`; `requires_feature`: `OneWorld` [`real_name`]; `constraint`: `account_eliminates_intercompany`; `caveat`: `elimination_impossible_if_account_flag_cleared`; `applies_to`: `inventory_items`, `non_inventory_resale`, `other_charge_resale`, `service_resale` |
| `intercoincomeaccount` | select | Intercompany Income Account | no | no |  | `purpose`: `intercompany_income_account`; `requires_feature`: `OneWorld` [`real_name`]; `constraint`: `account_eliminates_intercompany`; `applies_to`: `inventory_items`, `non_inventory_resale`, `other_charge_resale`, `service_resale`; `caveat`: `elimination_impossible_if_account_flag_cleared` |
| `isfulfillable` | checkbox | Can be Fulfilled/Received | no | no |  | `purpose`: `can_be_fulfilled`; `cannot`: `change_after_used_on_transactions`; `requires_feature`: `Advanced Billing`, `Advanced Shipping` [`real_name`] |
| `isinactive` | checkbox | Inactive | no | no |  | `purpose`: `inactivate_item`; `ui_effect`: `hidden_from_lists_unless_show_all` |
| `isonline` | checkbox | Display in Web Store | no | no |  | `purpose`: `available_online`; `requires`: `checked_to_sell_online`; `caveat`: `other_charge_only_at_checkout_not_catalog` |
| `issueproduct` | select | Product Name | no | no |  | `purpose`: `issue_management_product`; `allowed_values_listed_in`: `issue_product_records`; `value_kind`: `product_with_versions_and_builds` |
| `itemid` | text | Item Name/Number | yes | no |  | `purpose`: `item_name`; `max_length`: `250` characters; `displayed_on`: `transaction_lists`; `displayed_on`: `sales_forms_item_column` (when `no_display_name`); `displayed_on`: `purchase_forms_item_column` (when `no_vendor_name`) |
| `itemrevenuecategory` | select | Item Revenue Category | no | no |  | `purpose`: `item_revenue_category`; `use_when`: `fair_value_price_list` |
| `itemtype` | text |  | no | no |  | `purpose`: `item_type_for_label_printing`; `special_values`: `All`; `doc_inconsistency`: `item_record_help_describes_label_printing` |
| `location` | select | Location | no | yes |  | `purpose`: `item_location`; `requires_feature`: `Multi-Location Inventory` (when `tracking_inventory_by_location`) [`real_name`]; `effect`: `limits_role_access_to_item`; `ui_effect`: `new_option_creates_record`; `use_instead`: `locations_sublist` (when `tracking_inventory_per_location`) |
| `matrixitemnametemplate` | textarea | Matrix Item Name Template | no | no |  | `purpose`: `matrix_item_name_template`; `value_kind`: `attribute_and_option_order_with_separators`; `effect`: `controls_matrix_item_name_display`; `value_examples`: `fabric_color_waist_length_location` |
| `maximumquantity` | posinteger | Maximum Quantity | no | yes |  | `purpose`: `maximum_purchase_quantity`; `ui_effect`: `warning_above_maximum`; `effect`: `web_store_checkout_blocked_above_maximum`; `default_behavior`: `no_maximum` (when `blank`); `configured_by`: `site_builder_customize_text` [`warning_text`] |
| `minimumquantity` | posinteger | Minimum Quantity | no | yes |  | `purpose`: `minimum_purchase_quantity`; `ui_effect`: `warning_below_minimum`; `effect`: `web_store_checkout_blocked_below_minimum`; `default_behavior`: `no_minimum` (when `blank`); `configured_by`: `site_builder_customize_text` [`warning_text`] |
| `mossapplies` | checkbox | MOSS Applies | no | no |  | `purpose`: `moss_vat_applies`; `constraint`: `not_in_item_groups`; `disagreement`: `other_items_call_scheme_oss` [`page_value_kept`] |
| `offersupport` | checkbox | Offer Support | no | no |  | `purpose`: `offer_support`; `effect`: `selectable_on_case_records` |
| `overallquantitypricingtype` | select | Calculate Quantity Discounts | no | no |  | `purpose`: `quantity_basis_for_pricing`; `options`: `By Line Quantity`, `By Overall Item Quantity`, `By Overall Parent Quantity`, `By Overall Schedule Quantity`; `option_meanings`: `line:quantity_on_the_line`, `item:all_lines_of_same_item`, `parent:all_items_with_same_parent`, `schedule:all_items_on_same_schedule`; `use_when`: `matrix_items_by_parent_quantity` |
| `overheadtype` | select | Overhead Type | yes | no |  | `purpose`: `overhead_type`; `requires`: `cost_categories_and_overhead_items_defined`; `constraint`: `one_percent_of_total_category_per_cost_template` |
| `parent` | select | Subitem of | no | no |  | `purpose`: `parent_item_of_subitem` |
| `pricinggroup` | select | Pricing Group | no | no |  | `purpose`: `pricing_group`; `use_when`: `customer_specific_group_price_levels`; `allowed_values_listed_in`: `accounting_list_pricing_group` |
| `purchasedescription` | textarea | Purchase Description | no | no |  | `purpose`: `purchase_description`; `max_length`: `999` characters; `displayed_on`: `vendor_orders`; `recommendation`: `include_unit_of_measure`; `ui_effect`: `text_enhance_button_may_appear` |
| `purchaseorderamount` | posfloat | Vendor Bill - Purchase Order Amount Tolerance | no | no |  | `purpose`: `tolerance_bill_vs_po_amount` |
| `purchaseorderquantity` | posfloat | Vendor Bill - Purchase Order Quantity Tolerance | no | no |  | `purpose`: `tolerance_bill_vs_po_quantity` |
| `purchaseorderquantitydiff` | posfloat | Vendor Bill - Purchase Order Quantity Difference | no | no |  | `purpose`: `tolerance_bill_vs_po_quantity_difference` |
| `purchaseunit` | select | Purchase Units | no | no |  | `purpose`: `purchase_unit` |
| `quantitypricingschedule` | select | Quantity Pricing Schedule | no | no |  | `purpose`: `quantity_pricing_schedule`; `effect`: `sets_usemarginalrates_and_calculate_discounts`; `allowed_values_listed_in`: `quantity_pricing_schedule_records` |
| `receiptamount` | posfloat | Vendor Bill - Item Receipt Amount Tolerance | no | no |  | `purpose`: `tolerance_bill_vs_receipt_amount` |
| `receiptquantity` | posfloat | Vendor Bill - Item Receipt Quantity Tolerance | no | no |  | `purpose`: `tolerance_bill_vs_receipt_quantity` |
| `receiptquantitydiff` | posfloat | Vendor Bill - Item Receipt Quantity Difference | no | no |  | `purpose`: `tolerance_bill_vs_receipt_quantity_difference` |
| `residual` | rate | Residual | no | no |  | `purpose`: `residual_amount_not_amortized`; `caveat`: `transaction_residual_overrides_item_residual` |
| `revenueallocationgroup` | select | Revenue Allocation Group | no | no |  | `purpose`: `revenue_allocation_group`; `use_when`: `groupsum_in_fair_value_formulas`; `effect`: `default_on_revenue_element`; `can`: `change_on_revenue_element` |
| `revenuerecognitionrule` | select | Revenue Recognition Rule | no | no |  | `purpose`: `default_revenue_rule`; `recommendation`: `match_create_revenue_plans_on` |
| `revrecforecastrule` | select | Rev Rec Forecast Rule | yes | no |  | `purpose`: `forecast_revenue_rule`; `default_behavior`: `same_preference_as_revenue_rule`; `not_supported_for`: `percent_complete_rules` |
| `revreclassfxaccount` | select | Foreign Currency Adjustment Account | no | no |  | `purpose`: `revenue_reclass_fx_account`; `allowed_values`: `income`, `other_income`, `expense`, `other_expense`; `can`: `change_on_revenue_element_before_plans` |
| `revrecschedule` | select | Revenue Recognition Template | no | no |  | `purpose`: `default_revenue_recognition_template`; `requires`: `deferredrevenueaccount` |
| `salesdescription` | textarea | Sales Description | no | no |  | `purpose`: `sales_description`; `max_length`: `999` characters; `displayed_on`: `sales_transactions`, `web_store_cart`; `displayed_on`: `web_store_item_lists` (when `no_store_description`); `accepts_forms`: `letters`, `numbers` |
| `saleunit` | select | Sale Units | no | no |  | `purpose`: `sales_unit` |
| `softdescriptor` | select | Soft Descriptor | no | no |  | `purpose`: `credit_card_soft_descriptor`; `displayed_on`: `customer_card_statement`; `allowed_values_listed_in`: `credit_card_soft_descriptors` |
| `subsidiary` | select | Subsidiary | no | no |  | `purpose`: `item_subsidiaries`; `value_kind`: `multi_select`; `constraint`: `item_usable_only_on_its_subsidiaries_transactions`; `value_examples`: `only_wolfe_us_items_on_wolfe_us_orders` |
| `taxschedule` | select | Tax Schedule | yes | no |  | `purpose`: `tax_schedule`; `allowed_values_listed_in`: `tax_schedule_records` |
| `unitstype` | select | Units Type | no | no |  | `purpose`: `units_type` |
| `upccode` | text | UPC Code | no | no |  | `purpose`: `upc_code`; `max_length`: `999` characters; `ui_effect`: `item_labels_print_upc_barcode`; `default_behavior`: `labels_print_item_name_code128` (when `blank`) |
| `usemarginalrates` | checkbox | Use Marginal Rates | no | no |  | `purpose`: `use_marginal_rates`; `true_means`: `discount_per_bracket`; `false_means`: `discount_applies_to_all_units`; `value_examples`: `first_100_full_price_next_50_at_5_percent` |
| `vendorname` | text | Vendor Name/Code | no | no |  | `purpose`: `vendor_item_name`; `max_length`: `60` characters; `displayed_on`: `purchase_forms_item_column` |
| `vsoedeferral` | select | Deferral | no | no |  | `purpose`: `bundle_deferral`; `options`: `Defer Bundle Until Delivered`, `Defer Until Item Delivered`; `default_value`: `Defer Until Item Delivered`; `option_meanings`: `defer_bundle_until_delivered:whole_bundle_waits_for_item`, `defer_until_item_delivered:item_waits_for_itself`; `caveat`: `interacts_with_other_bundle_items_settings` |
| `vsoedelivered` | checkbox | Default as Delivered | no | no |  | `purpose`: `auto_mark_delivered`; `effect`: `delivered_status_when_added_to_transaction` |
| `vsoepermitdiscount` | select | Permit Discount | no | no |  | `purpose`: `vsoe_discount_handling`; `options`: `As Allowed`, `Never`; `caveat`: `vsoe_items_never_discount_with_residual_arm` |
| `vsoeprice` | currency2 | VSOE Price | no | no |  | `purpose`: `vsoe_price`; `can`: `change_per_order` |
| `vsoesopgroup` | select | Allocation Type | no | no |  | `purpose`: `revenue_allocation_type`; `options`: `Normal`, `Exclude`, `Software`; `option_meanings`: `exclude:discounted_amount_is_revenue`; `requires_one_of`: `vsoe_with_eitf_suiteapp`, `advanced_revenue_management` (when `software_option`); `option_meanings`: `normal:eitf_08_01_allocation`, `software:eitf_and_sop_97_2_or_fair_value_list`; `effect`: `software_reallocates_by_residual_when_estimates` |
| `costforpricing` | currency2 | Cost for Pricing | no | no | marked_new |  |
| `costunits` | text |  | no | no |  |  |
| `createddate` | datetime | Date Created | no | no |  |  |
| `externalid` | text | ExternalId | no | no |  |  |
| `isgcocompliant` | checkbox | Allow Google Checkout Purchase | no | yes |  |  |
| `itemoptions` | select | Item Options | no | no |  |  |
| `lastmodifieddate` | datetime | Last Modified | no | no |  |  |
| `matrixtype` | select | Matrix Type | no | no |  |  |
| `minimumquantityunits` | text |  | no | no |  |  |
| `subtype` | text |  | no | no |  |  |

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

## Sublist `hierarchyversions` (Hierarchy Versions)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `hierarchynode` | select | Hierarchy Node | no |  |  |
| `isincluded` | checkbox | Included In Version | no |  |  |

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
| `matrix` | Matrix |
| `pricing` | Pricing |
| `projects` | Projects |
| `relitems` | Related Items |
| `specials` | Specials |
| `store` | Store |
| `translation` | Translation |
| `vsoepricing` | Rev Rec / Amort |
| `workflow` | Workflow |

## Suspected source errors

> [!SOURCE-ERROR] `full_level_but_client_and_server_unchecked`
<!-- source-error: record:otherchargeitem | - | full_level_but_client_and_server_unchecked -->

> [!SOURCE-ERROR] `item_record_help_describes_label_printing` (on `itemtype`)
<!-- source-error: record:otherchargeitem | itemtype | item_record_help_describes_label_printing -->

> [!SOURCE-ERROR] `other_items_call_scheme_oss` (on `mossapplies`)
<!-- source-error: record:otherchargeitem | mossapplies | other_items_call_scheme_oss -->
