# Website (`website`)
<!-- id: record:website -->

| Fact | Value |
|---|---|
| Internal ID | `website` |
| Category | website |
| Client scripts | no |
| Server scripts | yes |
| Searchable | yes |
| Tags | `custom_fields_supported`, `scripting_full`, `ue_before_load_unsupported` |

## Record facts

- `script_types`: `server`
- `script_functions`: `create`, `copy`, `update`, `delete`, `search`
- `user_event_not_supported`: `beforeLoad`
- `sitetype_websitescope`: `ADVANCED/SUITE_COMMERCE`, `ADVANCED/SUITE_COMMERCE_ADVANCED`, `ADVANCED/SUITE_COMMERCE_IN_STORE`, `ADVANCED/SUITE_COMMERCE_MY_ACCOUNT`, `STANDARD/site_builder_scopes`
- `read_only_fields`: `websitescope` (when `advanced_site`)
- `ui_location`: `commerce_websites_website_list`
- `applies_to`: `site_builder`, `commerce_web_stores`, `scis`
- `caveat`: `set_dropdowns_with_internal_value_ids_not_ui_labels`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `analyticsclickattributes` | textarea | Analytics Click Attributes | no | no |  | `purpose`: `add_to_cart_button_attributes`; `value_kind`: `html_event_attributes`; `use_when`: `analytics_before_cart` |
| `analyticssubmitattributes` | textarea | Analytics Submit Attributes | no | no |  | `purpose`: `checkout_button_attributes`; `value_kind`: `html_event_attributes`; `use_when`: `analytics_at_checkout` |
| `autodownloadmedia` | checkbox | Automatically download published site media items | no | no |  | `purpose`: `auto_download_media` |
| `cartdisplayorder` | select | Display Order of Cart Items | no | no |  | `purpose`: `cart_display_order`; `options`: `Most Recently Added First`, `Most Recently Added Last`; `allowed_values`: `RECENT_FIRST`, `RECENT_LAST`; `applies_to`: `site_builder` |
| `cartupsellitems` | select | Items to Upsell in Cart | no | no |  | `purpose`: `cart_upsell_items`; `options`: `show_only_related_items`, `show_only_upsell_items`, `show_related_items_first_and_upsell_items_next`, `show_upsell_items_first_and_related_items_next`; `allowed_values`: `RELATED_FIRST_UPSELL_NEXT`, `UPSELL_FIRST_RELATED_NEXT`, `ONLY_RELATED_ITEMS`, `ONLY_UPSELL_ITEMS` |
| `categorylistlayout` | select | Default Category List Layout | no | no |  | `purpose`: `default_category_list_layout`; `effect`: `overrides_theme_default`; `can`: `override_per_category_or_tab` |
| `ccformtemplate` | select | Credit Card Form | no | no |  | `purpose`: `card_payment_form`; `applies_when`: `sales_order_type_per_customer` |
| `ccrequireauth` | checkbox | Require Authorization for Credit Card Transactions | no | no |  | `purpose`: `require_card_authorization`; `applies_to`: `web_site_sales_orders` |
| `checkoutdomain` | select | Checkout URL | no | no |  | `purpose`: `checkout_domain`; `allowed_values_listed_in`: `configured_checkout_subdomains` |
| `colorset` | select | Web Site Color Theme | no | no |  | `purpose`: `color_theme` |
| `confpagetrackinghtml` | textarea | Order Tracking Script HTML | no | no |  | `purpose`: `confirmation_page_tracking_html`; `use_when`: `tracking_orders_per_day` |
| `cookiepolicy` | select | Cookie Policy | no | no |  | `purpose`: `cookie_policy_file`; `displayed_on`: `cookie_banner_link` |
| `createcustomersascompanies` | checkbox | Create Customers as Companies | no | no |  | `purpose`: `register_customers_as_companies`; `requires`: `requirecompanyfield`; `false_means`: `customers_created_as_individuals` |
| `custromeregistrationtype` | select | Customer registration is | no | no |  | `purpose`: `customer_registration_type`; `options`: `disabled`, `existing customers only`, `optional (anonymous)`, `required`; `requires_feature`: `Advanced Site Customization` (when `existing_customers_only`) [`real_name`]; `effect`: `record_created_even_without_checkout` (when `required`) |
| `defaultcustomercategory` | select | Default Customer Category | no | no |  | `purpose`: `default_customer_category`; `effect`: `assigned_on_registration_or_purchase`; `can`: `edit_on_customer_record`; `allowed_values`: `CORPORATE`, `INDIVIDUAL`, `EMPLOYEE` |
| `defaulthostingroot` | select | Default Hosting Root | no | no |  | `purpose`: `hosting_root_folder` |
| `defaultshippingcountry` | select | Default Shipping Country for Checkout | no | no |  | `purpose`: `default_shipping_country`; `ui_effect`: `fills_checkout_country` |
| `defaultshippingmethod` | select | Default Web Site Shipping Method | no | no |  | `purpose`: `default_shipping_method`; `constraint`: `shipping_item_displayed_in_web_site`; `allowed_values_listed_in`: `shipping_item_records` |
| `descriptionfontsize` | posinteger | Description Font Size | yes | no |  | `purpose`: `description_font_size` |
| `displaycompanyfield` | checkbox | Display Company Field on Registration Page | no | no |  | `purpose`: `display_company_field_at_checkout` |
| `displayname` | text | Display Name | yes | no |  | `purpose`: `site_display_name`; `displayed_on`: `web_store_heading`, `browser_title` |
| `displayunsubscribe` | checkbox | Display Unsubscribe to Email Checkbox on Registration Page | no | no |  | `purpose`: `offer_unsubscribe_at_registration`; `configured_by`: `marketing_preferences` |
| `doctypehtml` | textarea | Document Type | no | no |  | `purpose`: `doctype_declaration`; `value_examples`: `html5_doctype` |
| `enablegaintegration` | checkbox | Enable Google Analytics Integration | no | no |  | `purpose`: `google_analytics_integration`; `effect`: `passes_tracking_cookies_to_checkout`; `use_when`: `cross_domain_analytics` |
| `font` | select | Web Site Font | no | no |  | `purpose`: `site_font` |
| `giftcerteaffectsminamount` | checkbox | Gift Certificate Affects Minimum Order Amount | no | no |  | `purpose`: `gift_certificate_counts_toward_minimum`; `true_means`: `order_below_minimum_rejected`; `false_means`: `credits_excluded_from_minimum_total` |
| `hidepaymentpagewhennobalance` | checkbox | Hide payment page if order total equals zero | no | no |  | `purpose`: `hide_payment_page_when_zero`; `use_when`: `free_promotional_items` |
| `igniteedition` | select | Type | no | no |  | `purpose`: `website_type`; `allowed_values`: `SuiteCommerce Advanced`, `Site Builder` |
| `includeuncategorizeditems` | checkbox | Show Uncategorized Items | no | no |  | `purpose`: `include_uncategorized_items_in_search` |
| `includevatwithprices` | checkbox | Prices Include Tax | no | no |  | `purpose`: `include_vat_in_prices`; `applies_to`: `uk_accounts` |
| `internalname` | text | Internal Name | yes | no |  | `purpose`: `internal_site_name`; `not_displayed_on`: `web_store` |
| `invoiceformtemplate` | select | Invoice Form | no | no |  | `purpose`: `invoice_form`; `applies_when`: `sales_order_type_per_customer` |
| `isscriptablecartandcheckout` | checkbox | Scriptable Cart and Checkout | no | no |  | `purpose`: `scriptable_cart_and_checkout`; `effect`: `suitescript_runs_on_checkout_sales_order`; `requires`: `script_on_customized_online_order_form`; `requires`: `scripts_on_invoice_and_cash_sale_forms` (when `customer_terms`) |
| `iswebstoreoffline` | checkbox | Take Web Site Offline for Maintenance | no | no |  | `purpose`: `take_site_offline` |
| `iswsdk` | checkbox | WSDK | no | no |  | `purpose`: `external_catalog_site`; `use_when`: `wsdk_live_pricing_and_availability` |
| `itemlistlayout` | select | Default Item List Layout | no | no |  | `purpose`: `default_item_list_layout`; `effect`: `overrides_theme_default`; `can`: `override_per_category_or_tab` |
| `itemtemplateaccountingtitem` | select | Default Item Drilldown Template | no | no |  | `purpose`: `default_item_template`; `can`: `override_per_item`; `effect`: `overrides_theme_default` |
| `itemtemplateinformationitem` | select | Default Info Item Drilldown Template | no | no |  | `purpose`: `default_information_item_template`; `can`: `override_per_item`; `effect`: `overrides_theme_default` |
| `legacyimageattributetags` | checkbox | Legacy Tag Support for Images | no | no |  | `purpose`: `legacy_image_attribute_tags`; `true_means`: `tags_return_file_name`; `false_means`: `tags_return_image_url`; `recommendation`: `clear_box` |
| `noactivatetabonhostedpages` | checkbox | Don't Activate Hosted Page Tabs | no | no |  | `purpose`: `hosted_pages_not_tab_associated`; `effect`: `hosted_page_inherits_no_tab_settings` |
| `onlinepricelevel` | select | Price Level | yes | no |  | `purpose`: `site_price_level`; `can`: `different_price_level_per_site` (when `multiple_web_sites`) |
| `outofstockitems` | select | Web Store Out Of Stock Items | no | no |  | `purpose`: `out_of_stock_display`; `options`: `allow_back_orders_show_message`, `remove_from_store`, `allow_back_orders_no_message`, `disallow_back_orders_show_message` (when `site_builder`); `caveat`: `items_hidden_until_distributed` (when `multi_location_inventory`) |
| `outputlinebreaksasbr` | checkbox | Output Line Breaks as <br> | no | no |  | `purpose`: `line_breaks_as_br` |
| `pagealign` | select | Page Alignment | no | no |  | `purpose`: `page_alignment`; `allowed_values`: `LEFT`, `RIGHT`, `CENTER`; `applies_to`: `site_builder` |
| `pagewidth` | text | Page Width | no | no |  | `purpose`: `fixed_page_width`; `value_unit`: `pixels` |
| `pagezoom` | select | Zoom | no | no |  | `purpose`: `page_zoom` |
| `passpromocodetocheckout` | checkbox | Pass URL Promotion Code to Checkout | no | no |  | `purpose`: `apply_promo_code_from_link`; `effect`: `discount_shown_on_last_checkout_page`; `format`: `promocode_url_parameter`; `doc_inconsistency`: `unresolved_navigation_placeholders` |
| `relateditemsdescription` | text | Related Items | no | no |  | `purpose`: `related_items_heading`; `displayed_on`: `item_details_page` |
| `relateditemslayout` | select | Default Related Items Layout | no | no |  | `purpose`: `related_items_layout`; `effect`: `overrides_theme_default`; `can`: `override_per_category_or_tab` |
| `requestshippingaddressfirst` | checkbox | Ask For Shipping Address First | no | no |  | `purpose`: `shipping_address_first` |
| `requirecompanyfield` | checkbox | Mandatory | no | no |  | `purpose`: `require_company_field`; `requires`: `displaycompanyfield`; `required_when`: `creating_company_customers` |
| `requireloginforpricing` | checkbox | Require Login For Pricing | no | no |  | `purpose`: `require_login_for_pricing`; `ui_effect`: `prices_and_add_to_cart_hidden`; `risk`: `content_still_reachable_via_api_urls` |
| `requireshippinginformation` | checkbox | Shipping Information is Required | no | no |  | `purpose`: `require_shipping_information` |
| `requiretermsandconditions` | checkbox | Require Terms and Conditions | no | no |  | `purpose`: `require_terms_and_conditions`; `ui_effect`: `i_agree_checkbox_at_checkout` |
| `salesordertype` | select | Sales Order Type | no | no |  | `purpose`: `web_sales_order_type`; `options`: `cash_sale`, `invoice`, `Per Customer Basis`; `required_when`: `credit_card_only_store` [`cash_sale`]; `allowed_values`: `PER_CUSTOMER` |
| `savecreditinfo` | checkbox | Save Credit Card Info by Default | no | no |  | `purpose`: `save_card_by_default`; `risk`: `company_contacts_share_saved_cards` |
| `scripttemplatecreditcard` | select | Scripting Template (Credit Card) | no | no |  | `purpose`: `checkout_script_form_cash_sale`; `requires`: `script_attached_to_customized_sales_order_form`; `doc_inconsistency`: `note_names_same_cash_sale_form_twice` |
| `scripttemplateinvoice` | select | Scripting Template (Invoice) | no | no |  | `purpose`: `checkout_script_form_invoice`; `requires`: `script_attached_to_customized_sales_order_form`; `doc_inconsistency`: `note_names_same_cash_sale_form_twice` |
| `searchcategorylistlayout` | select | Category List Layout | no | no |  | `purpose`: `category_search_layout`; `value_kind`: `search_results_layout` |
| `searchforcategories` | checkbox | Include Categories | no | no |  | `purpose`: `show_categories_in_search`; `false_means`: `items_only` |
| `searchforinformationitems` | checkbox | Include Information Items | no | no |  | `purpose`: `show_information_items_in_search` |
| `searchformitemlistlayout` | select | Item List Layout | no | no |  | `purpose`: `search_form_item_layout`; `value_kind`: `search_results_layout` |
| `searchitemlistlayout` | select | Item List Layout | no | no |  | `purpose`: `item_search_layout`; `value_kind`: `search_results_layout` |
| `searchlinkform` | select | Search Link Form | no | no |  | `purpose`: `search_link_form` |
| `searchportletdisplay` | radio | Global Search | no | no |  | `purpose`: `search_portlet_display`; `options`: `Global Search`, `Search Form` |
| `searchportletform` | select | Search Portlet Form | no | no |  | `purpose`: `search_portlet_form`; `caveat`: `results_sortable_by_price_or_name` |
| `semailaddrforerror` | emails | Email To | no | no |  | `purpose`: `checkout_error_email` |
| `shipstoallcountries` | checkbox | Web Store Ships to all countries | no | no |  | `purpose`: `ships_to_all_countries`; `false_means`: `limited_to_shipstocountries` |
| `shipstocountries` | select | Countries Web Store Ships To | no | no |  | `purpose`: `allowed_ship_countries`; `editable_only_when`: `shipstoallcountries_cleared` |
| `showcartsummaryportlet` | checkbox | Show Cart Summary | no | no |  | `purpose`: `show_cart_summary_portlet` |
| `showcookieconsentbanner` | checkbox | Show Cookie Consent Banner | no | no |  | `purpose`: `show_cookie_consent_banner` |
| `showcurrencyportlet` | checkbox | Show Web Site Currency | no | no |  | `purpose`: `show_currency_portlet`; `effect`: `prices_shown_in_selected_currency` |
| `showextendedcart` | checkbox | Show Extended Cart | no | no |  | `purpose`: `show_extended_cart`; `applies_to`: `coupon_code`, `gift_certificate`, `shipping_information`; `requires`: `show_shipping_estimator` (when `shipping_fields`) |
| `shownavigationportlet` | checkbox | Show Navigation Portlet | no | no |  | `purpose`: `show_navigation_portlet` |
| `showpofieldonpayment` | checkbox | Display Purchase Order Field on Payment Info Page | no | no |  | `purpose`: `show_po_field_at_payment`; `effect`: `po_number_copied_to_web_sales_order` |
| `showquantitypricinginlists` | checkbox | Show Quantity Pricing in Lists | no | no |  | `purpose`: `quantity_pricing_in_lists`; `false_means`: `single_price_in_lists_table_on_detail` |
| `showsavecreditinfo` | checkbox | Display 'Save My Credit Card Info' Field | no | no |  | `purpose`: `offer_save_card`; `effect`: `card_stored_on_customer_record`; `risk`: `company_contacts_share_saved_cards` |
| `showsearchportlet` | checkbox | Show Search | no | no |  | `purpose`: `show_search_portlet` |
| `showshippingestimator` | checkbox | Show Shipping Estimator in Cart | no | no |  | `purpose`: `show_shipping_estimator`; `effect`: `lowest_estimate_shown` |
| `showtellafriendlink` | checkbox | Show "Tell a Friend" Link | no | no |  | `purpose`: `tell_a_friend_link`; `doc_inconsistency`: `unresolved_navigation_placeholders` |
| `siteloginrequired` | checkbox | Password-Protect Entire Site | no | no |  | `purpose`: `site_login_required`; `requires`: `customer_access_granted`; `risk`: `content_still_reachable_via_api_urls` |
| `siteusescarttags` | checkbox | Site Uses Cart Summary Tags | no | no |  | `purpose`: `site_uses_cart_tags` |
| `sstoreemailaddrforcopy` | emails | Email To | no | no |  | `purpose`: `bcc_confirmation_addresses`; `format`: `comma_separated`; `applies_when`: `bcc_confirmation_email` |
| `sstoreemailaddrforgiftcopy` | emails | Email To | no | no |  | `purpose`: `bcc_order_received_addresses`; `format`: `comma_separated` |
| `storeemail` | email | Default Web Store Email From Address | no | no |  | `purpose`: `store_from_email`; `caveat`: `used_only_when_template_from_blank` |
| `termsandconditionshtml` | textarea | Terms and Conditions HTML | no | no |  | `purpose`: `terms_and_conditions_html`; `displayed_on`: `popup_before_agreement` |
| `textgroup` | select | Text Group | no | no |  | `purpose`: `web_site_text_group` |
| `titlefontsize` | posinteger | Title Font Size | yes | no |  | `purpose`: `title_font_size` |
| `upselldescription` | text | Upsell | no | no |  | `purpose`: `upsell_heading`; `value_examples`: `customers_who_bought_this_item_also_bought` |
| `upsellitems` | select | Items to Upsell | no | no |  | `purpose`: `upsell_items_type`; `options`: `show_only_related_items`, `show_only_upsell_items`, `show_related_items_first_and_upsell_items_next`, `show_upsell_items_first_and_related_items_next`; `allowed_values`: `RELATED_FIRST_UPSELL_NEXT`, `UPSELL_FIRST_RELATED_NEXT`, `ONLY_RELATED_ITEMS`, `ONLY_UPSELL_ITEMS` |
| `upsellitemslayout` | select | Default Upsell Items Layout | no | no |  | `purpose`: `upsell_layout`; `effect`: `overrides_theme_default` |
| `upsellminimumcorrelation` | percent | Minimum Correlation | no | no |  | `purpose`: `upsell_minimum_correlation_percent` |
| `upsellminimumcount` | integer | Minimum Count | no | no |  | `purpose`: `upsell_minimum_count` |
| `upsellminimumlift` | percent | Minimum Lift | no | no |  | `purpose`: `upsell_minimum_lift` |
| `urlformat` | select | Format | no | no |  | `purpose`: `url_format`; `options`: `parent_tab_and_category_in_url`, `item_name_only`; `effect`: `spaces_become_hyphens` |
| `useurlfileextension` | checkbox | Use File Extension | no | no |  | `purpose`: `use_url_file_extension`; `effect`: `file_extension_and_trailing_slash_in_urls` |
| `websitehomepage` | select | Web Site Home Page | yes | no |  | `purpose`: `site_home_page`; `requires`: `websitehomepagetype` |
| `websitehomepagetype` | select | Web Site Home Page Type | yes | no |  | `purpose`: `home_page_type`; `options`: `Web Site Tab`, `Web Page` |
| `websitelogo` | select | Web Site Logo | no | no |  | `purpose`: `store_logo`; `allowed_values`: `JPEG`, `GIF`; `recommendation`: `200_by_100_pixels`; `ui_effect`: `new_option_creates_record` |
| `websitelogoalign` | select | Web Site Logo Alignment | no | no |  | `purpose`: `logo_alignment`; `options`: `left`, `right`, `center`; `allowed_values`: `LEFT`, `RIGHT`, `CENTER`; `applies_to`: `site_builder` |
| `websitelogolinkurl` | text | Logo Link URL | no | no |  | `purpose`: `logo_link_url`; `default_behavior`: `no_link` (when `blank`) |
| `websitescope` | select | Web Site Scope | no | no |  | `purpose`: `website_scope`; `value_source`: `system_displayed`; `allowed_values`: `Site Builder`, `SuiteCommerce`, `SuiteCommerce Advanced`, `SuiteCommerce My Account`, `SuiteCommerce InStore`; `allowed_values`: `FULL_WEB_STORE`, `INFO_CATALOG_PRICING`, `INFO_CATALOG`, `INFO_ONLY`; `editable_only_when`: `site_builder_site` |
| `websitetheme` | select | Web Site Theme | no | no |  | `purpose`: `site_theme`; `can`: `override_theme_settings_here` |
| `webstoreadditembehavior` | select | After shopper adds an item to the cart | no | no |  | `purpose`: `after_add_to_cart_behavior`; `options`: `stay_on_page`, `go_to_cart` |
| `webstoremetatags` | textarea | Addition to <head> | no | no |  | `purpose`: `store_head_tags`; `accepts_forms`: `analytics_script`, `meta_tags`; `recommendation`: `category_and_tab_meta_tags_better_for_seo` |
| `wsdkcancelcarturl` | text | Shopping cart page Continue Shopping link URL | no | no |  | `purpose`: `continue_shopping_url`; `value_kind`: `return_url_into_external_site`; `applies_when`: `external_catalog_wsdk_site` |
| `wsdkcancelcheckouturl` | text | Checkout page Cancel link URL | no | no |  | `purpose`: `cancel_checkout_url`; `value_kind`: `return_url_into_external_site`; `applies_when`: `external_catalog_wsdk_site` |
| `wsdkcancelloginurl` | text | Login/registration page Cancel link URL | no | no |  | `purpose`: `cancel_login_url`; `value_kind`: `return_url_into_external_site`; `applies_when`: `external_catalog_wsdk_site` |
| `wsdkcompletecheckouturl` | text | URL to go to after checkout is complete | no | no |  | `purpose`: `complete_checkout_url`; `value_kind`: `return_url_into_external_site`; `applies_when`: `external_catalog_wsdk_site` |
| `wsdkcompleteloginurl` | text | URL to go to after login/registration is complete | no | no |  | `purpose`: `complete_login_url`; `value_kind`: `return_url_into_external_site`; `applies_when`: `external_catalog_wsdk_site` |
| `sitetabalignment` | select | Site Tab Alignment | no | no |  | `allowed_values`: `LEFT`, `RIGHT`, `CENTER`; `applies_to`: `site_builder` |
| `emailfooter` | textarea | Footer Text | no | no |  |  |
| `emailheader` | textarea | Header Text | no | no |  |  |
| `externalid` | text | ExternalId | no | no |  |  |
| `gconotificationurl` | text | Google Notification URL | no | no |  |  |
| `legacycategorydescriptions` | checkbox | Legacy Category Descriptions | no | no |  |  |
| `legacydenselistsupport` | checkbox | Legacy Dense List Layout | no | no |  |  |
| `legacynavigationlinks` | checkbox | Legacy Navigation Style | no | no |  |  |
| `legacywelcomepage` | checkbox | Legacy Welcome Page Style | no | no |  |  |
| `nameorig` | text | Name | no | no |  |  |
| `searchresultforinvalidurls` | checkbox | Return Invalid URLs as Search Results | no | no |  |  |
| `showbillingaddress` | checkbox | Display Billing Address | no | no |  |  |
| `showordertrackinglink` | checkbox | Display Order Tracking Link | no | no |  |  |
| `siteportletstyle` | select | Site Portlet Style | no | no |  |  |
| `sitetabstyle` | select | Site Tab Style | no | no |  |  |
| `siteusesdropshadows` | checkbox | Site Uses Drop Shadows | no | no |  |  |
| `sstoreemailaddrforgiftconfcopy` | emails | Email To | no | no |  |  |

