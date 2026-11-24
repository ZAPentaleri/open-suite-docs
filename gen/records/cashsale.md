# Cash Sale (`cashsale`)
<!-- id: record:cashsale -->

| Fact | Value |
|---|---|
| Internal ID | `cashsale` |
| Category | transaction |
| Client scripts | yes |
| Server scripts | yes |
| Searchable | yes |
| Search data via | Transaction |
| Tags | `custom_fields_supported`, `scripting_full` |

## Record facts

- `initialization_default_value`: `entity`, `entity_internal_id` (refs `N/record.create`, `N/record.copy`, `N/record.transform`, `N/record.load`)
- `script_types`: `client`, `server`
- `script_functions`: `create`, `update`, `copy`, `delete`, `search`
- `unreadable_in_before_submit_for_external_roles`: `ccnumber`
- `before_load_runs_on_email_operation`: `true`
- `before_load_skipped_when`: `use_popup_for_main_transaction_email_button`
- `sublist_requires_feature`: `partners:multi_partner_management`
- `field_requires`: `shippingtaxcode:per_line_taxes`
- `list_sublists`: `billable_expenses`, `billable_items`, `billable_time`
- `purpose`: `sale_paid_at_delivery`
- `body_only_scriptable_fields`: `estgrossprofit`, `estgrossprofitpercent`, `totalcostestimate`
- `field_stored_only_when`: `isrecurringpayment:paymentmethod_credit_card`
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
| `account` | select |  | no | no |  |  | `purpose`: `deposit_account` |
| `althandlingcost` | currency | Handling Cost | no | no |  |  | `purpose`: `handling_cost`; `value_source`: `calculated_from_ship_method`; `value_source`: `shipping_item_rates` |
| `altshippingcost` | currency | Shipping Cost | no | no |  |  | `purpose`: `shipping_cost`; `value_source`: `calculated_from_ship_method`; `caveat`: `ups_real_time_splits_over_150_lb`; `value_source`: `shipping_item_rates` |
| `asofdate` | date | As-Of Date | no | no |  |  | `purpose`: `effective_date` |
| `authcode` | text | Auth. Code | no | no |  |  | `purpose`: `card_authorization_code`; `value_source`: `set_on_approval` (when `netsuite_merchant_account`); `use_when`: `card_validated_outside_netsuite` |
| `billaddress` | address | Bill To | no | no |  |  | `purpose`: `billing_address`; `default_behavior`: `customer_default_billing_address`; `options`: `New`, `Custom`; `option_meanings`: `new:saved_to_entity`, `custom:transaction_only`; `can`: `edit_existing_address_with_edit_icon` |
| `billaddresslist` | select | Bill To Select | no | no |  |  | `purpose`: `billing_address_selector`; `options`: `New`, `Custom`; `option_meanings`: `new:saved_to_entity`, `custom:transaction_only`; `can`: `edit_existing_address_with_edit_icon` |
| `ccapproved` | checkbox | Credit Card Approved | no | no |  |  | `purpose`: `card_approved_externally`; `use_when`: `card_processed_or_validated_outside`; `value_source`: `set_on_approval` (when `netsuite_merchant_account`); `value_source`: `checked_and_disabled_for_non_card_method`; `value_examples`: `gateway_back_office_or_bank_account_check` |
| `ccavsstreetmatch` | select | AVS Street Match | no | no |  |  | `purpose`: `avs_result_code`; `allowed_values`: `Y`, `N`, `X`; `option_meanings`: `y:matched`, `n:not_matched`, `x:not_supported_or_no_response`; `value_source`: `verisign_response`; `help_text_shared_with`: `cashsale.ccavszipmatch`, `customerdeposit.ccavszipmatch` |
| `ccavszipmatch` | select | AVS Zip Match | no | no |  |  | `purpose`: `avs_result_code`; `allowed_values`: `Y`, `N`, `X`; `option_meanings`: `y:matched`, `n:not_matched`, `x:not_supported_or_no_response`; `value_source`: `verisign_response`; `help_text_shared_with`: `cashsale.ccavsstreetmatch`, `customerdeposit.ccavsstreetmatch` |
| `ccexpiredate` | ccexpdate | Expires (MM/YYYY) | no | no |  |  | `purpose`: `card_expiration_date`; `format`: `MM/YYYY` |
| `ccname` | text | Name on Card | no | no |  |  | `purpose`: `cardholder_name`; `default_behavior`: `customer_default_card_holder`; `recommendation`: `fix_value_on_customer_record` |
| `ccnumber` | ccnumber | Credit Card # | no | no |  |  | `purpose`: `card_number`; `format`: `digits_without_spaces`; `caveat`: `visible_only_to_merchant_and_customer` |
| `ccsecuritycode` | text | CSC | no | no |  |  | `purpose`: `card_security_code`; `format`: `three_digits` |
| `ccsecuritycodematch` | select | CSC Match | no | no |  |  | `purpose`: `csc_result_code`; `allowed_values`: `Y`, `N`, `X`; `option_meanings`: `y:matched`, `n:not_matched`, `x:not_supported_or_no_response`; `value_source`: `verisign_response` |
| `ccstreet` | text | Card Street | no | no |  |  | `purpose`: `card_billing_street`; `value_source`: `customer_billing_address_street`; `recommendation`: `fix_value_on_customer_record` |
| `cczipcode` | text | Card Zip Code | no | no |  |  | `purpose`: `card_billing_postal_code`; `value_source`: `customer_billing_address_postal_code`; `recommendation`: `fix_value_on_customer_record` |
| `chargeit` | checkbox | Charge Credit Card | no | no |  |  | `purpose`: `charge_card_online`; `effect`: `funds_move_if_accepted`; `requires`: `netsuite_merchant_account` |
| `class` | select | Class | no | no |  |  | `purpose`: `transaction_class`; `allowed_values_listed_in`: `class_records`; `ui_effect`: `new_option_creates_record` |
| `couponcode` | select | Coupon Code | no | no |  |  | `purpose`: `coupon_code`; `value_source`: `promotion` (when `multiple_use_coupon`) |
| `createdfrom` | select | Created From | no | no |  |  | `purpose`: `created_from_sales_order`; `value_source`: `system_displayed` |
| `creditcard` | select | Credit Card Select | no | no |  |  | `purpose`: `stored_card_for_payment`; `allowed_values_listed_in`: `customer_stored_cards`; `ui_effect`: `new_option_creates_record` |
| `creditcardprocessor` | select | Credit Card Processor | no | no |  |  | `purpose`: `card_processor`; `allowed_values_listed_in`: `payment_processing_profiles` |
| `currency` | select | Currency | yes | no |  |  | `purpose`: `transaction_currency`; `default_behavior`: `customer_primary_currency`; `requires_feature`: `Multi-Currency Customers` (when `choosing_transaction_currency`) [`real_name`]; `ui_effect`: `reprices_items_and_recalculates_tax_and_profit`; `caveat`: `custom_prices_not_repriced` |
| `currencyname` | text | Currency | no | no |  |  | `purpose`: `customer_currency_name`; `effect`: `all_amounts_in_this_currency` |
| `customercode` | text | Customer Code | no | no |  |  | `purpose`: `card_customer_code`; `shown_only_when`: `display_customer_code_web_site_preference`; `requires`: `level_2_card_processing` |
| `customform` | select | Custom Form | yes | no |  |  | `purpose`: `custom_form`; `allowed_values_listed_in`: `custom_entry_forms`; `ui_effect`: `new_option_creates_record` |
| `debitcardissueno` | text | Issue No. | no | no |  |  | `purpose`: `debit_card_issue_number` |
| `deferredrevenue` | currency | Deferred Revenue | no | no |  |  | `purpose`: `deferred_revenue_amount` |
| `department` | select | Department | no | no |  |  | `purpose`: `transaction_department`; `allowed_values_listed_in`: `department_records`; `ui_effect`: `new_option_creates_record` |
| `discountitem` | select | Discount | no | no |  |  | `purpose`: `discount_item`; `ui_effect`: `fills_rate_and_discount_total`; `allowed_values_listed_in`: `discount_items`; `default_behavior`: `no_discount` (when `blank`) |
| `discountrate` | rate | Rate | no | no |  |  | `purpose`: `discount_rate`; `value_source`: `discount_item_rate`; `accepts_forms`: `amount`, `percentage` |
| `discounttotal` | currency | Discount | no | no |  |  | `purpose`: `discount_total`; `value_source`: `calculated`; `caveat`: `taxable_discount_before_tax_else_after` |
| `enddate` | date | End Date | no | no |  |  | `purpose`: `end_date` |
| `entity` | select | Customer | yes | no |  |  | `purpose`: `sale_customer`; `recommendation`: `select_project_not_parent`; `allowed_values_listed_in`: `customer_and_project_records`; `ui_effect`: `new_option_creates_record` |
| `entitytaxregnum` | select | Tax Reg. Number | no |  |  |  | `purpose`: `customer_tax_registration` |
| `estgrossprofit` | currency | Est. Gross Profit | no | no | yes |  | `purpose`: `estimated_gross_profit`; `value_source`: `revenue_minus_estimated_cost` |
| `estgrossprofitpercent` | percent | Est. Gross Profit Percent | no | no | yes |  | `purpose`: `estimated_gross_margin_percent`; `value_source`: `estimated_profit_divided_by_revenue` |
| `exchangerate` | currency2 | Exchange Rate | yes | no |  |  | `purpose`: `exchange_rate`; `can`: `update_currency_record_rate` |
| `excludecommission` | checkbox | Exclude Commissions | no | yes |  |  | `purpose`: `exclude_from_commissions`; `effect`: `includes_subordinate_transactions` |
| `expcostdiscamount` | currency | Amount | no | no |  |  | `purpose`: `discount_or_markup_amount`; `value_source`: `selected_discount_markup_item`; `help_text_shared_with`: `cashsale.itemcostdiscamount`, `cashsale.timediscamount` |
| `expcostdiscount` | select | Discount/Markup | no | no |  |  | `purpose`: `discount_markup_item_for_billable_expenses`; `ui_effect`: `fills_rate_and_total` |
| `expcostdiscprint` | checkbox | Print | no | no |  |  | `purpose`: `print_expense_discount_on_invoice` |
| `expcostdiscrate` | rate | Rate | no | no |  |  | `purpose`: `discount_markup_rate_for_billable_expenses`; `value_source`: `selected_discount_markup_item` |
| `expcostdisctaxable` | checkbox | Taxable | no | no |  |  | `purpose`: `expenses_taxable` |
| `expcosttaxcode` | select | Tax Code | no | no |  |  | `purpose`: `default_tax_code_for_billable_expenses`; `special_values`: `-Not Taxable-`; `can`: `edit_per_line` |
| `expcosttaxrate1` | percent | Tax Rate | no | no |  |  | `purpose`: `tax_rate_of_code`; `value_source`: `system_displayed`; `help_text_shared_with`: `cashsale.itemcosttaxrate1`, `cashsale.timetaxrate1` |
| `fob` | text | FOB | no | no |  |  | `purpose`: `fob_ownership_location`; `value_examples`: `our_warehouse` |
| `handlingcost` | currency | Handling Cost | no | no |  |  | `purpose`: `handling_cost`; `value_source`: `calculated_from_ship_method`; `value_source`: `shipping_item_rates` |
| `handlingtax1rate` | text | Handling Tax Rate | no | no |  |  | `purpose`: `tax_rate_of_code`; `value_source`: `system_displayed`; `help_text_shared_with`: `cashrefund.shippingtax1rate`, `cashsale.shippingtax1rate`, `creditmemo.shippingtax1rate` |
| `handlingtaxcode` | select | Handling Tax Code | no | no |  |  | `purpose`: `handling_tax_code` |
| `ignoreavs` | checkbox | Ignore AVS | no | no |  |  | `purpose`: `ignore_avs_result`; `true_means`: `payment_processed_despite_avs_mismatch` |
| `ignorecsc` | checkbox | Ignore CSC | no | no |  |  | `purpose`: `ignore_csc_result`; `true_means`: `payment_processed_despite_csc_mismatch` |
| `ismultishipto` | checkbox | Enable Item Line Shipping | no | no |  |  | `purpose`: `multiple_ship_to`; `effect`: `per_line_ship_address_and_method`; `default_behavior`: `customer_default_address_on_lines`; `options`: `New`, `Custom`; `option_meanings`: `new:saved_to_customer`, `custom:transaction_only` |
| `isrecurringpayment` | checkbox | Recurring Payment | no | no |  |  | `purpose`: `recurring_payment`; `effect`: `processes_after_card_expiry`; `use_when`: `billing_schedule_or_memorized_orders` |
| `istaxable` | checkbox | Taxable | no | no |  |  | `purpose`: `taxable`; `help_text_shared_with`: `cashsale.item.istaxable`, `salesorder.item.istaxable` |
| `itemcostdiscamount` | currency | Amount | no | no |  |  | `purpose`: `discount_or_markup_amount`; `value_source`: `selected_discount_markup_item`; `help_text_shared_with`: `cashsale.expcostdiscamount`, `cashsale.timediscamount` |
| `itemcostdiscount` | select | Discount/Markup | no | no |  |  | `purpose`: `discount_markup_item_for_billable_items`; `ui_effect`: `fills_rate_and_total` |
| `itemcostdiscprint` | checkbox | Print | no | no |  |  | `purpose`: `print_discount_on_invoice`; `help_text_shared_with`: `cashsale.timediscprint`, `invoice.expcostdiscprint`, `invoice.timediscprint` |
| `itemcostdiscrate` | rate | Rate | no | no |  |  | `purpose`: `discount_markup_rate_for_billable_items`; `value_source`: `selected_discount_markup_item` |
| `itemcostdisctaxable` | checkbox | Apply Before Tax | no | no |  |  | `purpose`: `apply_item_markup_before_tax`; `applies_to`: `all_billable_items` |
| `itemcosttaxcode` | select | Tax Code | no | no |  |  | `purpose`: `default_tax_code_for_billable_items`; `special_values`: `-Not Taxable-`; `can`: `edit_per_line` |
| `itemcosttaxrate1` | percent | Tax Rate | no | no |  |  | `purpose`: `tax_rate_of_code`; `value_source`: `system_displayed`; `help_text_shared_with`: `cashsale.expcosttaxrate1`, `cashsale.timetaxrate1` |
| `leadsource` | select | Lead Source | no | no |  |  | `purpose`: `lead_source`; `allowed_values_listed_in`: `campaigns` (when `marketing_automation`) |
| `location` | select | Location | yes | no |  |  | `purpose`: `transaction_location`; `allowed_values_listed_in`: `location_records`; `ui_effect`: `new_option_creates_record` |
| `memo` | text | Memo | no | yes |  |  | `purpose`: `memo`; `displayed_on`: `registers`; `value_kind`: `free_text` |
| `message` | textarea | Customer Message | no | no |  |  | `purpose`: `customer_message`; `value_source`: `selected_customer_message`; `can`: `edit_for_this_transaction` |
| `messagesel` | select | Select Message | no | no |  |  | `purpose`: `customer_message_selector`; `ui_effect`: `fills_message_field`; `allowed_values_listed_in`: `customer_message_list`; `value_examples`: `thank_you_for_your_business` |
| `nexus` | select | Nexus | no | no |  |  | `purpose`: `transaction_nexus`; `value_source`: `nexus_lookup`; `can`: `override_with_nexus_override` |
| `opportunity` | select | Opportunity | no | no |  |  | `purpose`: `opportunity`; `allowed_values_listed_in`: `opportunity_records` |
| `otherrefnum` | text | Check # | no | no |  |  | `purpose`: `customer_check_number`; `displayed_on`: `sales_receipt_form` |
| `partner` | select | Partner | no | yes |  |  | `purpose`: `partner`; `default_behavior`: `customer_partner` |
| `paymentmethod` | select | Payment Method | no | no |  |  | `purpose`: `payment_method`; `cannot`: `change` (when `google_checkout_order`); `allowed_values_listed_in`: `accounting_list_payment_method`; `value_source`: `google_checkout_account` (when `google_checkout_order`) |
| `paypalauthid` | text | Authorization ID | no | no |  |  | `purpose`: `paypal_authorization_id`; `value_source`: `paypal_response` |
| `paypalprocess` | checkbox | Process PayPal Payment | no | no |  |  | `purpose`: `process_via_paypal`; `false_means`: `paypal_fields_stored_not_sent`; `default_value`: `true` (when `paypal_express_checkout_order`) |
| `paypalstatus` | text | PayPal Status | no | no |  |  | `purpose`: `paypal_payment_status`; `value_source`: `paypal_response`; `allowed_values`: `None`, `Canceled-Reversal`, `Completed`, `Denied`, `Expired`, `Failed`, `Pending`, `Refunded`, `Reversed`, `Processed`, `Voided`; `caveat`: `failed_only_for_bank_account_payments`; `caveat`: `completed_only_value_for_point_of_sale`; `option_meanings`: `canceled_reversal:reversal_canceled_funds_returned`, `completed:funds_added`, `denied:payment_denied`, `expired:authorization_expired`, `failed:bank_payment_failed`, `refunded:payment_refunded`, `reversed:chargeback_funds_removed`, `processed:payment_accepted`, `voided:authorization_voided` |
| `paypaltranid` | text | PayPal Tran. ID | no | no |  |  | `purpose`: `paypal_transaction_id`; `value_source`: `paypal_response` |
| `pnrefnum` | text | P/N Ref. | no | no |  |  | `purpose`: `verisign_authentication_code`; `value_source`: `set_on_approval` |
| `postingperiod` | select | Posting Period | yes | no |  |  | `purpose`: `posting_period`; `cannot`: `post_to_closed_period` |
| `promocode` | select | Promotion | no | no |  |  | `purpose`: `promotion`; `value_source`: `coupon_code_promotion` (when `single_use_coupon`); `configured_by`: `default_promotion_code_marketing_preference`; `constraint`: `partner_customers_use_partner_promotions_only`; `effect`: `partner_promotion_attributes_transaction_to_partner`; `allowed_values_listed_in`: `promotion_records` |
| `recognizedrevenue` | currency | Recognized Revenue | no | no |  |  | `purpose`: `cumulative_recognized_revenue` |
| `revenuestatus` | text | Revenue Status | no | no |  |  | `purpose`: `revenue_recognition_status`; `allowed_values`: `Pending`, `In Progress`, `Completed`; `option_meanings`: `pending:nothing_recognized_all_deferred`, `in_progress:partly_recognized`, `completed:fully_recognized_none_deferred` |
| `revreconrevcommitment` | checkbox | Rev Rec on Rev Commit. | no | no |  |  | `purpose`: `create_revenue_commitment`; `effect`: `commitment_replaces_invoice_or_credit_memo`; `effect`: `creates_revenue_recognition_schedules` |
| `saleseffectivedate` | date | Sales Effective Date | no | no |  |  | `purpose`: `sales_effective_date`; `effect`: `selects_commission_plan_and_historical_team` |
| `salesgroup` | select | Choose Team | no | no |  |  | `purpose`: `sales_team`; `allowed_values_listed_in`: `sales_team_groups` |
| `salesrep` | select | Sales Rep | no | yes |  |  | `purpose`: `sales_rep`; `default_behavior`: `customer_sales_rep`; `allowed_values_listed_in`: `employee_sales_reps`; `can`: `override_per_transaction` |
| `shipaddress` | address | Ship To | no | no |  |  | `purpose`: `shipping_address`; `default_behavior`: `customer_default_shipping_address`; `can`: `choose_other_or_new_address_in_selector` |
| `shipaddresslist` | select | Ship To Select | no | no |  |  | `purpose`: `shipping_address_selector`; `ui_effect`: `new_option_creates_record` |
| `shipdate` | date | Ship Date | no | no |  |  | `purpose`: `ship_date` |
| `shipmethod` | select | Ship Via | no | no |  |  | `purpose`: `ship_method`; `effect`: `fills_shipping_cost` (when `charge_for_shipping`); `allowed_values_listed_in`: `shipping_item_records`; `configured_by`: `charge_for_shipping_setting`; `requires`: `shipping_item_charge_details` (when `charge_for_shipping`) |
| `shippingcost` | currency | Shipping Cost | no | no |  |  | `purpose`: `shipping_cost`; `value_source`: `calculated_from_ship_method`; `caveat`: `ups_real_time_splits_over_150_lb`; `value_source`: `shipping_item_rates` |
| `shippingtax1rate` | text | Shipping Tax Rate | no | no |  |  | `purpose`: `tax_rate_of_code`; `value_source`: `system_displayed`; `help_text_shared_with`: `cashrefund.handlingtax1rate`, `cashsale.handlingtax1rate`, `creditmemo.handlingtax1rate` |
| `shippingtaxcode` | select | Shipping Tax Code | no | no |  |  | `purpose`: `shipping_tax_code` |
| `startdate` | date | Start Date | no | no |  |  | `purpose`: `first_bill_date`; `requires_feature`: `Advanced Billing` [`real_name`]; `value_source`: `sales_order_start_and_end_dates` (when `billed_from_order`) |
| `subsidiary` | select | Subsidiary | no | no |  |  | `purpose`: `subsidiary`; `value_source`: `system_displayed` |
| `subsidiarytaxregnum` | select | Tax Reg. Number | no |  |  |  | `purpose`: `subsidiary_tax_registration`; `value_source`: `nexus_lookup`; `can`: `override_with_nexus_override` |
| `subtotal` | currency | Subtotal | no | no |  |  | `purpose`: `subtotal_before_discount_shipping_tax`; `value_source`: `sum_of_item_lines`; `doc_inconsistency`: `cash_sale_help_says_sales_order` |
| `syncsalesteams` | checkbox | Update Customer | no | no |  |  | `purpose`: `sync_sales_team_to_customer`; `true_means`: `customer_team_updated_from_transaction`; `help_text_shared_with`: `creditmemo.syncpartnerteams`, `invoice.syncpartnerteams`, `salesorder.syncpartnerteams` |
| `taxdetailsoverride` | checkbox | Tax Details Override | no |  |  |  | `purpose`: `override_tax_details`; `requires_permission`: `tax_details_tab_edit` |
| `taxitem` | select | Tax | no | no |  |  | `purpose`: `tax_item`; `default_behavior`: `customer_tax_item`; `value_kind`: `single_rate_single_agency_tax`; `allowed_values_listed_in`: `sales_tax_items`; `can`: `override_per_transaction` |
| `taxpointdate` | date | Tax Point Date | no | no |  |  | `purpose`: `tax_point_date`; `value_kind`: `date_determining_taxability` |
| `taxrate` | float | Tax % | no | no |  |  | `purpose`: `tax_rate`; `default_behavior`: `tax_item_rate`; `can`: `override_per_transaction`; `value_unit`: `percent` |
| `taxregoverride` | checkbox | Tax Registration Override | no |  |  |  | `purpose`: `override_nexus_and_tax_registration`; `requires_permission`: `tax_details_tab_edit` |
| `taxtotal` | currency | Tax Total | no | no |  |  | `purpose`: `tax_total`; `value_source`: `tax_rate_times_taxable_total` |
| `timediscamount` | currency | Amount | no | no |  |  | `purpose`: `discount_or_markup_amount`; `value_source`: `selected_discount_markup_item`; `help_text_shared_with`: `cashsale.expcostdiscamount`, `cashsale.itemcostdiscamount` |
| `timediscount` | select | Discount/Markup | no | no |  |  | `purpose`: `discount_markup_item_for_billable_time`; `ui_effect`: `fills_rate_and_total` |
| `timediscprint` | checkbox | Print | no | no |  |  | `purpose`: `print_discount_on_invoice`; `help_text_shared_with`: `cashsale.itemcostdiscprint`, `invoice.expcostdiscprint`, `invoice.itemcostdiscprint` |
| `timediscrate` | rate | Rate | no | no |  |  | `purpose`: `discount_markup_rate_for_billable_time`; `value_source`: `selected_discount_markup_item` |
| `timedisctaxable` | checkbox | Apply Before Tax | no | no |  |  | `purpose`: `apply_time_discount_before_tax` |
| `timetaxcode` | select | Tax Code | no | no |  |  | `purpose`: `default_tax_code_for_billable_time`; `special_values`: `-Not Taxable-`; `can`: `edit_per_line` |
| `timetaxrate1` | percent | Tax Rate | no | no |  |  | `purpose`: `tax_rate_of_code`; `value_source`: `system_displayed`; `help_text_shared_with`: `cashsale.expcosttaxrate1`, `cashsale.itemcosttaxrate1` |
| `tobeemailed` | checkbox | To Be E-mailed | no | no |  |  | `purpose`: `email_transaction`; `format`: `semicolon_separated_no_spaces` |
| `tobefaxed` | checkbox | To Be Faxed | no | no |  |  | `purpose`: `fax_transaction`; `requires`: `fax_service_set_up`; `ui_effect`: `fax_number_shown_beside_checkbox` |
| `tobeprinted` | checkbox | To Be Printed | no | yes |  |  | `purpose`: `print_later`; `true_means`: `queued_for_printing` |
| `total` | currency | Total | no | no |  |  | `purpose`: `total`; `value_source`: `lines_tax_and_shipping` |
| `totalcostestimate` | currency | Est. Extended Cost | no | no |  |  | `purpose`: `estimated_cost`; `value_source`: `estimated_rate_times_quantity` |
| `trackingnumbers` | text | Additional Tracking # | no | no |  |  | `purpose`: `tracking_numbers`; `ui_effect`: `ups_fedex_numbers_become_links` |
| `trandate` | date | Date | yes | no |  |  | `purpose`: `transaction_date`; `default_behavior`: `current_date` |
| `tranid` | text | Sale # | no | no |  |  | `purpose`: `sale_number`; `accepts_forms`: `non_numeric_characters`; `default_behavior`: `highest_number_plus_one`; `max_length`: `45` characters; `caveat`: `next_number_reverts_to_standard_pattern` |
| `tranisvsoebundle` | checkbox | Transaction Is VSOE Bundle | no | no |  |  | `purpose`: `transaction_is_vsoe_bundle` |
| `undepfunds` | radio | Undep. Funds | no | no |  |  | `purpose`: `undeposited_funds_or_account`; `use_when`: `undeposited_for_cash_or_check`; `options`: `Undep. Funds`, `Account`; `requires`: `make_deposits_later` (when `undeposited_funds`); `use_when`: `account_for_card_payments` |
| `validfrom` | ccvalidfrom | Valid From / Start Date (MM/YYYY) | no | no |  |  | `purpose`: `card_valid_from`; `format`: `MM/YYYY` |
| `vsoeautocalc` | checkbox | Auto Calculate VSOE Allocation | no | no |  |  | `purpose`: `auto_calculate_vsoe_allocation`; `effect`: `allocated_total_is_bundle_revenue` |
| `allowemptycards` | checkbox | Credits | no | no |  |  |  |
| `billingaccount` | select | Billing Account | no | no |  |  |  |
| `billingaddress` | summary |  | no | no |  |  |  |
| `billisresidential` | text | Shipping address is residential | no | no |  |  |  |
| `bulkprocsubmission` | select | Submission Id | no | no |  |  |  |
| `canhavestackable` | checkbox | Can have Stackable Promotions | no | no |  |  |  |
| `cchold` | text | Detail | no | no |  |  |  |
| `ccholdetails` | textarea | Detail | no | no |  |  |  |
| `cciavsmatch` | select | IAVS Match | no | no |  |  |  |
| `ccispurchasecardbin` | checkbox | Purchase Card BIN | no | no |  |  |  |
| `ccprocessoraccount` | text | Credit Card Processing Profile | no | no |  |  |  |
| `checknumber` | text | Check # | no | no |  |  |  |
| `cleared` | checkbox | Cleared | no |  |  |  |  |
| `cleareddate` | date | Date Cleared | no |  |  |  |  |
| `createddate` | datetime |  | no | no |  |  |  |
| `currencysymbol` | text | Currency Symbol | no | no |  |  |  |
| `dynamicdescriptor` | text | Soft Descriptor | no | no |  |  |  |
| `email` | emails |  | no | yes |  |  |  |
| `entitynexus` | select | Nexus | no | no |  |  |  |
| `externalid` | text | ExternalId | no | no |  |  |  |
| `giftcertapplied` | currency | Gift Certificate | no | no |  |  |  |
| `handlingmode` | select | Handling Mode | no | no |  |  |  |
| `inputauthcode` | text | Input Auth. Code | no | no |  |  |  |
| `inputpnrefnum` | text |  | no | no |  |  |  |
| `inputreferencecode` | text | Input P/N Ref. | yes | no |  |  |  |
| `isbasecurrency` | checkbox | Base Currency | no | no |  |  |  |
| `ispurchasecard` | text | Override Hold | no | no |  |  |  |
| `lastmodifieddate` | datetime |  | no | no |  |  |  |
| `linkedtrackingnumbers` | text | Tracking # | no | no |  |  |  |
| `muccpromocodeinstance` | text | Promo Code | no | no |  |  |  |
| `outputauthcode` | text | Auth. Code | no | no |  |  |  |
| `outputreferencecode` | text | P/N Ref. | no | no |  |  |  |
| `overridehold` | checkbox | Override Hold | no | no |  |  |  |
| `overrideholdchecked` | checkbox | Override Hold | no | no |  |  |  |
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
| `recurringbill` | checkbox | Recurring Bill | no | no |  |  |  |
| `returntrackingnumbers` | text | Return Tracking # | no | no |  |  |  |
| `saascommitmentenddate` | date | SaaS Metric Commitment End Date | no | no |  | marked_new |  |
| `saascommitmentstartdate` | date | SaaS Metric Commitment Start Date | no | no |  | marked_new |  |
| `shipisresidential` | text | Shipping address is residential | no | no |  |  |  |
| `shipoverride` | text |  | no | no |  |  |  |
| `shippingaddress` | summary |  | no | no |  |  |  |
| `shippingcostoverridden` | text | Shipping Cost Overridden | no | no |  |  |  |
| `softdescriptor` | text | Soft Descriptor | no | no |  |  |  |
| `source` | text | Source | no | no |  |  |  |
| `status` | text | Status | no | no |  |  |  |
| `statusRef` | text |  | no | no |  |  |  |
| `syncpartnerteams` | checkbox | Update Customer | no | no |  |  |  |
| `taxpointdateoverride` | checkbox | Override | no | no |  |  |  |
| `threedstatuscode` | text | Payer Authentication Status | no | no |  |  |  |

