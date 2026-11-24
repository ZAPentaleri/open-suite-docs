# Estimate (`estimate`)
<!-- id: record:estimate -->

| Fact | Value |
|---|---|
| Internal ID | `estimate` |
| Category | transaction |
| Client scripts | yes |
| Server scripts | yes |
| Searchable | yes |
| Search data via | Transaction |
| Tags | `custom_fields_supported`, `scripting_full` |

## Record facts

- `initialization_default_value`: `entity`, `entity_internal_id` (refs `N/record.create`, `N/record.copy`, `N/record.transform`, `N/record.load`)
- `script_types`: `client`, `server`
- `script_functions`: `create`, `update`, `copy`, `delete`, `search`, `transform`
- `body_only_scriptable_fields`: `estgrossprofit`, `estgrossprofitpercent`, `totalcostestimate`
- `before_load_runs_on_email_operation`: `true`
- `before_load_skipped_when`: `use_popup_for_main_transaction_email_button`
- `sublist_requires_feature`: `partners:multi_partner_management`
- `field_requires`: `shippingtaxcode:per_line_taxes`
- `caveat`: `gross_profit_fields_not_scriptable_at_line_level`
- `supported_in`: `transaction_search` [`estgrossprofit`, `estgrossprofitpercent`, `totalcostestimate`]
- `before_load_skipped_when`: `view_actions_email_with_popup_preference` [`email_popup_path`]
- `before_load_skipped_when`: `communications_subtab_email_with_popup_preference` [`email_popup_path`]
- `caveat`: `email_popup_actions_are_not_transaction_operations`
- `workaround`: `deploy_before_load_on_message_record` (when `emails_sent_from_popup`)
- `effect`: `save_and_email_always_triggers_before_load`
- `configured_by`: `use_popup_for_main_transaction_email_button_preference`

## Body fields