## Sublist `cartcolumn` (Cart)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `show` | checkbox | Show | no |  |  |
| `width` | float | Width (%) | no |  |  |

## Sublist `shoppingdomain` (Domain Names)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `domain` | text | Domain Name | yes |  |  |
| `hostingroot` | select | HTML Hosting Root | yes |  |  |
| `isprimary` | checkbox | Primary Web Site URL | no |  |  |
| `notfoundpage` | select | Not-Found Page | no |  |  |
| `touchpoints` | namevaluelist | Touch Points | no |  |  |

## Sublist `sitecurrency` (Currency)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `currency` | text |  | no |  |  |
| `isdefault` | radio | Default | no |  |  |
| `isonline` | checkbox | Online | no |  |  |
| `minimumorderamount` | currency | Minimum Order Amount | no |  |  |

## Sublist `sitelanguage` (Languages)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `isdefault` | radio | Default | no |  |  |
| `isonline` | checkbox | Online | no |  |  |
| `locale` | text |  | no |  |  |

## Sublist `siteregion` (Subsidiaries)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `displayname` | text | Display Name | no |  |  |
| `isdefault` | radio | Default | no |  |  |
| `isonline` | checkbox | Online | no |  |  |
| `paypalaccount` | select | PayPal Account | no |  |  |
| `subsidiary` | text |  | no |  |  |