## Sublist `expcost` (Billable Expenses)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `memo` | text | Memo | no |  | `purpose`: `memo`; `displayed_on`: `registers`; `doc_inconsistency`: `expense_line_memo_help_describes_transaction`; `value_kind`: `free_text` |
| `amortizationperiod` | text |  | no |  |  |
| `amortizationtype` | text |  | no |  |  |
| `amount` | currency | Bill Amount | no |  |  |
| `apply` | checkbox | Apply | no |  |  |
| `billeddate` | date | Date | no |  |  |
| `category` | text |  | no |  |  |
| `doc` | integer |  | no |  |  |
| `employee` | text |  | no |  |  |
| `job` | text |  | no |  |  |
| `line` | integer |  | no |  |  |
| `location` | select | Location | no |  |  |
| `originalamount` | currency | Original Amount | no |  |  |
| `revrecenddate` | date | Rev. Rec. End Date | no |  |  |
| `revrecschedule` | select | Rev. Rec. Schedule | no |  |  |
| `revrecstartdate` | date | Rev. Rec. Start Date | no |  |  |
| `taxable` | text |  | no |  |  |
| `taxcode` | select | Tax Code | yes |  |  |
| `taxrate1` | percent | Tax Rate | no |  |  |
| `url` | text |  | no |  |  |