| Field ID | Type | Label | Required | submitField | Read-only | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `althandlingcost` | currency | Handling Cost | no | no |  |  | `purpose`: `handling_cost`; `value_source`: `calculated_from_ship_method`; `value_source`: `shipping_item_rates` |
| `altsalestotal` | currency | Total (Alt. Sales) | no | no |  |  | `purpose`: `alternate_sales_total`; `value_source`: `system_displayed` |
| `altshippingcost` | currency | Shipping Cost | no | no |  |  | `purpose`: `shipping_cost`; `value_source`: `calculated_from_ship_method`; `caveat`: `ups_real_time_splits_over_150_lb`; `value_source`: `shipping_item_rates`; `help_text_shared_with`: `invoice.shippingcost`, `invoicegroup.shippingcost` |
| `billaddress` | address | Bill To | no | no |  |  | `purpose`: `billing_address`; `default_behavior`: `customer_default_billing_address`; `can`: `choose_other_or_new_address_in_selector` |
| `billaddresslist` | select | Bill To Select | no | no |  |  | `purpose`: `billing_address_selector`; `ui_effect`: `new_option_creates_record` |
| `billingschedule` | select | Billing Schedule | no | no |  |  | `purpose`: `billing_schedule`; `effect`: `all_items_billed_by_schedule`; `effect`: `private_schedule_carried_to_sales_order`; `allowed_values_listed_in`: `billing_schedule_records`; `ui_effect`: `new_option_creates_record`; `option_meanings`: `public:selectable_everywhere`, `private:only_originating_transaction` (when `created_on_the_fly`) |
| `class` | select | Class | no | no |  |  | `purpose`: `class`; `allowed_values_listed_in`: `class_records`; `ui_effect`: `new_option_creates_record` |
| `couponcode` | select | Coupon Code | no | no |  |  | `purpose`: `coupon_code`; `value_source`: `promotion` (when `multiple_use_coupon`) |
| `currency` | select | Currency | yes | no |  |  | `purpose`: `transaction_currency`; `default_behavior`: `customer_primary_currency`; `requires_feature`: `Multi-Currency Customers` (when `choosing_transaction_currency`) [`real_name`]; `ui_effect`: `reprices_items_and_recalculates_tax_and_profit`; `caveat`: `custom_prices_not_repriced` |
| `currencyname` | text | Currency | no | no |  |  | `purpose`: `customer_currency_name`; `effect`: `all_amounts_in_this_currency` |
| `customform` | select | Custom Form | yes | no |  |  | `purpose`: `custom_form` |
| `department` | select | Department | no | no |  |  | `purpose`: `department`; `allowed_values_listed_in`: `department_records`; `ui_effect`: `new_option_creates_record`; `help_text_shared_with`: `purchaseorder.expense.department`, `purchaseorder.item.department` |
| `discountitem` | select | Discount | no | no |  |  | `purpose`: `discount_item`; `ui_effect`: `fills_rate_and_discount_total`; `allowed_values_listed_in`: `discount_items`; `default_behavior`: `no_discount` (when `blank`) |
| `discountrate` | rate | Rate | no | no |  |  | `purpose`: `discount_rate`; `value_source`: `discount_item_rate`; `accepts_forms`: `amount`, `percentage` |
| `discounttotal` | currency | Discount | no | no |  |  | `purpose`: `discount_total`; `value_source`: `calculated`; `caveat`: `taxable_discount_before_tax_else_after` |
| `duedate` | date | Expires | no | no |  |  | `purpose`: `estimate_expiration_date` |
| `enddate` | date | End Date | no | no |  |  | `purpose`: `end_date` |
| `entity` | select | Customer | yes | no |  |  | `purpose`: `estimate_customer`; `recommendation`: `select_job_not_parent`; `allowed_values_listed_in`: `customer_and_job_records`; `ui_effect`: `new_option_creates_record` |
| `entitystatus` | select | Status | yes | no |  |  | `purpose`: `estimate_status`; `effect`: `sets_probability`; `effect`: `updates_prospect_record`; `default_behavior`: `prospect_status_sales_preference`; `allowed_values_listed_in`: `customer_status_records` |
| `entitytaxregnum` | select | Tax Reg. Number | no |  |  |  | `purpose`: `entity_tax_registration` |
| `estgrossprofit` | currency | Est. Gross Profit | no | no | yes |  | `purpose`: `estimated_gross_profit`; `value_source`: `revenue_minus_estimated_cost` |
| `estgrossprofitpercent` | percent | Est. Gross Profit Percent | no | no | yes |  | `purpose`: `estimated_gross_margin_percent`; `value_source`: `estimated_profit_divided_by_revenue` |
| `exchangerate` | currency2 | Exchange Rate | yes | no |  |  | `purpose`: `exchange_rate`; `can`: `update_currency_record_rate` |
| `expectedclosedate` | date | Exp. Close | yes | yes |  |  | `purpose`: `expected_close_date` |
| `fob` | text | FOB | no | no |  |  | `purpose`: `fob_ownership_location`; `value_examples`: `our_warehouse` |
| `forecasttype` | select | Forecast Type | no | no |  |  | `purpose`: `forecast_category`; `special_values`: `Omitted` |
| `handlingcost` | currency | Handling Cost | no | no |  |  | `purpose`: `handling_cost`; `value_source`: `calculated_from_ship_method`; `value_source`: `shipping_item_rates` |
| `handlingtax1rate` | text | Tax Rate | no | no |  |  | `purpose`: `handling_tax_rate`; `value_source`: `system_displayed` |
| `handlingtaxcode` | select | Handling Tax Code | no | no |  |  | `purpose`: `handling_tax_code` |
| `ismultishipto` | checkbox | Enable Item Line Shipping | no | no |  |  | `purpose`: `multiple_ship_to`; `effect`: `per_line_shipping_amounts` |
| `istaxable` | checkbox | Taxable | no | no |  |  | `purpose`: `quoted_order_taxable`; `help_text_shared_with`: `estimate.item.istaxable` |
| `job` | select | Project | no | no |  |  | `purpose`: `project` |
| `leadsource` | select | Lead Source | no | no |  |  | `purpose`: `lead_source`; `default_behavior`: `default_lead_source_marketing_preference`; `can`: `override_per_transaction` |
| `location` | select | Location | no | no |  |  | `purpose`: `transaction_location`; `allowed_values_listed_in`: `location_records`; `ui_effect`: `new_option_creates_record` |
| `memo` | text | Memo | no | yes |  |  | `purpose`: `memo`; `displayed_on`: `registers`, `customer_center`; `value_kind`: `free_text` |
| `message` | textarea | Customer Message | no | no |  |  | `purpose`: `customer_message`; `value_source`: `selected_customer_message`; `can`: `edit_for_this_transaction` |
| `messagesel` | select | Select Message | no | no |  |  | `purpose`: `customer_message_selector`; `ui_effect`: `fills_message_field`; `allowed_values_listed_in`: `accounting_list_customer_message`; `value_examples`: `thank_you_for_your_business`; `can`: `edit_for_this_transaction` |
| `nexus` | select | Nexus | no | no |  |  | `purpose`: `transaction_nexus`; `value_source`: `nexus_lookup`; `can`: `override_with_nexus_override` |
| `opportunity` | select | Opportunity | no | no |  |  | `purpose`: `opportunity`; `allowed_values_listed_in`: `opportunity_records` |
| `otherrefnum` | text | PO # | no | no |  |  | `purpose`: `purchase_order_number` |
| `partner` | select | Partner | no | yes |  |  | `purpose`: `partner`; `default_behavior`: `customer_partner` |
| `probability` | percent | Probability | yes | no |  |  | `purpose`: `probability`; `default_behavior`: `customer_status_probability`; `effect`: `updates_prospect_record`; `configured_by`: `customer_status_probability_setting` |
| `promocode` | select | Promotion | no | no |  |  | `purpose`: `promotion`; `value_source`: `coupon_code_promotion` (when `single_use_coupon`); `configured_by`: `default_promotion_code_marketing_preference`; `constraint`: `partner_customers_use_partner_promotions_only`; `effect`: `partner_promotion_attributes_transaction_to_partner`; `allowed_values_listed_in`: `promotion_records` |
| `salesgroup` | select | Choose Team | no | no |  |  | `purpose`: `sales_team`; `allowed_values_listed_in`: `sales_team_groups` |
| `salesrep` | select | Sales Rep | no | yes |  |  | `purpose`: `sales_rep`; `default_behavior`: `customer_sales_rep`; `allowed_values_listed_in`: `employee_sales_reps`; `can`: `override_per_transaction` |
| `shipaddress` | address | Ship To | no | no |  |  | `purpose`: `shipping_address`; `default_behavior`: `customer_default_shipping_address`; `can`: `choose_other_or_new_address_in_selector` |
| `shipaddresslist` | select | Ship To Select | no | no |  |  | `purpose`: `shipping_address_selector`; `ui_effect`: `new_option_creates_record` |
| `shipdate` | date | Ship Date | no | no |  |  | `purpose`: `expected_ship_date` |
| `shipmethod` | select | Ship Via | no | no |  |  | `purpose`: `ship_method`; `effect`: `fills_shipping_cost` (when `charge_for_shipping`); `allowed_values_listed_in`: `shipping_item_records`; `configured_by`: `charge_for_shipping_setting` |
| `shippingcost` | currency | Shipping Cost | no | no |  |  | `purpose`: `shipping_cost`; `value_source`: `calculated_from_ship_method`; `special_values`: `ReCalc`; `caveat`: `ups_real_time_splits_over_150_lb` |
| `shippingtax1rate` | text | Tax Rate | no | no |  |  | `purpose`: `shipping_tax_rate`; `value_source`: `system_displayed` |
| `shippingtaxcode` | select | Shipping Tax Code | no | no |  |  | `purpose`: `shipping_tax_code` |
| `startdate` | date | Start Date | no | no |  |  | `purpose`: `start_date`; `default_behavior`: `current_date` |
| `subsidiary` | select | Subsidiary | no | no |  |  | `purpose`: `subsidiary`; `value_source`: `system_displayed` |
| `subsidiarytaxregnum` | select | Tax Reg. Number | no |  |  |  | `purpose`: `subsidiary_tax_registration`; `value_source`: `nexus_lookup`; `can`: `override_with_nexus_override` |
| `subtotal` | currency | Subtotal | no | no |  |  | `purpose`: `subtotal_before_discount_shipping_tax`; `value_source`: `sum_of_item_lines`; `doc_inconsistency`: `estimate_help_says_sales_order` |
| `syncpartnerteams` | checkbox | Update Customer | no | no |  |  | `purpose`: `sync_partner_team_to_customer` |
| `syncsalesteams` | checkbox | Update Customer | no | no |  |  | `purpose`: `sync_sales_team_to_customer`; `true_means`: `customer_team_updated_from_transaction`; `help_text_shared_with`: `creditmemo.syncpartnerteams`, `invoice.syncpartnerteams`, `salesorder.syncpartnerteams` |
| `taxdetailsoverride` | checkbox | Tax Details Override | no |  |  |  | `purpose`: `override_tax_details`; `requires_permission`: `tax_details_tab_edit` |
| `taxregoverride` | checkbox | Tax Registration Override | no |  |  |  | `purpose`: `override_nexus_and_tax_registration`; `requires_permission`: `tax_details_tab_edit` |
| `taxtotal` | currency | Tax Total | no | no |  |  | `purpose`: `tax_total`; `value_source`: `tax_rate_times_taxable_total` |
| `terms` | select | Terms | no | no |  |  | `purpose`: `discount_terms`; `effect`: `copied_to_sale_on_conversion` |
| `title` | text | Title | no | yes |  |  | `purpose`: `estimate_title`; `displayed_on`: `lists`, `search_results`, `estimates_subtab`; `default_behavior`: `opportunity_title` (when `converted_from_opportunity`); `effect`: `suffix_number_for_later_estimates_of_opportunity`; `supported_in`: `customization`, `inline_editing`, `mass_update`; `value_examples`: `Project Estimate`, `Project Estimate1` |
| `tobeemailed` | checkbox | To Be E-mailed | no | no |  |  | `purpose`: `email_transaction`; `format`: `semicolon_separated_no_spaces` |
| `tobefaxed` | checkbox | To Be Faxed | no | no |  |  | `purpose`: `fax_transaction`; `requires`: `fax_service_set_up`; `ui_effect`: `fax_number_shown_beside_checkbox` |
| `tobeprinted` | checkbox | To Be Printed | no | yes |  |  | `purpose`: `print_later`; `true_means`: `queued_for_printing` |
| `total` | currency | Total | no | no |  |  | `purpose`: `total`; `value_source`: `lines_plus_tax`; `help_text_shared_with`: `creditmemo.apply.total` |
| `totalcostestimate` | currency | Est. Extended Cost | no | no |  |  | `purpose`: `estimated_cost`; `value_source`: `estimated_rate_times_quantity` |
| `trackingnumbers` | text | Additional Tracking # | no | no |  |  | `purpose`: `tracking_numbers`; `ui_effect`: `ups_fedex_numbers_become_links` |
| `trandate` | date | Date | yes | no |  |  | `purpose`: `posting_date`; `default_behavior`: `current_date`; `displayed_on`: `registers` |
| `tranid` | text | Estimate # | no | no |  |  | `purpose`: `estimate_number`; `default_behavior`: `highest_number_plus_one`; `caveat`: `next_number_reverts_to_standard_pattern` |
| `visibletocustomer` | checkbox | Available In Customer | no | yes |  |  | `purpose`: `visible_in_customer_center`; `default_value`: `true` |
| `balance` | currency | Balance | no | no |  |  |  |
| `billingaddress` | summary |  | no | no |  |  |  |
| `billisresidential` | text | Shipping address is residential | no | no |  |  |  |
| `canhavestackable` | checkbox | Can have Stackable Promotions | no | no |  |  |  |
| `consolidatebalance` | currency | Consolidated Balance | no | no |  |  |  |
| `createddate` | datetime |  | no | no |  |  |  |
| `createdfrom` | select | Created From | no | no |  |  |  |
| `currencysymbol` | text | Currency Symbol | no | no |  |  |  |
| `email` | emails |  | no | yes |  |  |  |
| `entitynexus` | select | Nexus | no | no |  |  |  |
| `externalid` | text | ExternalId | no | no |  |  |  |
| `includeinforecasthigh` | text |  | no | no |  |  |  |
| `includeinforecastlow` | text |  | no | no |  |  |  |
| `isbasecurrency` | checkbox | Base Currency | no | no |  |  |  |
| `lastmodifieddate` | datetime |  | no | no |  |  |  |
| `linkedtrackingnumbers` | text | Tracking # | no | no |  |  |  |
| `muccpromocodeinstance` | text | Promo Code | no | no |  |  |  |
| `onetime` | currency2 | One Time | no | no |  |  |  |
| `promocodepluginimpl` | text | Promo Code | no | no |  |  |  |
| `recurannually` | currency2 | Annually | no | no |  |  |  |
| `recurmonthly` | currency2 | Monthly | no | no |  |  |  |
| `recurquarterly` | currency2 | Quarterly | no | no |  |  |  |
| `recurweekly` | currency2 | Weekly | no | no |  |  |  |
| `returntrackingnumbers` | text | Return Tracking # | no | no |  |  |  |
| `shipisresidential` | text | Shipping address is residential | no | no |  |  |  |
| `shipoverride` | text |  | no | no |  |  |  |
| `shippingaddress` | summary |  | no | no |  |  |  |
| `shippingcostoverridden` | text | Shipping Cost Overridden | no | no |  |  |  |
| `source` | text | Source | no | no |  |  |  |
| `status` | text |  | no | no |  |  |  |
| `statusref` | text |  | no | no |  |  |  |
| `taxpointdate` | date | Tax Point Date | no | no |  |  |  |
| `taxpointdateoverride` | checkbox | Override | no | no |  |  |  |
| `unbilledorders` | currency | Unbilled Orders | no | no |  |  |  |

