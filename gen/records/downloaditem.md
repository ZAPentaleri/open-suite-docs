# Download Item (`downloaditem`)
<!-- id: record:downloaditem -->

| Fact | Value |
|---|---|
| Internal ID | `downloaditem` |
| Category | item |
| Client scripts | yes |
| Server scripts | yes |
| Searchable | yes |
| Search data via | Item |
| Tags | `custom_fields_supported`, `scripting_full` |

## Record facts

- `script_types`: `client`, `server`
- `script_functions`: `create`, `update`, `copy`, `delete`, `search`
- `purpose`: `purchasable_downloadable_web_store_file`
- `value_examples`: `item_per_song_or_one_item_per_album`
- `displayed_on`: `customer_center`, `website_my_account`, `invoice_email` [`license_code`]
- `ui_location`: `customer_downloads_subtab` (when `adding_license_code`)

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `availabletopartners` | checkbox | Available to Adv. Partners | no | no |  | `purpose`: `visible_to_advanced_partner_center`; `applies_to`: `advanced_partner_center_roles`, `custom_roles_with_item_access` |
| `billingschedule` | select | Billing Schedule | no | no |  | `purpose`: `billing_schedule`; `effect`: `default_schedule_on_order_lines`; `requires`: `form_customized_to_show_line_schedules`; `allowed_values_listed_in`: `billing_schedule_records`; `ui_effect`: `new_option_creates_record` |
| `class` | select | Class | no | yes |  | `purpose`: `item_class`; `allowed_values_listed_in`: `class_records`; `ui_effect`: `new_option_creates_record` |
| `costestimate` | currency | Item Defined Cost | no | no |  | `purpose`: `item_defined_cost` |
| `costestimatetype` | select | Cost Estimate Type | no | no |  | `purpose`: `cost_basis_for_estimated_gross_profit`; `options`: `Item Defined Cost`, `Average Cost`, `Last Purchase Price`, `Purchase Price`, `Preferred Vendor Rate`, `Derived from member items`; `requires_feature`: `Multi-Vendor` (when `preferred_vendor_rate`) [`real_name`]; `stage_order`: `preferred_vendor_rate`, `purchase_price`, `purchase_order_rate` (when `preferred_vendor_rate`); `applies_to`: `kits` (when `derived_from_member_items`); `effect`: `drives_transaction_estimated_gross_profit`; `option_meanings`: `item_defined_cost:costestimate_field`, `average_cost:average_unit_cost`, `last_purchase_price:latest_po_receipt_price`, `purchase_price:cost_field_or_latest_po_price`; `caveat`: `derived_uses_current_kit_definition`; `caveat`: `po_rate_used_for_special_orders_and_drop_ships` |
| `createrevenueplanson` | select | Create Revenue Plans On | no | no |  | `purpose`: `revenue_plan_creation_event`; `options`: `Revenue Arrangement Creation`, `Billing`, `Fulfillment`, `Project Progress`; `requires_feature`: `Advanced Shipping` (when `fulfillment_option`) [`real_name`]; `requires_feature`: `Projects` (when `project_progress_option`) [`real_name`]; `recommendation`: `match_rule_amount_source`; `option_meanings`: `revenue_arrangement_creation:rule_source_event_amount`, `billing:rule_source_event_percent_amount`, `fulfillment:rule_source_event_percent_quantity`, `project_progress:rule_source_percent_complete`; `applies_to`: `sales_orders`, `cash_sales`, `invoices`, `credit_memos`, `cash_refunds` (when `billing_option`) |
| `customform` | select | Custom Form | no | no |  | `purpose`: `entry_form`; `allowed_values_listed_in`: `standard_and_custom_entry_forms`; `ui_effect`: `new_option_creates_record` |
| `daysbeforeexpiration` | integer | Days Before Expiration | no | no |  | `purpose`: `days_active_after_purchase`; `effect`: `countdown_restarts_on_repurchase` (when `download_item`); `constraint`: `gift_certificates_cannot_expire_in_some_states`; `allowed_values`: `CA`, `CT`, `LA`, `MA`, `NH`, `RI`, `WA` [`no_expiry_states`] |
| `deferredrevenueaccount` | select | Deferred Revenue Account | no | no |  | `purpose`: `deferred_revenue_account`; `applies_when`: `revenue_recognition_or_arm` |
| `deferrevrec` | checkbox | Hold Revenue Recognition | no | no |  | `purpose`: `defer_revenue_recognition`; `true_means`: `plans_created_on_hold` |
| `department` | select | Department | no | yes |  | `purpose`: `item_department`; `allowed_values_listed_in`: `department_records`; `ui_effect`: `new_option_creates_record` |
| `directrevenueposting` | checkbox | Direct Revenue Posting | no |  |  | `purpose`: `bypass_advanced_revenue_management`; `true_means`: `posts_directly_to_revenue_no_arrangement`; `ui_effect`: `disables_deferred_revenue_account`; `constraint`: `all_transaction_items_must_match`; `constraint`: `all_kit_members_must_match`; `cannot`: `change_after_used_with_arm`; `default_value`: `false` |
| `displayname` | text | Display Name/Code | no | no |  | `purpose`: `display_name`; `max_length`: `250` characters; `displayed_on`: `sales_forms_item_column` (when `basic_printing`); `default_behavior`: `item_name_on_printed_forms` (when `blank`) |
| `dontshowprice` | checkbox | Don't Show Price | no | no |  | `purpose`: `hide_price_online`; `use_when`: `advertised_not_sold_or_sold_in_combination` |
| `excludefromsitemap` | checkbox | Exclude From Sitemap | no | no |  | `purpose`: `exclude_from_site_map` |
| `featureddescription` | textarea | Featured Description | no | no |  | `purpose`: `featured_description`; `displayed_on`: `web_store_home_page`; `max_length`: `999` characters; `accepts_forms`: `text`, `basic_html` |
| `immediatedownload` | checkbox | Immediate Download | no | no |  | `purpose`: `immediate_download`; `true_means`: `download_after_checkout`; `false_means`: `download_after_billing_with_email` |
| `includechildren` | checkbox | Include Children | no | no |  | `purpose`: `share_item_with_child_subsidiaries`; `constraint`: `item_settings_compatible_across_subsidiaries`; `value_examples`: `shared_items_need_shared_income_and_asset_accounts` |
| `incomeaccount` | select | Income Account | no | no |  | `purpose`: `income_account`; `ui_effect`: `hidden_from_sales_item_list_when_blank` |
| `internalid` | integer | Internal ID | no | no |  | `purpose`: `internal_id`; `use_when`: `smart_tags_in_site_templates` |
| `isdonationitem` | checkbox | Variable Amount | no | no |  | `purpose`: `variable_price_item`; `true_means`: `customer_enters_price`; `use_when`: `donations` |
| `isfulfillable` | checkbox | Can be Fulfilled/Received | no | no |  | `purpose`: `can_be_fulfilled`; `cannot`: `change_after_used_on_transactions`; `requires_feature`: `Advanced Billing`, `Advanced Shipping` [`real_name`] |
| `isinactive` | checkbox | Inactive | no | no |  | `purpose`: `inactivate_item`; `ui_effect`: `hidden_from_lists_unless_show_all` |
| `isonline` | checkbox | Display in Web Store | no | no |  | `purpose`: `available_online`; `requires`: `checked_to_sell_online`; `caveat`: `other_charge_only_at_checkout_not_catalog` |
| `issueproduct` | select | Product Name | no | no |  | `purpose`: `issue_management_product`; `allowed_values_listed_in`: `issue_product_records`; `value_kind`: `product_with_versions_and_builds` |
| `itemid` | text | Item Name/Number | yes | no |  | `purpose`: `item_name`; `max_length`: `250` characters; `displayed_on`: `transaction_lists`; `displayed_on`: `sales_forms_item_column` (when `no_display_name`); `displayed_on`: `purchase_forms_item_column` (when `no_vendor_name`) |
| `itemrevenuecategory` | select | Item Revenue Category | no | no |  | `purpose`: `item_revenue_category`; `use_when`: `fair_value_price_list` |
| `itemtype` | text |  | no | no |  | `purpose`: `item_type_for_label_printing`; `special_values`: `All`; `doc_inconsistency`: `item_record_help_describes_label_printing` |
| `location` | select | Location | no | yes |  | `purpose`: `item_location`; `requires_feature`: `Multi-Location Inventory` (when `tracking_inventory_by_location`) [`real_name`]; `effect`: `limits_role_access_to_item`; `value_examples`: `role_limited_to_location_one_items`; `cannot`: `track_stock_by_location` (when `no_multi_location_inventory`) |
| `maxdonationamount` | currency2 | Maximum Variable Amount | no | no |  | `purpose`: `maximum_donation_amount`; `applies_when`: `variable_amount_item` |
| `metataghtml` | textarea | Meta Tag HTML | no | no |  | `purpose`: `meta_tag_html`; `displayed_on`: `web_store`; `value_kind`: `html_head_meta_tags` |
| `mossapplies` | checkbox | MOSS Applies | no | no |  | `purpose`: `oss_vat_applies`; `applies_to`: `service_items`; `caveat`: `oss_vat_applied_to_goods_automatically`; `constraint`: `not_in_item_groups` |
| `nopricemessage` | text | No Price Message | no | no |  | `purpose`: `message_instead_of_price_online`; `applies_when`: `price_hidden_online`; `value_examples`: `call_for_price` |
| `numofalloweddownloads` | integer | Number Of Allowed Downloads | no | no |  | `purpose`: `max_downloads`; `effect`: `multiplied_by_purchase_count` |
| `offersupport` | checkbox | Offer Support | no | no |  | `purpose`: `offer_support`; `effect`: `selectable_on_case_records` |
| `outofstockbehavior` | select | Out Of Stock Behavior | no | no |  | `purpose`: `out_of_stock_behavior`; `options`: `Default`, `disallow_back_orders_show_message`, `allow_back_orders_show_message`, `remove_when_out_of_stock`, `allow_back_orders_no_message`; `option_meanings`: `default:web_site_out_of_stock_setting`; `effect`: `overrides_site_out_of_stock_behavior` |
| `outofstockmessage` | textarea | Out Of Stock Message | no | no |  | `purpose`: `custom_out_of_stock_message`; `effect`: `replaces_default_message` |
| `overallquantitypricingtype` | select | Calculate Quantity Discounts | no | no |  | `purpose`: `quantity_basis_for_pricing`; `options`: `By Line Quantity`, `By Overall Item Quantity`, `By Overall Parent Quantity`, `By Overall Schedule Quantity`; `option_meanings`: `line:quantity_on_the_line`, `item:all_lines_of_same_item`, `parent:all_items_with_same_parent`, `schedule:all_items_on_same_schedule`; `use_when`: `matrix_items_by_parent_quantity` |
| `pagetitle` | textarea | Page Title | no | no |  | `purpose`: `web_page_title`; `displayed_on`: `browser_tab` |
| `parent` | select | Subitem of | no | no |  | `purpose`: `parent_item_of_subitem` |
| `pricinggroup` | select | Pricing Group | no | no |  | `purpose`: `pricing_group`; `use_when`: `customer_specific_group_price_levels`; `allowed_values_listed_in`: `accounting_list_pricing_group` |
| `quantitypricingschedule` | select | Quantity Pricing Schedule | no | no |  | `purpose`: `quantity_pricing_schedule`; `effect`: `sets_usemarginalrates_and_calculate_discounts`; `allowed_values_listed_in`: `quantity_pricing_schedule_records` |
| `relateditemsdescription` | textarea | Related Items Description | no | no |  | `purpose`: `related_items_description`; `max_length`: `999` characters; `accepts_forms`: `text`, `basic_html` |
| `revenueallocationgroup` | select | Revenue Allocation Group | no | no |  | `purpose`: `revenue_allocation_group`; `use_when`: `groupsum_in_fair_value_formulas`; `effect`: `default_on_revenue_element`; `can`: `change_on_revenue_element` |
| `revenuerecognitionrule` | select | Revenue Recognition Rule | no | no |  | `purpose`: `default_revenue_rule`; `recommendation`: `match_create_revenue_plans_on` |
| `revrecforecastrule` | select | Rev Rec Forecast Rule | yes |  |  | `purpose`: `forecast_revenue_rule`; `default_behavior`: `same_preference_as_revenue_rule`; `not_supported_for`: `percent_complete_rules` |
| `revrecschedule` | select | Revenue Recognition Template | no | no |  | `purpose`: `default_revenue_recognition_template`; `requires`: `deferredrevenueaccount` |
| `salesdescription` | textarea | Sales Description | no | no |  | `purpose`: `sales_description`; `max_length`: `999` characters; `displayed_on`: `sales_transactions`, `web_store_cart`; `displayed_on`: `web_store_item_lists` (when `no_store_description`); `accepts_forms`: `letters`, `numbers` |
| `searchkeywords` | textarea | Search Keywords | no | no |  | `purpose`: `web_store_search_keywords`; `caveat`: `weighted_equal_to_item_name` |
| `showdefaultdonationamount` | checkbox | Show Default Amount | no | no |  | `purpose`: `suggest_online_price_for_donation`; `true_means`: `online_price_shown_then_any_amount_allowed` |
| `sitemappriority` | select | Sitemap Priority | no | no |  | `purpose`: `sitemap_priority`; `allowed_range`: `0.0`, `1.0`; `default_value`: `Auto`; `value_source`: `site_hierarchy_position` (when `auto`); `value_examples`: `tab:1.0`, `category_on_tab:0.5`, `item_in_category:0.3`; `applies_to`: `item_records`, `category_records`, `tab_records` |
| `storedescription` | textarea | Web Store Description | no | no |  | `purpose`: `store_description`; `displayed_on`: `web_store_item_lists`; `configured_by`: `web_site_cart_store_description_setting` [`cart_display`]; `accepts_forms`: `text`, `basic_html`; `comparable_to`: `salesdescription_fallback` |
| `storedetaileddescription` | textarea | Detailed Description | no | no |  | `purpose`: `store_detailed_description`; `displayed_on`: `web_store_item_detail`; `accepts_forms`: `text`, `basic_html`; `configured_by`: `rich_text_editing_preference` [`view_source_box`] |
| `storedisplayimage` | select | Item Display Image | no | no |  | `purpose`: `store_display_image`; `value_source`: `file_cabinet_image`; `caveat`: `auto_resize_only_under_5_megapixels` |
| `storedisplayname` | text | Web Store Display Name | no | no |  | `purpose`: `store_display_name`; `max_length`: `250` characters |
| `storedisplaythumbnail` | select | Item Display Thumbnail | no | no |  | `purpose`: `store_thumbnail_image`; `value_source`: `file_cabinet_image`; `displayed_on`: `web_store_before_item_click`; `caveat`: `auto_resize_only_under_5_megapixels` |
| `storeitemtemplate` | select | Item Drilldown Template | no | no |  | `purpose`: `store_item_template`; `allowed_values_listed_in`: `item_template_records`; `configured_by`: `site_templates_body_default_item_template` |
| `subsidiary` | select | Subsidiary | no | no |  | `purpose`: `item_subsidiaries`; `value_kind`: `multi_select`; `constraint`: `item_usable_only_on_its_subsidiaries_transactions`; `value_examples`: `only_wolfe_us_items_on_wolfe_us_orders` |
| `taxschedule` | select | Tax Schedule | yes | no |  | `purpose`: `tax_schedule`; `allowed_values_listed_in`: `tax_schedule_records` |
| `upccode` | text | UPC Code | no | no |  | `purpose`: `upc_code`; `max_length`: `999` characters; `ui_effect`: `item_labels_print_upc_barcode`; `default_behavior`: `labels_print_item_name_code128` (when `blank`) |
| `urlcomponent` | textarea | URL Component | no | no |  | `purpose`: `url_component`; `caveat`: `case_sensitive`; `default_behavior`: `system_identifiers_in_url` (when `blank`) |
| `usemarginalrates` | checkbox | Use Marginal Rates | no | no |  | `purpose`: `use_marginal_rates`; `true_means`: `discount_per_bracket`; `false_means`: `discount_applies_to_all_units`; `value_examples`: `first_100_full_price_next_50_at_5_percent` |
| `vsoedeferral` | select | Deferral | no | no |  | `purpose`: `bundle_deferral`; `options`: `Defer Bundle Until Delivered`, `Defer Until Item Delivered`; `default_value`: `Defer Until Item Delivered`; `option_meanings`: `defer_bundle_until_delivered:whole_bundle_waits_for_item`, `defer_until_item_delivered:item_waits_for_itself`; `caveat`: `interacts_with_other_bundle_items_settings` |
| `vsoedelivered` | checkbox | Default as Delivered | no | no |  | `purpose`: `auto_mark_delivered`; `effect`: `delivered_status_when_added_to_transaction` |
| `vsoepermitdiscount` | select | Permit Discount | no | no |  | `purpose`: `vsoe_discount_handling`; `options`: `As Allowed`, `Never`; `caveat`: `vsoe_items_never_discount_with_residual_arm` |
| `vsoeprice` | currency2 | VSOE Price | no | no |  | `purpose`: `vsoe_price`; `can`: `change_per_order` |
| `vsoesopgroup` | select | Allocation Type | no | no |  | `purpose`: `revenue_allocation_type`; `options`: `Normal`, `Exclude`, `Software`; `option_meanings`: `exclude:discounted_amount_is_revenue`; `requires_one_of`: `vsoe_with_eitf_suiteapp`, `advanced_revenue_management` (when `software_option`); `option_meanings`: `normal:eitf_08_01_allocation`, `software:eitf_and_sop_97_2_or_fair_value_list`; `effect`: `software_reallocates_by_residual_when_estimates` |
| `costforpricing` | currency2 | Cost for Pricing | no | no | marked_new |  |
| `createddate` | datetime | Date Created | no | no |  |  |
| `externalid` | text | ExternalId | no | no |  |  |
| `isgcocompliant` | checkbox | Allow Google Checkout Purchase | no | yes |  |  |
| `itemoptions` | select | Item Options | no | no |  |  |
| `lastmodifieddate` | datetime | Last Modified | no | no |  |  |
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

## Sublist `presentationitem` (Related Items)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `presentationitem` | text | Related Item | no | marked_new |  |

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

> [!SOURCE-ERROR] `item_record_help_describes_label_printing` (on `itemtype`)
<!-- source-error: record:downloaditem | itemtype | item_record_help_describes_label_printing -->