## Sublist `item` (Items)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `istaxable` | checkbox | Tax | no |  | `purpose`: `taxable`; `help_text_shared_with`: `cashsale.istaxable`, `salesorder.istaxable` |
| `amortizationperiod` | text |  | no |  |  |
| `amortizationtype` | text |  | no |  |  |
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
| `giftcertfrom` | text | From | no |  |  |
| `giftcertmessage` | textarea | Gift Message | no |  |  |
| `giftcertrecipientemail` | email | Recipient Email | no |  |  |
| `giftcertrecipientname` | text | Recipient Name | no |  |  |
| `id` | text |  | no |  |  |
| `inventorydetail` | summary | Inventory Detail | no |  |  |
| `isvsoebundle` | text |  | no |  |  |
| `item` | select | Item | yes |  |  |
| `itemsubtype` | text |  | no |  |  |
| `itemtype` | text |  | no |  |  |
| `job` | select | Project | no |  |  |
| `licensecode` | textarea | License Code | no |  |  |
| `line` | text |  | no |  |  |
| `linenumber` | integer |  | no |  |  |
| `matrixtype` | text |  | no |  |  |
| `options` | namevaluelist | Options | no |  |  |
| `price` | select | Price Level | no |  |  |
| `printitems` | text |  | no |  |  |
| `quantity` | float | Quantity | no |  |  |
| `quantityavailable` | float |  | no |  |  |
| `quantityremaining` | float | Back Ordered | no |  |  |
| `rate` | rate | Rate | no |  |  |
| `rateschedule` | text |  | no |  |  |
| `revrecenddate` | date | Rev. Rec. End Date | no |  |  |
| `revrecschedule` | select | Rev. Rec. Schedule | no |  |  |
| `revrecstartdate` | date | Rev. Rec. Start Date | no |  |  |
| `shipaddress` | select | Ship To | no |  |  |
| `shipcarrier` | select | Shipping Carrier | no |  |  |
| `shipmethod` | select | Ship Via | no |  |  |
| `subscriptionline` | select | Subscription Line | no |  |  |
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