## Sublist `item` (Items)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `istaxable` | checkbox | Tax | no |  | `purpose`: `quoted_order_taxable`; `help_text_shared_with`: `estimate.istaxable` |
| `altsalesamt` | currency | Alt. Sales | no |  |  |
| `amount` | currency | Amount | no |  |  |
| `billvariancestatus` | text |  | no |  |  |
| `catchupperiod` | select |  | no |  |  |
| `chargetype` | select | Charge Type | no |  |  |
| `costestimate` | currency | Est. Extended Cost | no |  |  |
| `costestimaterate` | rate |  | no |  |  |
| `costestimatetype` | select | Cost Estimate Type | no |  |  |
| `daysbeforeexpiration` | text |  | no |  |  |
| `deferrevrec` | checkbox |  | no |  |  |
| `description` | textarea | Description | no |  |  |
| `excludefromraterequest` | checkbox | Exclude Item from Rate Request | no |  |  |
| `expectedshipdate` | date | Expected Ship Date | no |  |  |
| `fromjob` | checkbox | Project Item | no |  |  |
| `id` | text |  | no |  |  |
| `inventorydetail` | summary | Inventory Detail | no |  |  |
| `isestimate` | checkbox | Billable Estimate | no |  |  |
| `isvsoebundle` | text |  | no |  |  |
| `item` | select | Item | yes |  |  |
| `itemsubtype` | text |  | no |  |  |
| `itemtype` | text |  | no |  |  |
| `job` | select | Job | no |  |  |
| `line` | text |  | no |  |  |
| `linenumber` | integer |  | no |  |  |
| `matrixtype` | text |  | no |  |  |
| `options` | namevaluelist | Options | no |  |  |
| `price` | select | Price Level | no |  |  |
| `printitems` | text |  | no |  |  |
| `quantity` | float | Quantity | no |  |  |
| `quantityavailable` | float |  | no |  |  |
| `rate` | rate | Rate | no |  |  |
| `rateschedule` | text |  | no |  |  |
| `shipaddress` | select | Ship To | no |  |  |
| `shipcarrier` | select | Shipping Carrier | no |  |  |
| `shipmethod` | select | Ship Via | no |  |  |
| `subscription` | select | Subscription | no |  |  |
| `taxcode` | select | Tax Code | yes |  |  |
| `taxrate1` | percent | Tax Rate | no |  |  |
| `units` | select | Units | no |  |  |

