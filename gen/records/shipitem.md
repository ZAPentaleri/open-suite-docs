# Shipping Item (`shipitem`)
<!-- id: record:shipitem -->

| Fact | Value |
|---|---|
| Internal ID | `shipitem` |
| Category | item |
| Client scripts | no |
| Server scripts | yes |
| Tags | `custom_fields_supported`, `no_copy`, `no_create` |

## Record facts

- `script_types`: `server`
- `script_functions`: `update`, `delete`, `search`
- `not_supported`: `create`, `copy`
- `not_scriptable`: `handling_table`, `shipping_table`, `free_shipping_items_tab`
- `purpose`: `carrier_delivery_method_with_rates_and_rules`
- `workaround`: `create_shipping_items_in_ui`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `account` | select | Account (Shipping) | yes | no |  | `purpose`: `shipping_income_account` |
| `accounthandling` | select | Account (Handling) | no | no |  | `purpose`: `handling_income_account` |
| `costbasis` | radio | FedEx Real-Time Rate | no | no |  | `purpose`: `shipping_cost_basis`; `options`: `fedex_ups_real_time_rate`, `flat_rate`, `by_weight`, `by_item`, `percent_of_total`, `shipping_table`; `requires`: `fedex_ups_accounts_registered` (when `real_time_rate`); `caveat`: `table_row_applies_until_next_row_value` |
| `countries` | select | Countries | no | no |  | `purpose`: `ship_to_countries`; `default_behavior`: `all_countries` (when `blank`); `comparable_to`: `excludecountries` |
| `description` | textarea | Description | no | no |  | `purpose`: `description` |
| `displayname` | text | Display Name/Code | yes | no |  | `purpose`: `display_name`; `displayed_on`: `printed_invoices`, `web_store`, `customer_center` |
| `doiftotal` | checkbox | Available If Order Total Is | no | no |  | `purpose`: `limit_online_offer_by_order_total` |
| `doifweight` | checkbox | Available If Order Weight Is | no | no |  | `purpose`: `limit_online_offer_by_order_weight` |
| `excludecountries` | checkbox | Exclude | no | no |  | `purpose`: `exclude_selected_countries` |
| `excludesites` | checkbox | Exclude | no | no |  | `purpose`: `exclude_selected_sites` |
| `fedexdiscountrate` | float | Discount Rate | no | no |  | `purpose`: `fedex_discount_multiplier`; `value_examples`: `0.75_for_25_percent_off`; `use_instead`: `promotion_or_zero_flat_rate` (when `free_shipping`) |
| `fedexonerate` | checkbox | FedEx One Rate® | no | no |  | `purpose`: `fedex_one_rate`; `applies_when`: `shipment_under_50_lb` |
| `handlingcost` | radio | Handling--No Handling Charge | no | no |  | `purpose`: `handling_cost_basis`; `options`: `No Handling Charge`, `flat_rate`, `by_weight`, `Per item`, `Percent of total`, `Handling Table` |
| `handlingperitemamount` | currency | Default Charge | no | no |  | `purpose`: `default_handling_per_item` |
| `hasmaximumshippingcost` | checkbox | Maximum shipping amount | no | no |  | `purpose`: `cap_shipping_cost` |
| `hasminimumshippingcost` | checkbox | Minimum shipping amount | no | no |  | `purpose`: `minimum_shipping_cost` |
| `invt_dispname` | text | Display Name | no | no |  | `purpose`: `translated_display_name` |
| `isfreeifordertotalisover` | checkbox | Free if total order is over | no | no |  | `purpose`: `free_shipping_over_total` |
| `ishandlingbyweightbracketed` | checkbox | In Whole Increments | no | no |  | `purpose`: `whole_increments_only`; `help_text_shared_with`: `shipitem.isshippingbyweightbracketed` |
| `isinactive` | checkbox | Inactive | no | no |  | `purpose`: `inactivate_shipping_item`; `ui_effect`: `hidden_from_list_unless_show_all` |
| `isonline` | checkbox | Display in Web Site | no | no |  | `purpose`: `available_online`; `constraint`: `one_online_item_needed_to_charge_shipping`; `configured_by`: `charge_for_shipping_setting` |
| `isshippingbyweightbracketed` | checkbox | In Whole Increments | no | no |  | `purpose`: `whole_increments_only`; `help_text_shared_with`: `shipitem.ishandlingbyweightbracketed` |
| `istaxable` | checkbox | Charge Tax on this shipping portion of item | no | no |  | `purpose`: `taxable_shipping` |
| `itemid` | text | Ship Name | yes | no |  | `purpose`: `shipping_item_name`; `displayed_on`: `transactions`, `lists`, `internal_documents` |
| `labelpluginselect` | select | Shipping Partner | no |  |  | `purpose`: `label_partner`; `applies_to`: `australia_post` |
| `labelregselect` | select | Registration | no |  |  | `purpose`: `label_registration`; `applies_to`: `australia_post` |
| `labelservicegroupselect` | select | Service Group | no |  |  | `purpose`: `label_service_group` |
| `labelserviceselect` | select | Service | no |  |  | `purpose`: `label_service` |
| `labeltype` | radio | Integrated Shipping Label | no |  |  | `purpose`: `label_type`; `options`: `integrated_labels`, `shipping_partner_labels` |
| `needsallfreeshippingitems` | checkbox | All items must be purchased | no | no |  | `purpose`: `require_all_free_shipping_items`; `false_means`: `any_listed_item_qualifies` |
| `omitpackaging` | checkbox | Omit packaging | no | no |  | `purpose`: `omit_packaging`; `effect`: `single_package_no_auto_packaging` |
| `ratingpluginselect` | select | Shipping Partner | no |  |  | `purpose`: `rating_partner`; `applies_to`: `australia_post` |
| `ratingregselect` | select | Registration | no |  |  | `purpose`: `rating_registration` |
| `ratingservicegroupselect` | select | Service Group | no |  |  | `purpose`: `rating_service_group`; `applies_to`: `australia_post` |
| `ratingserviceselect` | select | Service | no |  |  | `purpose`: `rating_service`; `applies_to`: `australia_post` |
| `returnlabelserviceselect` | select | Return Service | no |  |  | `purpose`: `return_label_service` |
| `returnsintegrated` | checkbox | Return Label Integration | no | no |  | `purpose`: `integrated_return_labels` |
| `shipitemcurrency` | text | Currency | no | no |  | `purpose`: `shipping_item_currency`; `value_source`: `subsidiary_currency` |
| `shipperintegrated` | checkbox | Shipping Label Integration | no | no |  | `purpose`: `carrier_integrated`; `effect`: `method_used_and_charged_to_integrated_account` |
| `shippingperitemamount` | currency | Default Charge | no | no |  | `purpose`: `default_shipping_per_item` |
| `site` | select | Limit To Sites | no | no |  | `purpose`: `available_sites`; `default_behavior`: `all_sites` (when `blank`) |
| `states` | select | States | no | no |  | `purpose`: `ship_to_states`; `default_behavior`: `all_states` (when `blank`) |
| `subsidiary` | select | Subsidiaries | no | no |  | `purpose`: `subsidiary`; `constraint`: `single_subsidiary_per_shipping_item` |
| `taxschedule` | select | Charge Tax on this shipping portion of item | yes | no |  | `purpose`: `shipping_tax_schedule` |
| `taxschedulehandling` | select | Charge Tax on this handling portion of item | yes | no |  | `purpose`: `handling_tax_schedule` |
| `upsdiscountrate` | float | Discount Rate | no | no |  | `purpose`: `ups_discount_multiplier`; `value_examples`: `0.75_for_25_percent_off`; `doc_inconsistency`: `ups_help_says_usps` |
| `accchange` | checkbox |  | no |  |  |  |
| `doifarrangement` | text | Web Site Rules | no | no |  |  |
| `doiftotalamt` | currency |  | no |  |  |  |
| `doiftotaloperator` | select |  | no |  |  |  |
| `doifweightamt` | currency |  | no |  |  |  |
| `doifweightoperator` | select |  | no |  |  |  |
| `doifweightunit` | select |  | no |  |  |  |
| `edition` | text |  | no |  |  |  |
| `externalid` | text | ExternalId | no | no |  |  |
| `fedexservicecode` | select |  | no |  |  |  |
| `freeifordertotalisoveramount` | currency |  | no |  |  |  |
| `handlingaspercentageoftotal` | percent |  | no |  |  |  |
| `handlingbyweightamount` | currency |  | no |  |  |  |
| `handlingbyweightperquantity` | float | per | no | no |  |  |
| `handlingbyweightperunit` | select |  | no |  |  |  |
| `handlingflatrateamount` | currency |  | no |  |  |  |
| `handlingtablechargeby` | select |  | no |  |  |  |
| `handlingtableuom` | select |  | no |  |  |  |
| `handlingtaxcode` | select | Charge Tax on this handling portion of item | no |  |  |  |
| `id` | integer | Internal ID | no |  |  |  |
| `integratedlabelsarrangement` | text | Integrated Labels | no | no |  |  |
| `integrationservicecode` | select |  | no |  |  |  |
| `ishandlingtaxable` | checkbox | Charge Tax on this handling portion of item | no |  |  |  |
| `itemtype` | text | stype | no | no |  |  |
| `labelplugin` | text |  | no |  |  |  |
| `labelreg` | text |  | no |  |  |  |
| `labelservice` | text |  | no |  |  |  |
| `labelservicegroup` | text |  | no |  |  |  |
| `maximumshippingcost` | currency |  | no |  |  |  |
| `minimumshippingcost` | currency |  | no |  |  |  |
| `pluginlabelsarrangement` | text | Shipping Partner Labels | no |  |  |  |
| `pluginratearrangement` | text | Shipping Partner Rate | no |  |  |  |
| `ratingplugin` | text |  | no |  |  |  |
| `ratingreg` | text |  | no |  |  |  |
| `ratingservice` | text |  | no |  |  |  |
| `ratingservicegroup` | text |  | no |  |  |  |
| `restrictionarrangement` | text | Web Store Shipping Restrictions | no | no |  |  |
| `returnlabelreg` | text |  | no |  |  |  |
| `returnlabelregselect` | select | Registration | no |  |  |  |
| `returnlabelservice` | text |  | no |  |  |  |
| `returnservicecode` | select |  | no |  |  |  |
| `shippingaspercentageoftotal` | percent |  | no |  |  |  |
| `shippingbyweightamount` | currency |  | no |  |  |  |
| `shippingbyweightperquantity` | float | per | no | no |  |  |
| `shippingbyweightperunit` | select | Weight Unit | no |  |  |  |
| `shippingcarrier` | text |  | no |  |  |  |
| `shippingflatrateamount` | currency |  | no |  |  |  |
| `shippingtablechargeby` | select |  | no |  |  |  |
| `shippingtableuom` | select |  | no |  |  |  |
| `shippingtaxcode` | select | Charge Tax on this shipping portion of item | no |  |  |  |
| `tabtext` | text |  | no |  |  |  |
| `upssavername` | text |  | no |  |  |  |
| `upsservicecode` | select |  | no |  |  |  |
| `uspsdiscountrate` | float | Discount Rate | no | no |  |  |
| `uspsservicecode` | select |  | no |  |  |  |