## Sublist `itemcost` (Billable Items)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `amortizationperiod` | text |  | no |  |  |
| `amortizationtype` | text |  | no |  |  |
| `amount` | currency | Total | no |  |  |
| `apply` | checkbox | Apply | no |  |  |
| `billeddate` | date | Date | no |  |  |
| `binnumbers` | textarea | Bin Numbers | no |  |  |
| `cost` | currency2 | Rate | no |  |  |
| `doc` | integer |  | no |  |  |
| `item` | text |  | no |  |  |
| `itemcostcount` | text | Qty | no |  |  |
| `job` | text |  | no |  |  |
| `line` | integer |  | no |  |  |
| `location` | select | Location | no |  |  |
| `memo` | text | Description | no |  |  |
| `options` | namevaluelist | Options | no |  |  |
| `rateschedule` | text |  | no |  |  |
| `revrecenddate` | date | Rev. Rec. End Date | no |  |  |
| `revrecschedule` | select | Rev. Rec. Schedule | no |  |  |
| `revrecstartdate` | date | Rev. Rec. Start Date | no |  |  |
| `serialnumbers` | textarea | Serial/Lot Numbers | no |  |  |
| `taxable` | text |  | no |  |  |
| `taxcode` | select | Tax Code | yes |  |  |
| `taxrate1` | percent | Tax Rate | no |  |  |
| `unit` | text |  | no |  |  |
| `url` | text |  | no |  |  |

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