## Sublist `accountingbookdetail` (Accounting Books)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `accountingbook` | select | Secondary Book | no |  |  |
| `exchangerate` | currency2 | Exchange Rate | yes |  |  |

## Sublist `partners` (Partners)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `contribution` | percent | Contribution % | yes |  |  |
| `id` | text |  | no |  |  |
| `isprimary` | checkbox | Primary | no |  |  |
| `partner` | select | Partner | yes |  |  |
| `partnerrole` | select | Partner Role | no |  |  |
| `transaction` | text |  | no |  |  |

## Sublist `promotions` (Promotions)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `cannotbecombined` | text |  | no |  |  |
| `couponcode` | select | Coupon Code | yes |  |  |
| `discount` | select | Discount Item | no |  |  |
| `discountrate` | rate | Rate | no |  |  |
| `muccpromocodeinstance` | text | Promo Code | no |  |  |
| `promocode` | select | Promotion | yes |  |  |
| `purchasediscount` | currency | Purchase Discount | no |  |  |
| `shippingdiscount` | rtext | Shipping Discount | no |  |  |

## Sublist `salesteam` (Sales Team)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `contribution` | percent | Contribution % | yes |  |  |
| `employee` | select | Employee | yes |  |  |
| `id` | text |  | no |  |  |
| `isprimary` | checkbox | Primary | no |  |  |
| `issalesrep` | text |  | no |  |  |
| `salesrole` | select | Sales Role | yes |  |  |
| `transaction` | text |  | no |  |  |

