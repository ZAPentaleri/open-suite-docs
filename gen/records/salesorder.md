# Sales Order (`salesorder`)
<!-- id: record:salesorder -->

| Fact | Value |
|---|---|
| Internal ID | `salesorder` |
| Category | transaction |
| Client scripts | yes |
| Server scripts | yes |
| Searchable | yes |
| Search data via | Transaction |
| Tags | `custom_fields_supported`, `scripting_full` |

## Record facts

- `record_macros`: `autoAssignLocations:sets_best_line_locations`
- `returns_detail`: `lines_with_assigned_location`
- `related` (refs `N/record.Macro`)
- `ui_location`: `auto_assign_locations_button`
- `initialization_default_value`: `entity`, `entity_internal_id` (refs `N/record.create`, `N/record.copy`, `N/record.transform`, `N/record.load`)
- `script_types`: `client`, `server`
- `script_functions`: `create`, `update`, `copy`, `delete`, `search`, `transform`
- `body_only_scriptable_fields`: `estgrossprofit`, `estgrossprofitpercent`, `totalcostestimate`
- `before_load_runs_on_email_operation`: `true`
- `before_load_skipped_when`: `use_popup_for_main_transaction_email_button`
- `field_stored_only_when`: `isrecurringpayment:paymentmethod_credit_card`
- `record_macros`: `autoAssignLocations`
- `transform_param_may_be_required`: `inventorylocation` (when `intercompany_cross_subsidiary_fulfillment`)
- `purpose`: `commitment_to_sell_no_gl_impact_until_fulfilled`
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
| `altshippingcost` | currency | Shipping Cost | no | no |  |  | `purpose`: `shipping_cost`; `value_source`: `calculated_from_ship_method`; `caveat`: `ups_real_time_splits_over_150_lb`; `value_source`: `shipping_item_rates` |
| `authcode` | text | Auth. Code | no | no |  |  | `purpose`: `card_authorization_code`; `value_source`: `set_on_approval` (when `netsuite_merchant_account`); `use_when`: `card_validated_outside_netsuite` |
| `balance` | currency | Balance | no | no |  |  | `purpose`: `customer_balance`; `value_source`: `system_displayed` |
| `billaddress` | address | Bill To | no | no |  |  | `purpose`: `billing_address`; `default_behavior`: `customer_default_billing_address`; `options`: `New`, `Custom`; `option_meanings`: `new:saved_to_entity`, `custom:transaction_only`; `can`: `edit_existing_address_with_edit_icon` |
| `billaddresslist` | select | Bill To Select | no | no |  |  | `purpose`: `billing_address_selector`; `options`: `New`, `Custom`; `option_meanings`: `new:saved_to_entity`, `custom:transaction_only`; `can`: `edit_existing_address_with_edit_icon` |
| `billingschedule` | select | Billing Schedule | no | no |  |  | `purpose`: `billing_schedule`; `effect`: `all_items_billed_by_schedule`; `ui_effect`: `new_option_creates_record` |
| `ccapproved` | checkbox | Credit Card Approved | no | no |  |  | `purpose`: `card_approved_externally`; `use_when`: `card_validated_outside_netsuite`; `value_source`: `set_on_approval` (when `netsuite_merchant_account`) |
| `ccavsstreetmatch` | select | AVS Street Match | no | no |  |  | `purpose`: `avs_street_result`; `allowed_values`: `Y`, `N`, `X`; `option_meanings`: `y:matched`, `n:not_matched`, `x:not_supported_or_no_response`; `value_source`: `card_processor_response` |
| `ccavszipmatch` | select | AVS Zip Match | no | no |  |  | `purpose`: `avs_zip_result`; `allowed_values`: `Y`, `N`, `X`; `option_meanings`: `y:matched`, `n:not_matched`, `x:not_supported_or_no_response`; `value_source`: `card_processor_response` |
| `ccexpiredate` | ccexpdate | Expires (MM/YYYY) | no | no |  |  | `purpose`: `card_expiration_date`; `format`: `MM/YYYY` |
| `ccname` | text | Name on Card | no | no |  |  | `purpose`: `cardholder_name`; `default_behavior`: `customer_default_card_holder`; `recommendation`: `fix_value_on_customer_record` |
| `ccnumber` | ccnumber | Credit Card # | no | no |  |  | `purpose`: `card_number`; `format`: `digits_without_spaces`; `caveat`: `visible_only_to_merchant_and_customer` |
| `ccsecuritycode` | text | CSC | no | no |  |  | `purpose`: `card_security_code`; `format`: `three_digits` |
| `ccsecuritycodematch` | select | CSC Match | no | no |  |  | `purpose`: `csc_result_code`; `allowed_values`: `Y`, `N`, `X`; `option_meanings`: `y:matched`, `n:not_matched`, `x:not_supported_or_no_response`; `value_source`: `card_processor_response` |
| `ccstreet` | text | Card Street | no | no |  |  | `purpose`: `card_billing_street`; `value_source`: `customer_billing_address_street`; `recommendation`: `fix_value_on_customer_record` |
| `cczipcode` | text | Card Zip Code | no | no |  |  | `purpose`: `card_billing_postal_code`; `value_source`: `customer_billing_address_postal_code`; `recommendation`: `fix_value_on_customer_record` |
| `class` | select | Class | no | no |  |  | `purpose`: `class`; `allowed_values_listed_in`: `class_records`; `ui_effect`: `new_option_creates_record` |
| `couponcode` | select | Coupon Code | no | no |  |  | `purpose`: `coupon_code`; `value_source`: `promotion` (when `multiple_use_coupon`) |
| `createdfrom` | select | Created From | no | no |  |  | `purpose`: `created_from_opportunity_or_estimate`; `value_source`: `system_displayed` |
| `creditcard` | select | Credit Card Select | no | no |  |  | `purpose`: `stored_card_for_payment`; `allowed_values_listed_in`: `customer_stored_cards`; `ui_effect`: `new_option_creates_record` |
| `creditcardprocessor` | select | Credit Card Processor | no | no |  |  | `purpose`: `payment_processing_profile`; `allowed_values_listed_in`: `payment_processing_profiles` |
| `currency` | select | Currency | yes | no |  |  | `purpose`: `transaction_currency`; `default_behavior`: `customer_primary_currency`; `requires_feature`: `Multi-Currency Customers` (when `choosing_transaction_currency`) [`real_name`]; `ui_effect`: `reprices_items_and_recalculates_tax_and_profit`; `caveat`: `custom_prices_not_repriced` |
| `currencyname` | text | Currency | no | no |  |  | `purpose`: `customer_currency_name`; `effect`: `all_amounts_in_this_currency` |
| `customercode` | text | Customer Code | no | no |  |  | `purpose`: `level_2_customer_code`; `reason`: `best_level_2_processor_rates` |
| `customform` | select | Custom Form | yes | no |  |  | `purpose`: `custom_form`; `ui_effect`: `new_option_creates_record` |
| `debitcardissueno` | text | Issue No. | no | no |  |  | `purpose`: `debit_card_issue_number` |
| `deferredrevenue` | currency | Deferred Revenue | no | no |  |  | `purpose`: `deferred_revenue_amount` |
| `department` | select | Department | no | no |  |  | `purpose`: `department`; `allowed_values_listed_in`: `department_records`; `ui_effect`: `new_option_creates_record` |
| `discountitem` | select | Discount Item | no | no |  |  | `purpose`: `discount_item`; `ui_effect`: `fills_rate_and_discount_total`; `default_behavior`: `no_discount` (when `blank`); `allowed_values_listed_in`: `discount_items` |
| `discountrate` | rate | Rate | no | no |  |  | `purpose`: `discount_rate`; `default_behavior`: `discount_item_rate`; `format`: `negative_amount_or_percent_to_subtract`; `value_examples`: `-10.00`, `-10%` |
| `discounttotal` | currency | Discount Item | no | no |  |  | `purpose`: `discount_total`; `value_source`: `calculated`; `caveat`: `taxable_discount_before_tax_else_after` |
| `draccount` | select | Deferred Revenue Reclassification Account | no | no |  |  | `purpose`: `deferred_revenue_reclass_account`; `requires_feature`: `Revenue Commitment` [`real_name`]; `caveat`: `ignored_on_new_orders_after_arm_enabled` |
| `enddate` | date | End Date | no | no |  |  | `purpose`: `end_date`; `effect`: `copied_to_invoice_or_cash_sale_on_billing` |
| `entity` | select | Customer | yes | no |  |  | `purpose`: `customer_or_project`; `use_instead`: `job_field_for_project` (when `advanced_projects`) |
| `entitytaxregnum` | select | Tax Reg. Number | no |  |  |  | `purpose`: `customer_tax_registration` |
| `estgrossprofit` | currency | Est. Gross Profit | no | no | yes |  | `purpose`: `estimated_gross_profit`; `value_source`: `revenue_minus_estimated_cost` |
| `estgrossprofitpercent` | percent | Est. Gross Profit Percent | no | no | yes |  | `purpose`: `estimated_gross_margin_percent`; `value_source`: `estimated_profit_divided_by_revenue` |
| `exchangerate` | currency2 | Exchange Rate | yes | no |  |  | `purpose`: `exchange_rate`; `can`: `update_currency_record_rate` |
| `excludecommission` | checkbox | Exclude Commissions | no | yes |  |  | `purpose`: `exclude_from_commissions`; `effect`: `includes_subordinate_transactions` |
| `fob` | text | FOB | no | no |  |  | `purpose`: `fob_ownership_location`; `value_examples`: `our_warehouse` |
| `forinvoicegrouping` | checkbox | For Invoice Grouping | no | no |  |  | `purpose`: `include_in_invoice_grouping`; `use_cases`: `print_group_details`, `accept_group_payments`, `group_saved_search_reports` |
| `fxaccount` | select | Foreign Currency Adjustment Revenue Account | no | no |  |  | `purpose`: `fx_adjustment_revenue_account`; `requires_feature`: `Revenue Commitment` [`real_name`]; `constraint`: `income_account`; `caveat`: `ignored_on_new_orders_after_arm_enabled` |
| `getauth` | checkbox | Get Authorization | no | no |  |  | `purpose`: `get_card_authorization`; `requires`: `netsuite_merchant_account`; `effect`: `save_sends_gateway_request`; `effect`: `sale_captured_as_customer_deposit` (when `profile_supports_sale_only`) |
| `handlingcost` | currency | Handling Cost | no | no |  |  | `purpose`: `handling_cost`; `value_source`: `shipping_method_handling_rates`; `value_examples`: `flat_1.50_us_mail` |
| `handlingtax1rate` | text | Handling Tax Rate | no | no |  |  | `purpose`: `handling_tax_rate`; `value_source`: `system_displayed` |
| `handlingtaxcode` | select | Handling Tax Code | no | no |  |  | `purpose`: `handling_tax_code` |
| `intercostatus` | select | Intercompany Status | no | no |  |  | `purpose`: `intercompany_status`; `allowed_values`: `Linked`, `Pending`, `Rejected`; `option_meanings`: `linked:paired_with_other_subsidiary_order`, `pending:not_paired`, `rejected:cannot_pair_incorrect_details` |
| `intercotransaction` | select | Paired Intercompany Transaction | no | no |  |  | `purpose`: `paired_intercompany_order`; `allowed_values`: `sales_order_purchase_order`, `vendor_return_return_authorization` |
| `ismultishipto` | checkbox | Enable Item Line Shipping | no | no |  |  | `purpose`: `multiple_ship_to`; `effect`: `per_line_ship_address_and_method`; `default_behavior`: `customer_default_address_on_lines`; `options`: `New`, `Custom`; `option_meanings`: `new:saved_to_customer`, `custom:transaction_only` |
| `isrecurringpayment` | checkbox | Recurring Payment | no | no |  |  | `purpose`: `recurring_payment`; `effect`: `processes_after_card_expiry`; `use_when`: `billing_schedule_or_memorized_orders` |
| `istaxable` | checkbox | Taxable | no | no |  |  | `purpose`: `taxable`; `help_text_shared_with`: `cashsale.item.istaxable`, `salesorder.item.istaxable` |
| `leadsource` | select | Lead Source | no | no |  |  | `purpose`: `lead_source`; `allowed_values_listed_in`: `campaigns` (when `marketing_automation`) |
| `location` | select | Location | no | no |  |  | `purpose`: `transaction_location`; `allowed_values_listed_in`: `location_records`; `ui_effect`: `new_option_creates_record` |
| `memo` | text | Memo | no | yes |  |  | `purpose`: `memo`; `displayed_on`: `registers` |
| `message` | textarea | Customer Message | no | no |  |  | `purpose`: `customer_message`; `value_source`: `selected_customer_message`; `can`: `edit_for_this_transaction` |
| `messagesel` | select | Select Message | no | no |  |  | `purpose`: `customer_message_selector`; `ui_effect`: `fills_message_field`; `allowed_values_listed_in`: `accounting_list_customer_message`; `value_examples`: `thank_you_for_your_business` |
| `nexus` | select | Nexus | no | no |  |  | `purpose`: `transaction_nexus`; `value_source`: `nexus_lookup`; `can`: `override_with_nexus_override` |
| `opportunity` | select | Opportunity | no | no |  |  | `purpose`: `opportunity`; `allowed_values_listed_in`: `opportunity_records` |
| `orderstatus` | select | Status | yes | no |  |  | `purpose`: `order_status`; `default_value`: `Pending Approval` (when `sales_order_approval`); `default_value`: `Pending Fulfillment` (when `no_approval`); `configured_by`: `company_sales_order_default_status`; `can`: `override` |
| `ordertype` | select | Order Type | no |  |  |  | `purpose`: `order_type`; `effect`: `referenced_by_pick_strategies_and_reports` |
| `otherrefnum` | text | PO # | no | no |  |  | `purpose`: `check_or_po_number` |
| `partner` | select | Partner | no | yes |  |  | `purpose`: `partner`; `allowed_values_listed_in`: `partner_records` |
| `paymentmethod` | select | Payment Method | no | no |  |  | `purpose`: `payment_method`; `read_only_when`: `google_checkout_order` |
| `paypalauthid` | text | Authorization ID | no | no |  |  | `purpose`: `paypal_authorization_id`; `value_source`: `paypal_response` |
| `paypalprocess` | checkbox | Process PayPal Payment | no | no |  |  | `purpose`: `process_via_paypal`; `false_means`: `paypal_fields_stored_not_sent`; `default_value`: `true` (when `paypal_express_checkout_order`) |
| `paypalstatus` | text | PayPal Status | no | no |  |  | `purpose`: `paypal_payment_status`; `value_source`: `paypal_response`; `allowed_values`: `None`, `Canceled-Reversal`, `Completed`, `Denied`, `Expired`, `Failed`, `Pending`, `Refunded`, `Reversed`, `Processed`, `Voided`; `caveat`: `failed_only_for_bank_account_payments`; `caveat`: `completed_only_value_for_point_of_sale`; `option_meanings`: `canceled_reversal:reversal_canceled_funds_returned`, `completed:funds_added`, `denied:payment_denied`, `expired:authorization_expired`, `failed:bank_payment_failed`, `refunded:payment_refunded`, `reversed:chargeback_funds_removed`, `processed:payment_accepted`, `voided:authorization_voided` |
| `paypaltranid` | text | PayPal Tran. ID | no | no |  |  | `purpose`: `paypal_transaction_id`; `value_source`: `paypal_response` |
| `pnrefnum` | text | P/N Ref. | no | no |  |  | `purpose`: `payment_request_id`; `value_source`: `set_on_authorization` |
| `promocode` | select | Promotion | no | no |  |  | `purpose`: `promotion`; `value_source`: `coupon_code_promotion` (when `single_use_coupon`); `configured_by`: `default_promotion_code_marketing_preference`; `constraint`: `partner_customers_use_partner_promotions_only`; `effect`: `partner_promotion_attributes_transaction_to_partner`; `allowed_values_listed_in`: `promotion_records` |
| `recognizedrevenue` | currency | Recognized Revenue | no | no |  |  | `purpose`: `cumulative_recognized_revenue` |
| `revcommitstatus` | text | Rev. Commit. Status | no | no |  |  | `purpose`: `revenue_commitment_status`; `allowed_values`: `Pending Commitment`, `Partially Committed`, `Committed`; `value_source`: `related_invoices_or_credit_memos` (when `revreconrevcommitment_cleared`) |
| `revenuestatus` | text | Revenue Status | no | no |  |  | `purpose`: `revenue_recognition_status`; `allowed_values`: `Pending`, `In Progress`, `Completed`; `option_meanings`: `pending:nothing_recognized_all_deferred`, `in_progress:partly_recognized`, `completed:fully_recognized_none_deferred` |
| `revreconrevcommitment` | checkbox | Rev Rec on Rev Commit. | no | no |  |  | `purpose`: `create_revenue_commitment`; `effect`: `commitment_replaces_invoice_or_credit_memo`; `effect`: `creates_revenue_recognition_schedules` |
| `saleseffectivedate` | date | Sales Effective Date | no | no |  |  | `purpose`: `sales_effective_date`; `effect`: `selects_commission_plan_and_historical_team` |
| `salesgroup` | select | Choose Team | no | no |  |  | `purpose`: `sales_team`; `allowed_values_listed_in`: `sales_team_groups` |
| `salesrep` | select | Sales Rep | no | yes |  |  | `purpose`: `sales_rep`; `default_behavior`: `customer_sales_rep`; `allowed_values_listed_in`: `employee_sales_reps`; `can`: `override_per_transaction` |
| `shipaddress` | address | Ship To | no | no |  |  | `purpose`: `shipping_address`; `default_behavior`: `customer_default_shipping_address`; `options`: `New`, `Custom`; `option_meanings`: `new:saved_to_entity`, `custom:transaction_only` |
| `shipaddresslist` | select | Ship To Select | no | no |  |  | `purpose`: `shipping_address_selector`; `options`: `New`, `Custom`; `option_meanings`: `new:saved_to_entity`, `custom:transaction_only` |
| `shipcomplete` | checkbox | Ship Complete | no | no |  |  | `purpose`: `ship_complete_only`; `effect`: `prevents_partial_shipments`; `supported_in`: `bulk_fulfillment_filter` |
| `shipdate` | date | Ship Date | no | no |  |  | `purpose`: `ship_date` |
| `shipmethod` | select | Shipping Method | no | no |  |  | `purpose`: `ship_method`; `effect`: `drives_shipping_cost` (when `charge_for_shipping`); `value_examples`: `next_day_delivery`, `airmail` |
| `shippingcost` | currency | Shipping Cost | no | no |  |  | `purpose`: `shipping_cost`; `value_source`: `calculated_from_ship_method` (when `charge_for_shipping`); `configured_by`: `charge_for_shipping_setting`; `caveat`: `ups_real_time_splits_over_150_lb` |
| `shippingtax1rate` | text | Shipping Tax Rate | no | no |  |  | `purpose`: `shipping_tax_rate`; `value_source`: `system_displayed` |
| `shippingtaxcode` | select | Shipping Tax Code | no | no |  |  | `purpose`: `shipping_tax_code` |
| `softdescriptor` | text | Soft Descriptor | no | no |  |  | `purpose`: `soft_descriptor`; `displayed_on`: `customer_card_statement` |
| `source` | text | Source | no | no |  |  | `purpose`: `transaction_source_role_and_location`; `value_source`: `system_displayed` |
| `startdate` | date | Start Date | no | no |  |  | `purpose`: `first_invoice_date`; `requires_feature`: `Advanced Billing` [`real_name`]; `effect`: `copied_to_invoice_or_cash_sale_on_billing` |
| `subsidiary` | select | Subsidiary | no | no |  |  | `purpose`: `subsidiary`; `value_source`: `system_displayed` |
| `subsidiarytaxregnum` | select | Tax Reg. Number | no |  |  |  | `purpose`: `subsidiary_tax_registration`; `value_source`: `nexus_lookup`; `can`: `override_with_nexus_override` |
| `subtotal` | currency | Subtotal | no | no |  |  | `purpose`: `subtotal_before_discount_shipping_tax`; `value_source`: `sum_of_item_lines`; `doc_inconsistency`: `estimate_help_says_sales_order` |
| `syncpartnerteams` | checkbox | Update Customer | no | no |  |  | `purpose`: `sync_sales_team_to_customer`; `true_means`: `customer_team_updated_from_transaction`; `help_text_shared_with`: `cashrefund.syncsalesteams`, `cashsale.syncsalesteams`, `creditmemo.syncsalesteams`, `estimate.syncsalesteams`, `invoice.syncsalesteams`, `returnauthorization.syncsalesteams`, `salesorder.syncsalesteams` |
| `syncsalesteams` | checkbox | Update Customer | no | no |  |  | `purpose`: `sync_sales_team_to_customer`; `true_means`: `customer_team_updated_from_transaction`; `help_text_shared_with`: `creditmemo.syncpartnerteams`, `invoice.syncpartnerteams`, `salesorder.syncpartnerteams` |
| `taxdetailsoverride` | checkbox | Tax Details Override | no |  |  |  | `purpose`: `override_tax_details`; `requires_permission`: `tax_details_tab_edit` |
| `taxitem` | select | Tax | no | no |  |  | `purpose`: `tax_item`; `default_behavior`: `customer_tax_item`; `allowed_values_listed_in`: `sales_tax_items`; `can`: `override_per_transaction` |
| `taxpointdate` | date | Tax Point Date | no | no |  |  | `purpose`: `tax_point_date`; `value_kind`: `date_determining_taxability` |
| `taxpointdateoverride` | checkbox | Override | no | no |  |  | `purpose`: `override_tax_point_date` |
| `taxrate` | float | Tax % | no | no |  |  | `purpose`: `tax_rate`; `default_behavior`: `tax_item_rate`; `can`: `override_per_transaction`; `value_unit`: `percent` |
| `taxregoverride` | checkbox | Tax Registration Override | no |  |  |  | `purpose`: `override_nexus_and_tax_registration`; `requires_permission`: `tax_details_tab_edit` |
| `taxtotal` | currency | Tax Total | no | no |  |  | `purpose`: `tax_total`; `value_source`: `tax_rate_times_taxable_total` |
| `terms` | select | Terms | no | no |  |  | `purpose`: `terms`; `allowed_values_listed_in`: `accounting_list_terms` |
| `tobeemailed` | checkbox | To Be E-mailed | no | no |  |  | `purpose`: `email_transaction`; `format`: `semicolon_separated_no_spaces` |
| `tobefaxed` | checkbox | To Be Faxed | no | no |  |  | `purpose`: `fax_transaction`; `requires`: `fax_service_set_up` |
| `tobeprinted` | checkbox | To Be Printed | no | yes |  |  | `purpose`: `print_later`; `true_means`: `queued_with_picking_tickets` |
| `total` | currency | Total | no | no |  |  | `purpose`: `total`; `value_source`: `lines_tax_and_shipping` |
| `totalcostestimate` | currency | Est. Extended Cost | no | no |  |  | `purpose`: `estimated_cost`; `value_source`: `estimated_rate_times_quantity` |
| `trandate` | date | Date | yes | no |  |  | `purpose`: `posting_date`; `default_behavior`: `current_date`; `displayed_on`: `registers` |
| `tranid` | text | Order # | no | no |  |  | `purpose`: `sales_order_number`; `default_behavior`: `highest_number_plus_one`; `max_length`: `45` characters; `caveat`: `next_number_reverts_to_standard_pattern` |
| `tranisvsoebundle` | checkbox | Transaction Is VSOE Bundle | no | no |  |  | `purpose`: `vsoe_bundle`; `effect`: `order_dates_drive_schedule` (when `term_source_dates_on_sales_order`) |
| `validfrom` | ccvalidfrom | Valid From / Start Date (MM/YYYY) | no | no |  |  | `purpose`: `card_valid_from`; `format`: `MM/YYYY` |
| `vsoeautocalc` | checkbox | Auto Calculate VSOE Allocation | no | no |  |  | `purpose`: `auto_calculate_allocation`; `effect`: `allocation_calculated_on_save` |
| `allowemptycards` | checkbox | Credits | no | no |  |  |  |
| `aomautomated` | checkbox | AOM Automated | no | no |  |  |  |
| `billingaddress` | summary |  | no | no |  |  |  |
| `billisresidential` | text | Shipping address is residential | no | no |  |  |  |
| `canhavestackable` | checkbox | Can have Stackable Promotions | no | no |  |  |  |
| `cchold` | text | Detail | no | no |  |  |  |
| `ccholdetails` | textarea | Detail | no | no |  |  |  |
| `cciavsmatch` | select | IAVS Match | no | no |  |  |  |
| `ccprocessoraccount` | text | Credit Card Processing Profile | no | no |  |  |  |
| `checknumber` | text | Check # | no | no |  |  |  |
| `consolidatebalance` | currency | Consolidated Balance | no | no |  |  |  |
| `createddate` | datetime |  | no | no |  |  |  |
| `currencysymbol` | text | Currency Symbol | no | no |  |  |  |
| `dynamicdescriptor` | text | Soft Descriptor | no | no |  |  |  |
| `email` | emails |  | no | yes |  |  |  |
| `entitynexus` | select | Nexus | no | no |  |  |  |
| `externalid` | text | ExternalId | no | no |  |  |  |
| `giftcertapplied` | currency | Gift Certificate | no | no |  |  |  |
| `handlingmode` | select | Handling Mode | no | no |  |  |  |
| `ignoreavs` | checkbox | Ignore AVS | no | no |  |  |  |
| `ignorecsc` | checkbox | Ignore CSC | no | no |  |  |  |
| `inputauthcode` | text | Input Auth. Code | no | no |  |  |  |
| `inputpnrefnum` | text |  | no | no |  |  |  |
| `inputreferencecode` | text | Input P/N Ref. | yes | no |  |  |  |
| `isbasecurrency` | checkbox | Base Currency | no | no |  |  |  |
| `isdefaultshippingrequest` | text | Default | no | no |  |  |  |
| `ispurchasecard` | text | Override Hold | no | no |  |  |  |
| `lastmodifieddate` | datetime |  | no | no |  |  |  |
| `linkedtrackingnumbers` | text | Tracking # | no | no |  |  |  |
| `muccpromocodeinstance` | text | Promo Code | no | no |  |  |  |
| `nextbill` | date |  | no |  |  |  |  |
| `onetime` | currency2 | One Time | no | no |  |  |  |
| `outputauthcode` | text | Auth. Code | no | no |  |  |  |
| `outputreferencecode` | text | P/N Ref. | no | no |  |  |  |
| `overridehold` | checkbox | Override Hold | no | no |  |  |  |
| `overrideholdchecked` | checkbox | Override Hold | no | no |  |  |  |
| `overrideshippingcost` | float | Default | no | no |  |  |  |
| `paymentcardcsc` | text | CSC | no | no |  |  |  |
| `paymenteventdate` | datetime | Override Hold | no | no |  |  |  |
| `paymenteventholdreason` | select | Reason | no | no |  |  |  |
| `paymenteventpurchasedatasent` | text | Override Hold | no | no |  |  |  |
| `paymenteventresult` | select | Status | no | no |  |  |  |
| `paymenteventtype` | text | Override Hold | no | no |  |  |  |
| `paymenteventupdatedby` | text | Override Hold | no | no |  |  |  |
| `paymentoperation` | text | Payment Operation | no |  |  |  |  |
| `paymentoption` | select | Payment Option | no | no |  |  |  |
| `paymentprocessingprofile` | select | Payment Processing Profile | no | no |  |  |  |
| `promocodepluginimpl` | text | Promo Code | no | no |  |  |  |
| `recurannually` | currency2 | Annually | no | no |  |  |  |
| `recurmonthly` | currency2 | Monthly | no | no |  |  |  |
| `recurquarterly` | currency2 | Quarterly | no | no |  |  |  |
| `recurweekly` | currency2 | Weekly | no | no |  |  |  |
| `returntrackingnumbers` | text | Return Tracking # | no | no |  |  |  |
| `saascommitmentenddate` | date | SaaS Metric Commitment End Date | no | no |  | marked_new |  |
| `saascommitmentstartdate` | date | SaaS Metric Commitment Start Date | no | no |  | marked_new |  |
| `shipisresidential` | text | Shipping address is residential | no | no |  |  |  |
| `shipoverride` | text |  | no | no |  |  |  |
| `shippingaddress` | summary |  | no | no |  |  |  |
| `shippingcostoverridden` | text | Shipping Cost Overridden | no | no |  |  |  |
| `status` | text |  | no | no |  |  |  |
| `statusRef` | text |  | no | no |  |  |  |
| `threedstatuscode` | text | Payer Authentication Status | no | no |  |  |  |
| `unbilledorders` | currency | Unbilled Orders | no | no |  |  |  |

