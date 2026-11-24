# Service (`serviceitem`)
<!-- id: record:serviceitem -->

| Fact | Value |
|---|---|
| Internal ID | `serviceitem` |
| Category | item |
| Client scripts | yes |
| Server scripts | yes |
| Searchable | yes |
| Search data via | Item |
| Tags | `custom_fields_supported`, `scripting_full` |

## Record facts

- `initialization_default_value`: `subtype`, `sale`, `resale`, `purchase` (refs `N/record.create`, `N/record.copy`, `N/record.transform`, `N/record.load`)
- `script_types`: `client`, `server`
- `script_functions`: `create`, `update`, `copy`, `delete`, `search`

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
| `createjob` | checkbox | Create Project | no | no |  | `purpose`: `create_job_when_sold` |
| `createrevenueplanson` | select | Create Revenue Plans On | no | no |  | `purpose`: `revenue_plan_creation_event`; `options`: `Revenue Arrangement Creation`, `Billing`, `Fulfillment`, `Project Progress`; `requires_feature`: `Advanced Shipping` (when `fulfillment_option`) [`real_name`]; `requires_feature`: `Projects` (when `project_progress_option`) [`real_name`]; `recommendation`: `match_rule_amount_source`; `option_meanings`: `revenue_arrangement_creation:rule_source_event_amount`, `billing:rule_source_event_percent_amount`, `fulfillment:rule_source_event_percent_quantity`, `project_progress:rule_source_percent_complete`; `applies_to`: `sales_orders`, `cash_sales`, `invoices`, `credit_memos`, `cash_refunds` (when `billing_option`) |
| `currency` | text | Currency | no | no |  | `purpose`: `purchase_price_currency`; `value_source`: `preferred_vendor_currency`; `default_behavior`: `base_currency` (when `no_preferred_vendor`) |
| `customform` | select | Custom Form | no | no |  | `purpose`: `entry_form`; `allowed_values_listed_in`: `standard_and_custom_entry_forms`; `ui_effect`: `new_option_creates_record` |
| `deferralaccount` | select | Deferred Expense Account | no | no |  | `purpose`: `deferred_expense_account`; `constraint`: `template_needs_own_deferred_account_when_blank` |
| `deferredrevenueaccount` | select | Deferred Revenue Account | no | no |  | `purpose`: `deferred_revenue_account`; `applies_when`: `revenue_recognition_or_arm` |
| `deferrevrec` | checkbox | Hold Revenue Recognition | no | no |  | `purpose`: `defer_revenue_recognition`; `true_means`: `plans_created_on_hold` |
| `department` | select | Department | no | yes |  | `purpose`: `item_department`; `allowed_values_listed_in`: `department_records`; `ui_effect`: `new_option_creates_record` |
| `directrevenueposting` | checkbox | Direct Revenue Posting | no | no |  | `purpose`: `bypass_advanced_revenue_management`; `true_means`: `posts_directly_to_revenue_no_arrangement`; `ui_effect`: `disables_deferred_revenue_account`; `constraint`: `all_transaction_items_must_match`; `constraint`: `all_kit_members_must_match`; `cannot`: `change_after_used_with_arm`; `default_value`: `false` |
| `displayname` | text | Display Name/Code | no | no |  | `purpose`: `display_name`; `max_length`: `250` characters; `displayed_on`: `sales_forms_item_column` (when `basic_printing`); `default_behavior`: `item_name_on_printed_forms` (when `blank`) |
| `dontshowprice` | checkbox | Don't Show Price | no | no |  | `purpose`: `hide_price_online`; `use_when`: `advertised_not_sold_or_sold_in_combination` |
| `enforceminqtyinternally` | checkbox | Enforce Internally | no | no |  | `purpose`: `enforce_minimum_quantity_internally`; `false_means`: `minimum_applies_to_web_store_only` |
| `excludefromsitemap` | checkbox | Exclude From Sitemap | no | no |  | `purpose`: `exclude_from_site_map` |
| `expenseaccount` | select | Expense Account | no | no |  | `purpose`: `expense_account` |
| `featureddescription` | textarea | Featured Description | no | no |  | `purpose`: `featured_description`; `displayed_on`: `web_store_home_page`; `max_length`: `999` characters; `accepts_forms`: `text`, `basic_html`; `ui_effect`: `text_enhance_button_may_appear` |
| `generateaccruals` | checkbox | Generate Accruals | no | no |  | `purpose`: `generate_accruals`; `effect`: `posts_accrual_to_gl` |
| `includechildren` | checkbox | Include Children | no | no |  | `purpose`: `share_item_with_child_subsidiaries`; `constraint`: `item_settings_compatible_across_subsidiaries`; `value_examples`: `shared_items_need_shared_income_and_asset_accounts` |
| `incomeaccount` | select | Income Account | no | no |  | `purpose`: `income_account`; `ui_effect`: `hidden_from_sales_item_list_when_blank` |
| `intercodefrevaccount` | select | Intercompany Deferred Revenue Account | no | no |  | `purpose`: `intercompany_deferred_revenue_account`; `requires_feature`: `OneWorld` [`real_name`]; `constraint`: `account_eliminates_intercompany` |
| `intercoexpenseaccount` | select | Intercompany Expense Account | no | no |  | `purpose`: `intercompany_expense_account`; `requires_feature`: `OneWorld` [`real_name`]; `constraint`: `account_eliminates_intercompany`; `caveat`: `elimination_impossible_if_account_flag_cleared`; `applies_to`: `inventory_items`, `non_inventory_resale`, `other_charge_resale`, `service_resale` |
| `intercoincomeaccount` | select | Intercompany Income Account | no | no |  | `purpose`: `intercompany_income_account`; `requires_feature`: `OneWorld` [`real_name`]; `constraint`: `account_eliminates_intercompany`; `applies_to`: `inventory_items`, `non_inventory_resale`, `other_charge_resale`, `service_resale`; `caveat`: `elimination_impossible_if_account_flag_cleared` |
| `internalid` | integer | Internal ID | no | no |  | `purpose`: `internal_id`; `use_when`: `smart_tags_in_site_templates`; `doc_inconsistency`: `help_text_truncated` |
| `isdonationitem` | checkbox | Variable Amount | no | no |  | `purpose`: `variable_price_item`; `true_means`: `customer_enters_price`; `use_when`: `donations` |
| `isfulfillable` | checkbox | Can be Fulfilled/Received | no | no |  | `purpose`: `can_be_fulfilled`; `requires_feature`: `Advanced Billing`, `Advanced Shipping` [`real_name`] |
| `isinactive` | checkbox | Inactive | no | no |  | `purpose`: `inactivate_item`; `ui_effect`: `hidden_from_lists_unless_show_all` |
| `isonline` | checkbox | Display in Web Store | no | no |  | `purpose`: `available_online`; `requires`: `checked_to_sell_online`; `caveat`: `other_charge_only_at_checkout_not_catalog` |
| `issueproduct` | select | Product Name | no | no |  | `purpose`: `issue_management_product`; `allowed_values_listed_in`: `issue_product_records`; `value_kind`: `product_with_versions_and_builds` |
| `itemid` | text | Item Name/Number | yes | no |  | `purpose`: `item_name`; `max_length`: `250` characters; `displayed_on`: `transaction_lists`; `displayed_on`: `sales_forms_item_column` (when `no_display_name`); `displayed_on`: `purchase_forms_item_column` (when `no_vendor_name`) |
| `itemrevenuecategory` | select | Item Revenue Category | no | no |  | `purpose`: `item_revenue_category`; `use_when`: `fair_value_price_list` |
| `itemtype` | text |  | no | no |  | `purpose`: `item_type_for_label_printing`; `special_values`: `All`; `doc_inconsistency`: `item_record_help_describes_label_printing` |
| `location` | select | Location | no | yes |  | `purpose`: `item_location`; `requires_feature`: `Multi-Location Inventory` (when `tracking_inventory_by_location`) [`real_name`]; `effect`: `limits_role_access_to_item`; `ui_effect`: `new_option_creates_record`; `use_instead`: `locations_sublist` (when `tracking_inventory_per_location`) |
| `manufacturingchargeitem` | checkbox | Manufacturing Charge Item | no | no |  | `purpose`: `manufacturing_charge_item`; `value_examples`: `hourly_activity_cost_and_expense_account` |
| `matrixitemnametemplate` | textarea | Matrix Item Name Template | no | no |  | `purpose`: `matrix_item_name_template`; `value_kind`: `attribute_and_option_order_with_separators`; `effect`: `controls_matrix_item_name_display`; `value_examples`: `fabric_color_waist_length_location` |
| `maxdonationamount` | currency2 | Maximum Variable Amount | no | no |  | `purpose`: `maximum_donation_amount`; `applies_when`: `variable_amount_item` |
| `maximumquantity` | posinteger | Maximum Quantity | no | yes |  | `purpose`: `maximum_purchase_quantity`; `ui_effect`: `warning_above_maximum`; `effect`: `web_store_checkout_blocked_above_maximum`; `default_behavior`: `no_maximum` (when `blank`); `configured_by`: `site_builder_customize_text` [`warning_text`] |
| `metataghtml` | textarea | Meta Tag HTML | no | no |  | `purpose`: `meta_tag_html`; `displayed_on`: `web_store`; `value_kind`: `html_head_meta_tags` |
| `minimumquantity` | posinteger | Minimum Quantity | no | yes |  | `purpose`: `minimum_purchase_quantity`; `ui_effect`: `warning_below_minimum`; `effect`: `web_store_checkout_blocked_below_minimum`; `default_behavior`: `no_minimum` (when `blank`); `configured_by`: `site_builder_customize_text` [`warning_text`] |
| `mossapplies` | checkbox | MOSS Applies | no | no |  | `purpose`: `moss_vat_applies`; `constraint`: `not_in_item_groups`; `disagreement`: `other_items_call_scheme_oss` [`page_value_kept`] |
| `nopricemessage` | text | No Price Message | no | no |  | `purpose`: `message_instead_of_price_online`; `applies_when`: `price_hidden_online`; `value_examples`: `call_for_price` |
| `offersupport` | checkbox | Offer Support | no | no |  | `purpose`: `offer_support`; `effect`: `selectable_on_case_records` |
| `outofstockbehavior` | select | Out Of Stock Behavior | no | no |  | `purpose`: `out_of_stock_behavior`; `options`: `Default`, `disallow_back_orders_show_message`, `allow_back_orders_show_message`, `remove_when_out_of_stock`, `allow_back_orders_no_message`; `option_meanings`: `default:web_site_out_of_stock_setting`; `effect`: `overrides_site_out_of_stock_behavior` |
| `outofstockmessage` | textarea | Out Of Stock Message | no | no |  | `purpose`: `custom_out_of_stock_message`; `effect`: `replaces_default_message` |
| `pagetitle` | textarea | Page Title | no | no |  | `purpose`: `web_page_title`; `displayed_on`: `browser_tab` |
| `parent` | select | Subitem of | no | no |  | `purpose`: `parent_item_of_subitem` |
| `pricinggroup` | select | Pricing Group | no | no |  | `purpose`: `pricing_group`; `use_when`: `customer_specific_group_price_levels`; `allowed_values_listed_in`: `accounting_list_pricing_group` |
| `purchasedescription` | textarea | Purchase Description | no | no |  | `purpose`: `purchase_description`; `max_length`: `999` characters; `displayed_on`: `vendor_orders`; `recommendation`: `include_unit_of_measure`; `ui_effect`: `text_enhance_button_may_appear` |
| `purchaseorderamount` | posfloat | Vendor Bill - Purchase Order Amount Tolerance | no | no |  | `purpose`: `tolerance_bill_vs_po_amount` |
| `purchaseorderquantity` | posfloat | Vendor Bill - Purchase Order Quantity Tolerance | no | no |  | `purpose`: `tolerance_bill_vs_po_quantity` |
| `purchaseorderquantitydiff` | posfloat | Vendor Bill - Purchase Order Quantity Difference | no | no |  | `purpose`: `tolerance_bill_vs_po_quantity_difference` |
| `purchaseunit` | select | Purchase Units | no | no |  | `purpose`: `purchase_unit` |
| `receiptamount` | posfloat | Vendor Bill - Item Receipt Amount Tolerance | no | no |  | `purpose`: `tolerance_bill_vs_receipt_amount` |
| `receiptquantity` | posfloat | Vendor Bill - Item Receipt Quantity Tolerance | no | no |  | `purpose`: `tolerance_bill_vs_receipt_quantity` |
| `receiptquantitydiff` | posfloat | Vendor Bill - Item Receipt Quantity Difference | no | no |  | `purpose`: `tolerance_bill_vs_receipt_quantity_difference` |
| `relateditemsdescription` | textarea | Related Items Description | no | no |  | `purpose`: `related_items_description`; `max_length`: `999` characters; `accepts_forms`: `text`, `basic_html` |
| `residual` | rate | Residual | no | no |  | `purpose`: `residual_amount_not_amortized`; `caveat`: `transaction_residual_overrides_item_residual` |
| `revenueallocationgroup` | select | Revenue Allocation Group | no | no |  | `purpose`: `revenue_allocation_group`; `use_when`: `groupsum_in_fair_value_formulas`; `effect`: `default_on_revenue_element`; `can`: `change_on_revenue_element` |
| `revenuerecognitionrule` | select | Revenue Recognition Rule | no | no |  | `purpose`: `default_revenue_rule`; `recommendation`: `match_create_revenue_plans_on` |
| `revrecforecastrule` | select | Rev Rec Forecast Rule | yes | no |  | `purpose`: `forecast_revenue_rule`; `default_behavior`: `same_preference_as_revenue_rule`; `not_supported_for`: `percent_complete_rules` |
| `revreclassfxaccount` | select | Foreign Currency Adjustment Account | no | no |  | `purpose`: `revenue_reclass_fx_account`; `allowed_values`: `income`, `other_income`, `expense`, `other_expense`; `can`: `change_on_revenue_element_before_plans` |
| `revrecschedule` | select | Revenue Recognition Template | no | no |  | `purpose`: `default_revenue_recognition_template`; `requires`: `deferredrevenueaccount` |
| `salesdescription` | textarea | Sales Description | no | no |  | `purpose`: `sales_description`; `max_length`: `999` characters; `displayed_on`: `sales_transactions`, `web_store_cart`; `displayed_on`: `web_store_item_lists` (when `no_store_description`); `accepts_forms`: `letters`, `numbers` |
| `saleunit` | select | Sale Units | no | no |  | `purpose`: `sales_unit` |
| `searchkeywords` | textarea | Search Keywords | no | no |  | `purpose`: `web_store_search_keywords`; `caveat`: `weighted_equal_to_item_name`; `value_examples`: `synonyms`, `acronyms`, `alternate_languages`, `misspellings` |
| `showdefaultdonationamount` | checkbox | Show Default Amount | no | no |  | `purpose`: `suggest_online_price_for_donation`; `true_means`: `online_price_shown_then_any_amount_allowed` |
| `sitemappriority` | select | Sitemap Priority | no | no |  | `purpose`: `sitemap_priority`; `allowed_range`: `0.0`, `1.0`; `default_value`: `Auto`; `value_source`: `site_hierarchy_position` (when `auto`); `value_examples`: `tab:1.0`, `category_on_tab:0.5`, `item_in_category:0.3`; `applies_to`: `item_records`, `category_records`, `tab_records` |
| `softdescriptor` | select | Soft Descriptor | no | no |  | `purpose`: `credit_card_soft_descriptor`; `displayed_on`: `customer_card_statement`; `allowed_values_listed_in`: `credit_card_soft_descriptors` |
| `storedescription` | textarea | Web Store Description | no | no |  | `purpose`: `store_description`; `displayed_on`: `web_store_item_lists`; `configured_by`: `web_site_cart_store_description_setting` [`cart_display`]; `ui_effect`: `text_enhance_button_may_appear` |
| `storedetaileddescription` | textarea | Detailed Description | no | no |  | `purpose`: `store_detailed_description`; `displayed_on`: `web_store_item_detail`; `ui_effect`: `text_enhance_button_may_appear` |
| `storedisplayimage` | select | Item Display Image | no | no |  | `purpose`: `store_display_image`; `value_source`: `file_cabinet_image`; `caveat`: `auto_resize_only_under_5_megapixels` |
| `storedisplayname` | text | Web Store Display Name | no | no |  | `purpose`: `store_display_name`; `max_length`: `250` characters |
| `storedisplaythumbnail` | select | Item Display Thumbnail | no | no |  | `purpose`: `store_thumbnail_image`; `value_source`: `file_cabinet_image`; `displayed_on`: `web_store_before_item_click`; `caveat`: `auto_resize_only_under_5_megapixels` |
| `storeitemtemplate` | select | Item Drilldown Template | no | no |  | `purpose`: `store_item_template`; `allowed_values_listed_in`: `item_template_records`; `configured_by`: `site_templates_body_default_item_template` |
| `subsidiary` | select | Subsidiary | no | no |  | `purpose`: `item_subsidiaries`; `value_kind`: `multi_select`; `constraint`: `item_usable_only_on_its_subsidiaries_transactions`; `value_examples`: `only_wolfe_us_items_on_wolfe_us_orders` |
| `taxschedule` | select | Tax Schedule | yes | no |  | `purpose`: `tax_schedule`; `allowed_values_listed_in`: `tax_schedule_records` |
| `tracksaasmetrics` | checkbox | Track SaaS Metrics | no |  |  | `purpose`: `track_saas_metrics`; `applies_to`: `subscriptions`, `sales_orders`, `cash_sales`, `invoices` |
| `tracksaasmetricsenddate` | date | SaaS Metric End Date | no |  |  | `purpose`: `saas_metrics_end_date` |
| `tracksaasmetricsstartdate` | date | SaaS Metric Start Date | no |  |  | `purpose`: `saas_metrics_start_date`; `constraint`: `only_start_dates_within_window_tracked` |
| `unitstype` | select | Units Type | no | no |  | `purpose`: `units_type` |
| `upccode` | text | UPC Code | no | no |  | `purpose`: `upc_code`; `max_length`: `999` characters; `ui_effect`: `item_labels_print_upc_barcode`; `default_behavior`: `labels_print_item_name_code128` (when `blank`) |
| `urlcomponent` | textarea | URL Component | no | no |  | `purpose`: `url_component`; `caveat`: `case_sensitive`; `default_behavior`: `system_identifiers_in_url` (when `blank`) |
| `vendorname` | text | Vendor Name/Code | no | no |  | `purpose`: `vendor_item_name`; `max_length`: `60` characters; `displayed_on`: `purchase_forms_item_column` |
| `vsoedeferral` | select | Deferral | no | no |  | `purpose`: `bundle_deferral`; `options`: `Defer Bundle Until Delivered`, `Defer Until Item Delivered`; `default_value`: `Defer Until Item Delivered` |
| `vsoedelivered` | checkbox | Default as Delivered | no | no |  | `purpose`: `auto_mark_delivered`; `effect`: `delivered_status_when_added_to_transaction` |
| `vsoepermitdiscount` | select | Permit Discount | no | no |  | `purpose`: `vsoe_discount_handling`; `options`: `As Allowed`, `Never`; `caveat`: `vsoe_items_never_discount_with_residual_arm` |
| `vsoeprice` | currency2 | VSOE Price | no | no |  | `purpose`: `vsoe_price`; `can`: `change_per_order` |
| `vsoesopgroup` | select | Allocation Type | no | no |  | `purpose`: `revenue_allocation_type`; `options`: `Normal`, `Exclude`, `Software`; `option_meanings`: `exclude:discounted_amount_is_revenue`; `requires_one_of`: `vsoe_with_eitf_suiteapp`, `advanced_revenue_management` (when `software_option`); `option_meanings`: `normal:eitf_08_01_allocation`, `software:eitf_and_sop_97_2_or_fair_value_list`; `effect`: `software_reallocates_by_residual_when_estimates` |
| `auctionquantity` | posinteger | Auction Quantity | no | no |  |  |
| `auctiontype` | select | Auction Type | no | no |  |  |
| `buyitnowprice` | currency | Buy It Now Price | no | yes |  |  |
| `conditionenabled` | text |  | no | no |  |  |
| `conditionhelpurl` | text |  | no | no |  |  |
| `costforpricing` | currency2 | Cost for Pricing | no | no | marked_new |  |
| `costunits` | text |  | no | no |  |  |
| `createddate` | datetime | Date Created | no | no |  |  |
| `displayinebaystore` | checkbox | eBay Store | no | yes |  |  |
| `ebayhandlingtime` | select | Handling Time | no | no |  |  |
| `ebayintlinsurancefee` | currency | Insurance Amount | no | no |  |  |
| `ebayintlpackagehandlingfee` | currency | Package & Handling Fee | no | no |  |  |
| `ebayintlshipinsurance` | select | Shipping Insurance | no | no |  |  |
| `ebayintlshippingitem1` | select | Shipping Item 1 | no | no |  |  |
| `ebayintlshippingitem2` | select | Shipping Item 2 | no | no |  |  |
| `ebayintlshippingitem3` | select | Shipping Item 3 | no | no |  |  |
| `ebayisintlcalculatedrate` | radio | Flat Rates | no | no |  |  |
| `ebayisirregularpackage` | checkbox | Irregular Package | no | no |  |  |
| `ebayitemdescription` | textarea | Item Description | no | no |  |  |
| `ebayitemlocdisplay` | text | Item Location | no | no |  |  |
| `ebayitemloczipcode` | text | Item Location Zip Code | no | no |  |  |
| `ebayitemlots` | integer | Lot Size | no | no |  |  |
| `ebayitemsubtitle` | text | Subtitle | no | yes |  |  |
| `ebayitemtitle` | text | Item Title | no | yes |  |  |
| `ebayitemweightamt` | posfloat | Item Weight | no | no |  |  |
| `ebaylayout` | select | Default Chart Type | no | no |  |  |
| `ebaypackagetype` | select | Package | no | no |  |  |
| `ebaypagecounter` | select | Page Counter | no | no |  |  |
| `ebayrelistingoption` | select | Relisting Option | no | no |  |  |
| `ebaytheme` | select | Theme | no | no |  |  |
| `ebaythemegroup` | select | Theme Group | no | no |  |  |
| `endauctionswhenoutofstock` | checkbox | End Auctions When Item is Out of Stock | no | yes |  |  |
| `externalid` | text | ExternalId | no | no |  |  |
| `gallery` | checkbox | Gallery | no | no |  |  |
| `galleryfeatured` | checkbox | Featured First | no | no |  |  |
| `gifttypeexpressship` | checkbox | Express shipping | no | no |  |  |
| `gifttypegiftwrap` | checkbox | Gift wrap / gift card | no | no |  |  |
| `gifttypeshiptorecipient` | checkbox | Ship to gift recipient | no | no |  |  |
| `handlinggroup` | text | Handling | no | no |  |  |
| `imagesgroup` | text | Images | no | no |  |  |
| `imageslocation` | select | Images Location | no | no |  |  |
| `iscalculatedrate` | radio | Flat Rates | no | no |  |  |
| `isgcocompliant` | checkbox | Allow Google Checkout Purchase | no | yes |  |  |
| `itemcondition` | select | Item Condition | no | no |  |  |
| `itemhandlingfee` | currency | Package & Handling Fee | no | no |  |  |
| `iteminsurancefee` | currency | Insurance Amount | no | no |  |  |
| `itemoptions` | select | Item Options | no | no |  |  |
| `itemshipinsurance` | select | Shipping Insurance | no | no |  |  |
| `lastmodifieddate` | datetime | Last Modified | no | no |  |  |
| `listimmediate` | radio | Start listing when submitted | no | no |  |  |
| `listingduration` | select | Duration | no | no |  |  |
| `listingstartdate` | date | Start Date | no | no |  |  |
| `listingstarttime` | select | Start Time | no | no |  |  |
| `matrixtype` | select | Matrix Type | no | no |  |  |
| `minimumquantityunits` | text |  | no | no |  |  |
| `numactivelistings` | integer | Active Listings | no | yes |  |  |
| `numcurrentlylisted` | integer | Quantity Currently Listed | no | yes |  |  |
| `overheadtype` | select | Overhead Type | yes | no |  |  |
| `packageheight` | posinteger | Height | no | no |  |  |
| `packagelength` | posinteger | Length | no | no |  |  |
| `packagewidth` | posinteger | Width | no | no |  |  |
| `primarycatdisplayname` | text |  | no | no |  |  |
| `primarycategory` | integer | Primary Category | no | yes |  |  |
| `refundgivenas` | select | Refund will be given as | no | no |  |  |
| `reserveprice` | currency | Reserve Price | no | yes |  |  |
| `returnpolicy` | select | Return Policy | no | no |  |  |
| `returnpolicydetails` | textarea | Details | no | no |  |  |
| `returnshippingpaidby` | select | Return Shipping will be paid by | no | no |  |  |
| `returnswithin` | select | Item must be returned within | no | no |  |  |
| `secondarycatdisplayname` | text |  | no | no |  |  |
| `secondarycategory` | integer | Secondary Category | no | no |  |  |
| `sellonebay` | checkbox | Sell on eBay | no | yes |  |  |
| `shipasia` | checkbox | Asia | no | no |  |  |
| `shipaustralia` | checkbox | Australia | no | no |  |  |
| `shipcanada` | checkbox | Canada | no | no |  |  |
| `shipeurope` | checkbox | Europe | no | no |  |  |
| `shipgermany` | checkbox | Germany | no | no |  |  |
| `shipjapan` | checkbox | Japan | no | no |  |  |
| `shipmexico` | checkbox | Mexico | no | no |  |  |
| `shipnorthsouthamerica` | checkbox | North and South America | no | no |  |  |
| `shippingdomesticmethodsgroup` | text | Shipping Methods | no | no |  |  |
| `shippingdomgroup` | text | Domestic Shipping Settings | no | no |  |  |
| `shippingintlgroup` | text | International Shipping Settings | no | no |  |  |
| `shippingintlgroup1` | text | Shipping Method 1 | no | no |  |  |
| `shippingintlgroup2` | text | Shipping Method 2 | no | no |  |  |
| `shippingintlgroup3` | text | Shipping Method 3 | no | no |  |  |
| `shippingitem1` | select | Shipping Item 1 | no | no |  |  |
| `shippingitem2` | select | Shipping Item 2 | no | no |  |  |
| `shippingitem3` | select | Shipping Item 3 | no | no |  |  |
| `shippinglocationsgroup` | text | Ship-to Locations | no | no |  |  |
| `shippingpackaginggroup` | text | Packaging | no | no |  |  |
| `shippingrate1` | currency | Rate | no | no |  |  |
| `shippingrate2` | currency | Rate | no | no |  |  |
| `shippingrate3` | currency | Rate | no | no |  |  |
| `shipuk` | checkbox | United Kingdom | no | no |  |  |
| `shipworldwide` | checkbox | Worldwide | no | no |  |  |
| `showasgift` | checkbox | Show as Gift | no | no |  |  |
| `standardimages` | radio | Standard | no | no |  |  |
| `startingprice` | currency | Starting Price | no | yes |  |  |
| `storecatdisplayname` | text |  | no | no |  |  |
| `storecatdisplayname2` | text |  | no | no |  |  |
| `storecategory` | integer | Store Category | no | no |  |  |
| `storecategory2` | integer | Secondary Store Category | no | no |  |  |
| `subtype` | text |  | no | no |  |  |
| `supersizeimages` | checkbox | Supersize Pictures | no | no |  |  |
| `taxable` | checkbox | Taxable | no | no |  |  |
| `templatesgroup` | text | Templates | no | no |  |  |
| `willship` | radio | Will Ship to the US and the following: | no | no |  |  |

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

## Sublist `sitecategory` (Web Site List)

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

> [!SOURCE-ERROR] `help_text_truncated` (on `internalid`)
<!-- source-error: record:serviceitem | internalid | help_text_truncated -->

> [!SOURCE-ERROR] `item_record_help_describes_label_printing` (on `itemtype`)
<!-- source-error: record:serviceitem | itemtype | item_record_help_describes_label_printing -->

> [!SOURCE-ERROR] `other_items_call_scheme_oss` (on `mossapplies`)
<!-- source-error: record:serviceitem | mossapplies | other_items_call_scheme_oss -->