## Sublist `handlingtable`

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `handlingtablecharge` | currency | Charge | yes |  |  |
| `handlingtablerangevalue` | currency | Range Value | yes |  |  |
| `shipitem` | select |  | no |  |  |

## Sublist `items` (Items)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `item` | select | Item | yes |  |  |
| `nqty` | integer | Quantity | no |  |  |
| `shipitem` | select |  | no |  |  |

## Sublist `shipmethodrulerelations` (Shipping Items)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `item` | select | Shipping Item | no |  |  |
| `subsidiary` | select | Subsidiary | no |  |  |

## Sublist `shippingtable`

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `shipitem` | select |  | no |  |  |
| `shippingtablecharge` | currency | Charge | yes |  |  |
| `shippingtablerangevalue` | currency | Range Value | yes |  |  |

## Sublist `translations` (Translation)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `displayname` | text | Display Name | no |  |  |
| `language` | text | Language | no |  |  |
| `locale` | text |  | no |  |  |
| `shipitem` | select |  | no |  |  |

## Tabs

| Tab ID | Label |
|---|---|
| `freeshipping` | Free Shipping Items |
| `handling` | Handling Rate |
| `rules` | Shipping and Handling Rules |
| `shipping` | Shipping Rate |
| `shippinglabels` | Shipping Labels |
| `translation` | Translation |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `account` | select | Account |  |
| `accounthandling` | select | Account (Handling) |  |
| `carrier` | text | Carrier |  |
| `description` | text | Description |  |
| `displayname` | text | Dipslay Name |  |
| `excludedforcountries` | checkbox | Excluded For Countries |  |
| `excludedforsites` | checkbox | Excluded For Sites |  |
| `externalid` | select | External ID |  |
| `externalidstring` | text | External ID (Text) |  |
| `freeiftotaloveramount` | float | Free If Total Over Amount |  |
| `handlingflatrate` | float | Handling Flat Rate |  |
| `handlingfunctiontype` | select | Handling Rate Type |  |
| `handlingrateaspercentoftotal` | float | Handling Rate As Percent of Total |  |
| `handlingratebyitem` | float | Handling Rate By Item |  |
| `handlingratebyweight` | float | Handling Rate By Weight |  |
| `handlingratebyweightbracketed` | checkbox | Handling Rate By Weight Bracketed |  |
| `handlingratebyweightper` | float | Handling Rate By Weight Per |  |
| `handlingratebyweightunit` | select | Handling Rate By Weight Unit |  |
| `handlingratetabletype` | select | Handling Rate Table Type |  |
| `hasmaxshippingcost` | checkbox | Has Maximum Shipping Cost |  |
| `hasminshippingcost` | checkbox | Has Minimum Shipping Cost |  |
| `internalid` | select | Internal ID |  |
| `internalidnumber` | integer | Internal ID (Number) |  |
| `isfreeiftotalover` | checkbox | Is Free If Total Over |  |
| `isinactive` | checkbox | Inactive |  |
| `isonline` | checkbox | Display in Web Site |  |
| `isreturnsintegrated` | checkbox | Is Return Service Integrated |  |
| `isshipperintegrated` | checkbox | Is Integrated Shipping Item |  |
| `itemid` | text | Name |  |
| `labelpluginid` | select | Label Plugin Id |  |
| `labelregistration` | select | Label Registration |  |
| `labelservicegroupid` | select | Label Service Group Id |  |
| `labelserviceid` | select | Label Service Id |  |
| `limitedtocountries` | multiselect | Limited To Countries |  |
| `limitedtosites` | multiselect | Limited To Sites |  |
| `limitedtostates` | multiselect | Limited To States |  |
| `limitwebsitevisibilitybytotal` | checkbox | Web Site Visibility Limited by Total |  |
| `limitwebsitevisibilitybyweight` | checkbox | Web Site Visibility Limited by Weight |  |
| `maxshippingcost` | float | Maximum Shipping Cost |  |
| `minshippingcost` | float | Minimum Shipping Cost |  |
| `ratingpluginid` | select | Rating Plugin Id |  |
| `ratingregistration` | select | Rating Registration |  |
| `ratingservicegroupid` | select | Rating Service Group Id |  |
| `ratingserviceid` | select | Rating Service Id |  |
| `realtimerateservice` | select | Real-Time Rate Service |  |
| `returnlabelregistration` | select | Return Label Registration |  |
| `returnlabelserviceid` | select | Return Label Service Id |  |
| `returnshippingservice` | select | Return Shipping Service |  |
| `shippingdiscountrate` | float | Shipping Rate Discount |  |
| `shippingflatrate` | float | Shipping Flat Rate |  |
| `shippingfunctiontype` | select | Shipping Rate Type |  |
| `shippingrateaspercentoftotal` | float | Shipping Rate As Percent of Total |  |
| `shippingratebyitem` | float | Shipping Rate By Item |  |
| `shippingratebyweight` | float | Shipping Rate By Weight |  |
| `shippingratebyweightbracketed` | checkbox | Shipping Rate By Weight Bracketed |  |
| `shippingratebyweightper` | float | Shipping Rate By Weight Per |  |
| `shippingratebyweightunit` | select | Shipping Rate By Weight Unit |  |
| `shippingratetabletype` | select | Shipping Rate Table Type |  |
| `shippingservice` | select | Shipping Service |  |
| `subsidiary` | select | Subsidiary |  |
| `websitevisibilitybytotaltype` | select | Web Site Visibility By Total Type |  |
| `websitevisibilitybyweighttype` | select | Web Site Visibility By Weight Type |  |
| `websitevisibilitytotallimit` | float | Web Site Visibility By Total Limit |  |
| `websitevisibilityweightlimit` | float | Web Site Visibility By Weight Limit |  |
| `websitevisibilityweightunit` | select | Web Site Visibility By Weight Unit |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `account` | text | Account |  |
| `accounthandling` | text | Account (Handling) |  |
| `carrier` | text | Carrier |  |
| `description` | text | Description |  |
| `displayname` | text | Display Name |  |
| `excludedforcountries` | checkbox | Excluded For Countries |  |
| `excludedforsites` | checkbox | Excluded For Sites |  |
| `externalid` | select | External ID |  |
| `freeiftotaloveramount` | float | Free Shipping Total Treshold |  |
| `handlingflatrate` | float | Handling Flat Rate |  |
| `handlingfunctiontype` | text | Handling Rate Type |  |
| `handlingrateaspercentoftotal` | float | Handling Rate As Percent of Total |  |
| `handlingratebyitem` | float | Handling Rate By Item |  |
| `handlingratebyweight` | float | Handling Rate By Weight |  |
| `handlingratebyweightbracketed` | checkbox | Handling Rate By Weight Whole Increments |  |
| `handlingratebyweightper` | float | Handling Rate By Weight Per |  |
| `handlingratebyweightunit` | float | Handling Rate By Weight Unit |  |
| `handlingratetabletype` | text | Handling Rate Table Type |  |
| `hasmaxshippingcost` | checkbox | Has Maximum Shipping Cost |  |
| `hasminshippingcost` | checkbox | Has Minimum Shipping Cost |  |
| `internalid` | select | Internal ID |  |
| `isfreeiftotalover` | checkbox | Free Shipping total |  |
| `isinactive` | checkbox | Inactive |  |
| `isonline` | checkbox | Display in Web Site |  |
| `isreturnserviceintegrated` | checkbox | Is Return Service Integrated |  |
| `isshipperintegrated` | checkbox | Is Integrated Shipping Item |  |
| `itemid` | text | Name |  |
| `labelpluginid` | text | Label Plugin Id |  |
| `labelregistration` | text | Label Registration |  |
| `labelservicegroupid` | text | Label Service Group Id |  |
| `labelserviceid` | text | Label Service Id |  |
| `limitedtocountries` | multiselect | Limited To Countries |  |
| `limitedtosites` | multiselect | Limited To Sites |  |
| `limitedtostates` | multiselect | Limited To States |  |
| `limitwebsitevisibilitybytotal` | checkbox | Web Site Visibility Limited by Total |  |
| `limitwebsitevisibilitybyweight` | checkbox | Web Site Visibility Limited by Weight |  |
| `maxshippingcost` | float | Maximum Shipping Cost |  |
| `minshippingcost` | float | Minimum Shipping Cost |  |
| `ratingpluginid` | text | Rating Plugin Id |  |
| `ratingregistration` | text | Rating Registration |  |
| `ratingservicegroupid` | text | Rating Service Group Id |  |
| `ratingserviceid` | text | Rating Service Id |  |
| `realtimerateservice` | text | Real-Time Rate Service |  |
| `returnlabelregistration` | text | Return Label Registration |  |
| `returnlabelserviceid` | text | Return Label Service Id |  |
| `returnshippingservice` | text | Return Shipping Service |  |
| `shippingdiscountrate` | float | Shipping Rate Discount |  |
| `shippingflatrate` | float | Shipping Flat Rate |  |
| `shippingfunctiontype` | text | Shipping Rate Type |  |
| `shippingrateaspercentoftotal` | float | Shipping Rate As Percent of Total |  |
| `shippingratebyitem` | float | Shipping Rate By Item |  |
| `shippingratebyweight` | float | Shipping Rate By Weight |  |
| `shippingratebyweightbracketed` | checkbox | Shipping Rate By Weight Whole Increments |  |
| `shippingratebyweightper` | float | Shipping Rate By Weight Per |  |
| `shippingratebyweightunit` | float | Shipping Rate By Weight Unit |  |
| `shippingratetabletype` | text | Shipping Rate Table Type |  |
| `shippingservice` | text | Shipping Service |  |
| `subsidiary` | select | Subsidiary |  |
| `websitevisibilitybytotaltype` | text | Web Site Visibility By Total Type |  |
| `websitevisibilitybyweighttype` | text | Web Site Visibility By Weight Type |  |
| `websitevisibilitytotallimit` | float | Web Site Visibility By Total Limit |  |
| `websitevisibilityweightlimit` | float | Web Site Visibility By Weight Limit |  |
| `websitevisibilityweightunit` | text | Web Site Visibility By Weight Unit |  |

## Suspected source errors

> [!SOURCE-ERROR] `ups_help_says_usps` (on `upsdiscountrate`)
<!-- source-error: record:shipitem | upsdiscountrate | ups_help_says_usps -->