## Sublist `storetab` (Tabs)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `tabdescription` | text | Tab Description | no |  |  |
| `tabname` | text | Tab Name | no |  |  |
| `tabtype` | text | Tab Type | no |  |  |

## Tabs

| Tab ID | Label |
|---|---|
| `advanced` | Advanced |
| `analytics` | Analytics |
| `appearance` | Appearance |
| `cart` | Cart |
| `email` | Email |
| `external` | External |
| `general` | Setup |
| `legacy` | Legacy |
| `search` | Search |
| `shopping` | Shopping |
| `shoppingdomainstab` | Domains |
| `upsell` | Upsell |

## Suspected source errors

> [!SOURCE-ERROR] `unresolved_navigation_placeholders` (on `passpromocodetocheckout`)
<!-- source-error: record:website | passpromocodetocheckout | unresolved_navigation_placeholders -->

> [!SOURCE-ERROR] `note_names_same_cash_sale_form_twice` (on `scripttemplatecreditcard`)
<!-- source-error: record:website | scripttemplatecreditcard | note_names_same_cash_sale_form_twice -->

> [!SOURCE-ERROR] `note_names_same_cash_sale_form_twice` (on `scripttemplateinvoice`)
<!-- source-error: record:website | scripttemplateinvoice | note_names_same_cash_sale_form_twice -->

> [!SOURCE-ERROR] `unresolved_navigation_placeholders` (on `showtellafriendlink`)
<!-- source-error: record:website | showtellafriendlink | unresolved_navigation_placeholders -->