## Sublist `shipgroup` (Shipment)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `destinationaddress` | text | Ship To | no |  |  |
| `handlingrate` | currency | Handling Rate | no |  |  |
| `id` | integer |  | no |  |  |
| `shippingcarrier` | text |  | no |  |  |
| `shippingmethod` | text | Ship Via | no |  |  |
| `shippingrate` | currency | Shipping Rate | no |  |  |
| `sourceaddress` | text | Ship From | no |  |  |
| `weight` | float |  | no |  |  |

## Sublist `taxdetails` (Tax Detail)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `calcdetail` | textarea | Details | no |  |  |
| `linename` | text | Name | no |  |  |
| `linetype` | text | Line type | no |  |  |
| `netamount` | currency | Net Amount | no |  |  |
| `taxamount` | currency | Tax Amount | yes |  |  |
| `taxbasis` | currency | Tax Basis | yes |  |  |
| `taxcode` | select | Tax Code | yes |  |  |
| `taxdetailsreference` | text | Tax Details Reference | no |  |  |
| `taxrate` | percent | Tax Rate | yes |  |  |
| `transaction` | select | Transaction | no |  |  |

## Tabs

| Tab ID | Label |
|---|---|
| `address` | Address |
| `custom` | Custom |
| `history` | History |
| `items` | Items |
| `multipartner` | Partners |
| `output` | Messages |
| `shipping` | Shipping |
| `teamselling` | Sales Team |
| `workflow` | Workflow |

## Transform targets

| Target record | Default fields |
|---|---|
| `cashsale` | `customform`, `billdate` |
| `invoice` | `customform`, `billdate` |
| `salesorder` | `customform` |

## Suspected source errors

> [!SOURCE-ERROR] `estimate_help_says_sales_order` (on `subtotal`)
<!-- source-error: record:estimate | subtotal | estimate_help_says_sales_order -->