## Sublist `item` (Items)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `istaxable` | checkbox | Tax | no |  | `purpose`: `taxable`; `help_text_shared_with`: `cashsale.istaxable`, `salesorder.istaxable` |
| `altsalesamt` | currency | Alt. Sales | no |  |  |
| `amortizationperiod` | text |  | no |  |  |
| `amortizationtype` | text |  | no |  |  |
| `amount` | currency | Amount | no |  |  |
| `billingschedule` | select | Billing Schedule | no |  |  |
| `billvariancestatus` | text |  | no |  |  |
| `catchupperiod` | select |  | no |  |  |
| `chargerule` | select | Project Charge Rule | no |  |  |
| `chargetype` | text | Charge Type | no |  |  |
| `commitinventory` | select | Commit | no |  |  |
| `commitmentfirm` | checkbox | Commitment Confirmed | no |  |  |
| `costestimate` | currency | Est. Extended Cost | no |  |  |
| `costestimaterate` | rate |  | no |  |  |
| `costestimatetype` | select | Cost Estimate Type | no |  |  |
| `createdpo` | select |  | no |  |  |
| `createpo` | select | Create PO | no |  |  |
| `createwo` | checkbox | Create WO | no |  |  |
| `daysbeforeexpiration` | text |  | no |  |  |
| `deferrevrec` | checkbox |  | no |  |  |
| `description` | textarea | Description | no |  |  |
| `excludefrompredictiverisk` | checkbox | Exclude From Predictive Risk | no |  |  |
| `excludefromraterequest` | checkbox | Exclude Item from Rate Request | no |  |  |
| `expectedshipdate` | date | Expected Ship Date | no |  |  |
| `fromjob` | checkbox | Project Item | no |  |  |
| `giftcertfrom` | text | From | no |  |  |
| `giftcertmessage` | textarea | Gift Message | no |  |  |
| `giftcertrecipientemail` | email | Recipient Email | no |  |  |
| `giftcertrecipientname` | text | Recipient Name | no |  |  |
| `id` | text |  | no |  |  |
| `inventorydetail` | summary | Inventory Detail | no |  |  |
| `inventorylocation` | select | Inventory Location | no |  |  |
| `inventorysubsidiary` | select | Inventory Subsidiary | no |  |  |
| `isclosed` | checkbox | Closed | no |  |  |
| `isestimate` | checkbox | Billable Estimate | no |  |  |
| `isvsoebundle` | text |  | no |  |  |
| `item` | select | Item | yes |  |  |
| `itemfulfillmentchoice` | text |  | no |  |  |
| `itemsubtype` | text |  | no |  |  |
| `itemtype` | text |  | no |  |  |
| `licensecode` | textarea | License Code | no |  |  |
| `line` | text |  | no |  |  |
| `linenumber` | integer |  | no |  |  |
| `lineuniquekey` | text |  | no |  |  |
| `locationautoassigned` | checkbox |  | no |  |  |
| `matrixtype` | text |  | no |  |  |
| `noautoassignlocation` | checkbox |  | no |  |  |
| `options` | namevaluelist | Options | no |  |  |
| `orderallocationstrategy` | select | Allocation Strategy | no |  |  |
| `orderpriority` | float | Order Priority | no |  |  |
| `porate` | currency2 |  | no |  |  |
| `povendor` | text |  | no |  |  |
| `price` | select | Price Level | no |  |  |
| `printitems` | text |  | no |  |  |
| `quantity` | float | Quantity | no |  |  |
| `quantityavailable` | float |  | no |  |  |
| `quantitybackordered` | float | Back Ordered | no |  |  |
| `quantitybilled` | float | Invoiced | no |  |  |
| `quantitycommitted` | float | Committed | no |  |  |
| `quantityfulfilled` | float | Fulfilled | no |  |  |
| `quantityrevcommitted` | float | Rev. Committed | no |  |  |
| `rate` | rate | Unit Price | no |  |  |
| `rateschedule` | text |  | no |  |  |
| `requesteddate` | date | Supply Required By Date | no |  |  |
| `revrecenddate` | date | Rev. Rec. End Date | no |  |  |
| `revrecschedule` | select | Rev. Rec. Schedule | no |  |  |
| `revrecstartdate` | date | Rev. Rec. Start Date | no |  |  |
| `shipaddress` | select | Ship To | no |  |  |
| `shipcarrier` | select | Carrier | no |  |  |
| `shipmethod` | select | Ship Via | no |  |  |
| `subscription` | select | Subscription | no |  |  |
| `taxcode` | select | Tax Code | yes |  |  |
| `taxrate1` | percent | Tax Rate | no |  |  |
| `units` | select | Units | no |  |  |
| `vsoeallocation` | currency | Allocation Amount | no |  |  |
| `vsoeamount` | currency | Calculated Amount | no |  |  |
| `vsoedeferral` | select | Deferral | no |  |  |
| `vsoedelivered` | checkbox | Delivered | no |  |  |
| `vsoeisestimate` | checkbox | Estimate | no |  |  |
| `vsoepermitdiscount` | select | Permit Discount | no |  |  |
| `vsoeprice` | currency2 | Allocation Price | no |  |  |
| `vsoesopgroup` | select | Allocation Type | no |  |  |

## Sublist `accountingbookdetail` (Accounting Books)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `accountingbook` | select | Secondary Book | no |  |  |
| `exchangerate` | currency2 | Exchange Rate | yes |  |  |
| `revreconrevcommitment` | checkbox | Rev Rec on Rev Commit. | no |  |  |
| `tranisvsoebundle` | checkbox | Transaction Is VSOE Bundle | no |  |  |

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
| `milestones` | Milestones |
| `multipartner` | Partners |
| `output` | Messages |
| `payment` | Payment |
| `revenue` | Revenue |
| `shipping` | Shipping |
| `teamselling` | Sales Team |
| `workflow` | Workflow |

## Transform targets

| Target record | Default fields |
|---|---|
| `cashsale` | `customform`, `billdate` |
| `customsale` | `customform` |
| `invoice` | `customform`, `billdate` |
| `itemfulfillment` | `customform`, `nsi`, `shipgroup` |
| `returnauthorization` | `customform`, `cf` |
| `revenuecommitment` | `customform` |

## Suspected source errors

> [!SOURCE-ERROR] `estimate_help_says_sales_order` (on `subtotal`)
<!-- source-error: record:salesorder | subtotal | estimate_help_says_sales_order -->