## Sublist `time` (Billable Time)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `amortizationperiod` | text |  | no |  |  |
| `amortizationtype` | text |  | no |  |  |
| `amount` | currency | Amount | no |  |  |
| `apply` | checkbox | Apply | no |  |  |
| `billeddate` | date | Date | no |  |  |
| `doc` | text |  | no |  |  |
| `item` | text | Item | no |  |  |
| `job` | text |  | no |  |  |
| `memo` | text | Description | no |  |  |
| `rate` | currency2 | Rate | no |  |  |
| `rateschedule` | text |  | no |  |  |
| `revrecenddate` | date | Rev. Rec. End Date | no |  |  |
| `revrecschedule` | select | Rev. Rec. Schedule | no |  |  |
| `revrecstartdate` | date | Rev. Rec. Start Date | no |  |  |
| `taxable` | text |  | no |  |  |
| `taxcode` | select | Tax Code | yes |  |  |
| `taxrate1` | percent | Tax Rate | no |  |  |
| `unit` | text |  | no |  |  |
| `url` | text |  | no |  |  |

## Tabs

| Tab ID | Label |
|---|---|
| `address` | Address |
| `custom` | Custom |
| `history` | History |
| `items` | Items |
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
| `cashrefund` | `customform` |
| `customsale` |  |
| `returnauthorization` | `customform`, `cf` |

## Suspected source errors

> [!SOURCE-ERROR] `cash_sale_help_says_sales_order` (on `subtotal`)
<!-- source-error: record:cashsale | subtotal | cash_sale_help_says_sales_order -->

> [!SOURCE-ERROR] `expense_line_memo_help_describes_transaction` (on `expcost.memo`)
<!-- source-error: record:cashsale | expcost.memo | expense_line_memo_help_describes_transaction -->
