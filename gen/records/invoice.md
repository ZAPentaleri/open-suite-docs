# Invoice (`invoice`)
<!-- id: record:invoice -->

| Fact | Value |
|---|---|
| Internal ID | `invoice` |
| Category | transaction |
| Client scripts | yes |
| Server scripts | yes |
| Searchable | yes |
| Search data via | Transaction |
| Tags | `custom_fields_supported`, `scripting_full` |

## Record facts

- `record_actions`: `markforgrouping`, `removeinvoicefromgroup`
- `requires_feature`: `Invoice Groups` [`record_actions`, `real_name`]
- `returns_detail`: `notifications_and_response`
- `action_errors`: `no_permission_or_restricted`, `already_marked`, `partially_or_fully_paid`, `no_sales_order`
- `doc_inconsistency`: `says_invoice_cannot_be_copied_or_transformed`
- `record_actions`: `unmarkforgrouping`
- `ui_location`: `invoice_mark_unmark_for_grouping_remove_from_group_buttons`
- `action_errors`: `no_permission_or_restricted`, `completed_concurrently`, `partially_or_fully_paid`, `only_invoice_in_group` [`removeinvoicefromgroup`]
- `action_errors`: `no_permission_or_restricted`, `already_grouped`, `already_standalone` [`unmarkforgrouping`]
- `contains`: `invoice_group_record_id`, `invoice_list` [`markforgrouping_params`]
- `effect`: `mark_for_grouping_does_not_add_to_group`
- `effect`: `unmark_does_not_remove_from_group`
- `initialization_default_value`: `entity`, `entity_internal_id` (refs `N/record.create`, `N/record.copy`, `N/record.transform`, `N/record.load`)
- `script_types`: `client`, `server`
- `script_functions`: `create`, `update`, `copy`, `delete`, `search`, `transform`
- `body_only_scriptable_fields`: `estgrossprofit`, `estgrossprofitpercent`, `totalcostestimate`
- `before_load_runs_on_email_operation`: `true`
- `before_load_skipped_when`: `use_popup_for_main_transaction_email_button`
- `related` (refs `N/action`)
- `record_actions`: `markforgrouping`, `removeinvoicefromgroup`
- `field_requires`: `shippingtaxcode:per_line_taxes`
- `list_sublists`: `billable_expenses`, `billable_items`, `billable_time`
- `sublist_requires_feature`: `partners:multi_partner_management`
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
| `account` | select | Account | no | no |  |  | `purpose`: `receivables_account`; `allowed_values_listed_in`: `accounts_receivable_accounts`; `ui_effect`: `new_option_creates_record` |
| `althandlingcost` | currency | Handling Cost | no | no |  |  | `purpose`: `handling_cost`; `value_source`: `calculated_from_ship_method`; `value_source`: `shipping_item_rates` |
| `altshippingcost` | currency | Shipping Cost | no | no |  |  | `purpose`: `shipping_cost`; `value_source`: `calculated_from_ship_method`; `caveat`: `ups_real_time_splits_over_150_lb`; `value_source`: `shipping_item_rates` |
| `amountpaid` | currency | Amount Paid | no | no |  |  | `purpose`: `amount_paid`; `value_source`: `system_displayed` |
| `amountremaining` | currency | Amount Due | no | no |  |  | `purpose`: `amount_due`; `value_source`: `system_displayed` |
| `amountremainingtotalbox` | currency | Amount Due | no | no |  |  | `purpose`: `unpaid_total`; `value_source`: `system_displayed` |
| `asofdate` | date | As-Of Date | no | no |  |  | `purpose`: `as_of_date_for_charges`; `effect`: `selects_charges_added`; `caveat`: `no_effect_on_gl_or_due_date`; `value_examples`: `after_invoice_date_to_capture_later_charges` |
| `balance` | currency | Balance | no | no |  |  | `purpose`: `customer_open_invoices_total`; `value_source`: `system_displayed` |
| `billaddress` | address | Bill To | no | no |  |  | `purpose`: `billing_address`; `default_behavior`: `customer_default_billing_address`; `options`: `New`, `Custom`; `option_meanings`: `new:saved_to_entity`, `custom:transaction_only`; `can`: `edit_existing_address_with_edit_icon` |
| `billaddresslist` | select | Bill To Select | no | no |  |  | `purpose`: `billing_address_selector`; `options`: `New`, `Custom`; `option_meanings`: `new:saved_to_entity`, `custom:transaction_only`; `can`: `edit_existing_address_with_edit_icon` |
| `billingaccount` | select | Billing Account | no | no |  |  | `purpose`: `billing_account`; `allowed_values_listed_in`: `billing_account_records` |
| `class` | select | Class | no | no |  |  | `purpose`: `class`; `allowed_values_listed_in`: `class_records`; `ui_effect`: `new_option_creates_record` |
| `couponcode` | select | Coupon Code | no | no |  |  | `purpose`: `coupon_code`; `value_source`: `promotion` (when `multiple_use_coupon`) |
| `createdfrom` | select | Created From | no | no |  |  | `purpose`: `created_from_sales_order`; `value_source`: `system_displayed` |
| `currency` | select | Currency | yes | no |  |  | `purpose`: `transaction_currency`; `default_behavior`: `customer_primary_currency`; `requires_feature`: `Multi-Currency Customers` (when `choosing_transaction_currency`) [`real_name`]; `ui_effect`: `reprices_items_and_recalculates_tax_and_profit`; `caveat`: `custom_prices_not_repriced` |
| `customform` | select | Custom Form | yes | no |  |  | `purpose`: `custom_form`; `allowed_values_listed_in`: `custom_entry_forms`; `ui_effect`: `new_option_creates_record` |
| `deferredrevenue` | currency | Deferred Revenue | no | no |  |  | `purpose`: `deferred_revenue_amount` |
| `department` | select | Department | no | no |  |  | `purpose`: `department`; `allowed_values_listed_in`: `department_records`; `ui_effect`: `new_option_creates_record` |
| `discountamount` | currency | Disc. Amt. | no | no |  |  | `purpose`: `early_payment_discount_amount`; `value_source`: `terms` |
| `discountdate` | date | Disc. Date | no | no |  |  | `purpose`: `early_payment_discount_date`; `value_source`: `customer_terms` |
| `discountitem` | select | Discount | no | no |  |  | `purpose`: `discount_item`; `ui_effect`: `fills_rate_and_discount_total`; `allowed_values_listed_in`: `discount_items`; `default_behavior`: `no_discount` (when `blank`) |
| `discountrate` | rate | Rate | no | no |  |  | `purpose`: `discount_rate`; `value_source`: `discount_item_rate`; `accepts_forms`: `amount`, `percentage` |
| `discounttotal` | currency | Discount | no | no |  |  | `purpose`: `discount_total`; `value_source`: `calculated`; `caveat`: `taxable_discount_before_tax_else_after` |
| `duedate` | date | Due Date | no | no |  |  | `purpose`: `due_date`; `default_behavior`: `transaction_date` (when `blank`); `displayed_on`: `aging_reports` (when `no_due_date`) |
| `enddate` | date | End Date | no | no |  |  | `purpose`: `end_date`; `value_source`: `sales_order_start_and_end_dates` (when `billed_from_order`) |
| `entity` | select | Customer | yes | no |  |  | `purpose`: `invoice_customer`; `allowed_values_listed_in`: `customer_records`; `ui_effect`: `new_option_creates_record` |
| `estgrossprofit` | currency | Est. Gross Profit | no | no | yes |  | `purpose`: `estimated_gross_profit`; `value_source`: `revenue_minus_estimated_cost` |
| `estgrossprofitpercent` | percent | Est. Gross Profit Percent | no | no | yes |  | `purpose`: `estimated_gross_margin_percent`; `value_source`: `estimated_profit_divided_by_revenue` |
| `exchangerate` | currency2 | Exchange Rate | yes | no |  |  | `purpose`: `exchange_rate`; `can`: `update_currency_record_rate` |
| `excludecommission` | checkbox | Exclude Commissions | no | yes |  |  | `purpose`: `exclude_from_commissions`; `effect`: `includes_subordinate_transactions` |
| `expcostdiscamount` | currency | Amount | no | no |  |  | `purpose`: `discount_or_markup_amount`; `value_source`: `percent_times_selected_lines_total` (when `percent_rate`); `help_text_shared_with`: `invoice.itemcostdiscamount`, `invoice.timediscamount` |
| `expcostdiscount` | select | Discount/Markup | no | no |  |  | `purpose`: `discount_markup_for_billable_expenses`; `allowed_values_listed_in`: `discount_and_markup_items` |
| `expcostdiscprint` | checkbox | Print | no | no |  |  | `purpose`: `print_discount_on_invoice`; `help_text_shared_with`: `cashsale.itemcostdiscprint`, `cashsale.timediscprint`, `invoice.itemcostdiscprint`, `invoice.timediscprint` |
| `expcostdiscrate` | rate | Rate | no | no |  |  | `purpose`: `discount_markup_rate`; `default_behavior`: `selected_item_rate`; `accepts_forms`: `amount`, `percentage`; `can`: `override`; `help_text_shared_with`: `invoice.itemcostdiscrate`, `invoice.timediscrate` |
| `expcostdisctaxable` | checkbox | Taxable | no | no |  |  | `purpose`: `billable_expenses_taxable` |
| `expcosttaxrate1` | percent | Tax Rate | no | no |  |  | `purpose`: `expense_tax_rate`; `value_source`: `tax_code_gst_rate`; `allowed_values_listed_in`: `tax_code_records` |
| `fob` | text | FOB | no | no |  |  | `purpose`: `fob_ownership_location`; `value_examples`: `our_warehouse` |
| `forinvoicegrouping` | checkbox | For Invoice Grouping | no | no |  |  | `purpose`: `include_in_invoice_grouping` |
| `groupedto` | select | Grouped To | no | no | yes |  | `purpose`: `invoice_group_number` |
| `handlingcost` | currency | Handling Cost | no | no |  |  | `purpose`: `handling_cost`; `value_source`: `calculated_from_ship_method`; `value_source`: `shipping_item_rates` |
| `handlingtax1rate` | text | Handling Tax Rate | no | no |  |  | `purpose`: `handling_tax_rate`; `value_source`: `system_displayed` |
| `handlingtaxcode` | select | Handling Tax Code | no | no |  |  | `purpose`: `handling_tax_code` |
| `ismultishipto` | checkbox | Enable Item Line Shipping | no | no |  |  | `purpose`: `line_item_shipping`; `use_when`: `lines_cannot_ship_together` |
| `istaxable` | checkbox | Taxable | no | no |  |  | `purpose`: `invoiced_order_taxable`; `help_text_shared_with`: `invoice.item.istaxable` |
| `itemcostdiscamount` | currency | Amount | no | no |  |  | `purpose`: `discount_or_markup_amount`; `value_source`: `percent_times_selected_lines_total` (when `percent_rate`); `help_text_shared_with`: `invoice.expcostdiscamount`, `invoice.timediscamount` |
| `itemcostdiscount` | select | Discount/Markup | no | no |  |  | `purpose`: `discount_markup_for_billable_items`; `allowed_values_listed_in`: `discount_and_markup_items` |
| `itemcostdiscprint` | checkbox | Print | no | no |  |  | `purpose`: `print_discount_on_invoice`; `help_text_shared_with`: `cashsale.timediscprint`, `invoice.expcostdiscprint`, `invoice.timediscprint` |
| `itemcostdiscrate` | rate | Rate | no | no |  |  | `purpose`: `discount_markup_rate`; `default_behavior`: `selected_item_rate`; `accepts_forms`: `amount`, `percentage`; `can`: `override`; `help_text_shared_with`: `invoice.expcostdiscrate`, `invoice.timediscrate` |
| `itemcostdisctaxable` | checkbox | Apply Before Tax | no | no |  |  | `purpose`: `apply_discount_markup_before_tax`; `help_text_shared_with`: `invoice.timedisctaxable` |
| `itemcosttaxrate1` | percent | Tax Rate | no | no |  |  | `purpose`: `item_tax_rate`; `value_source`: `tax_code_gst_rate`; `allowed_values_listed_in`: `tax_code_records` |
| `leadsource` | select | Lead Source | no | no |  |  | `purpose`: `lead_source`; `allowed_values_listed_in`: `campaigns` (when `marketing_automation`) |
| `location` | select | Location | yes | no |  |  | `purpose`: `transaction_location`; `allowed_values_listed_in`: `location_records`; `ui_effect`: `new_option_creates_record` |
| `memo` | text | Memo | no | yes |  |  | `purpose`: `memo`; `displayed_on`: `registers`, `customer_center`; `value_examples`: `Suite A installation`, `Suite B installation`; `help_text_shared_with`: `invoice.expcost.memo` |
| `message` | textarea | Customer Message | no | no |  |  | `purpose`: `customer_message`; `value_source`: `selected_customer_message`; `can`: `edit_for_this_transaction` |
| `messagesel` | select | Select Message | no | no |  |  | `purpose`: `customer_message_selector`; `ui_effect`: `fills_message_field`; `allowed_values_listed_in`: `customer_message_list`; `value_examples`: `thank_you_for_your_business` |
| `nexus` | select | Nexus | no | no |  |  | `purpose`: `transaction_nexus`; `value_source`: `nexus_lookup`; `can`: `override_with_nexus_override` |
| `opportunity` | select | Opportunity | no | no |  |  | `purpose`: `opportunity`; `allowed_values_listed_in`: `opportunity_records` |
| `otherrefnum` | text | PO # | no | no |  |  | `purpose`: `customer_po_number` |
| `partner` | select | Partner | no | yes |  |  | `purpose`: `partner`; `default_behavior`: `customer_partner` |
| `postingperiod` | select | Posting Period | yes | no |  |  | `purpose`: `posting_period`; `cannot`: `post_to_closed_period`; `value_source`: `set_on_approval` (when `subject_to_approval`) |
| `promocode` | select | Promotion | no | no |  |  | `purpose`: `promotion`; `value_source`: `coupon_code_promotion` (when `single_use_coupon`); `configured_by`: `default_promotion_code_marketing_preference`; `constraint`: `partner_customers_use_partner_promotions_only`; `effect`: `partner_promotion_attributes_transaction_to_partner`; `allowed_values_listed_in`: `promotion_records` |
| `recognizedrevenue` | currency | Recognized Revenue | no | no |  |  | `purpose`: `cumulative_recognized_revenue` |
| `recurringbill` | checkbox | Recurring Bill | no | no |  |  | `purpose`: `recurring_invoice_for_billing_account` |
| `revenuestatus` | text | Revenue Status | no | no |  |  | `purpose`: `revenue_recognition_status`; `allowed_values`: `Pending`, `In Progress`, `Completed`; `option_meanings`: `pending:nothing_recognized_all_deferred`, `in_progress:partly_recognized`, `completed:fully_recognized_none_deferred` |
| `revreconrevcommitment` | checkbox | Rev Rec on Rev Commit. | no | no |  |  | `purpose`: `create_revenue_commitment`; `effect`: `commitment_replaces_invoice_or_credit_memo`; `effect`: `creates_revenue_recognition_schedules` |
| `saleseffectivedate` | date | Sales Effective Date | no | no |  |  | `purpose`: `sales_effective_date`; `effect`: `selects_commission_plan_and_historical_team` |
| `salesgroup` | select | Choose Team | no | no |  |  | `purpose`: `sales_team`; `allowed_values_listed_in`: `sales_team_groups` |
| `salesrep` | select | Sales Rep | no | yes |  |  | `purpose`: `sales_rep`; `default_behavior`: `customer_sales_rep`; `allowed_values_listed_in`: `employee_sales_reps`; `can`: `override_per_transaction` |
| `shipaddress` | address | Ship To | no | no |  |  | `purpose`: `shipping_address`; `default_behavior`: `customer_default_shipping_address`; `options`: `New`, `Custom`; `option_meanings`: `new:saved_to_entity`, `custom:transaction_only` |
| `shipaddresslist` | select | Ship To Select | no | no |  |  | `purpose`: `shipping_address_selector`; `options`: `New`, `Custom`; `option_meanings`: `new:saved_to_entity`, `custom:transaction_only` |
| `shipdate` | date | Ship Date | no | no |  |  | `purpose`: `ship_date` |
| `shipmethod` | select | Shipping Method | no | no |  |  | `purpose`: `ship_method`; `effect`: `fills_shipping_cost` (when `charge_for_shipping`); `allowed_values_listed_in`: `shipping_item_records`; `configured_by`: `charge_for_shipping_setting`; `requires`: `shipping_item_charge_details` (when `charge_for_shipping`) |
| `shippingcost` | currency | Shipping Cost | no | no |  |  | `purpose`: `shipping_cost`; `value_source`: `calculated_from_ship_method`; `caveat`: `ups_real_time_splits_over_150_lb`; `value_source`: `shipping_item_rates`; `help_text_shared_with`: `estimate.altshippingcost` |
| `shippingtax1rate` | text | Shipping Tax Rate | no | no |  |  | `purpose`: `shipping_tax_rate`; `value_source`: `system_displayed` |
| `shippingtaxcode` | select | Shipping Tax Code | no | no |  |  | `purpose`: `shipping_tax_code` |
| `source` | text | Source | no | no |  |  | `purpose`: `invoice_source`; `value_examples`: `program_or_software_name` |
| `startdate` | date | Start Date | no | no |  |  | `purpose`: `first_invoice_date`; `requires_feature`: `Advanced Billing` [`real_name`]; `value_source`: `sales_order_start_and_end_dates` (when `billed_from_order`) |
| `subsidiary` | select | Subsidiary | no | no |  |  | `purpose`: `subsidiary`; `value_source`: `system_displayed` |
| `subsidiarytaxregnum` | select | Tax Reg. Number | no |  |  |  | `purpose`: `subsidiary_tax_registration`; `value_source`: `nexus_lookup`; `can`: `override_with_nexus_override` |
| `subtotal` | currency | Subtotal | no | no |  |  | `purpose`: `subtotal_before_discount_shipping_tax`; `value_source`: `sum_of_item_lines` |
| `syncpartnerteams` | checkbox | Update Customer | no | no |  |  | `purpose`: `sync_sales_team_to_customer`; `true_means`: `customer_team_updated_from_transaction`; `help_text_shared_with`: `cashrefund.syncsalesteams`, `cashsale.syncsalesteams`, `creditmemo.syncsalesteams`, `estimate.syncsalesteams`, `invoice.syncsalesteams`, `returnauthorization.syncsalesteams`, `salesorder.syncsalesteams` |
| `syncsalesteams` | checkbox | Update Customer | no | no |  |  | `purpose`: `sync_sales_team_to_customer`; `true_means`: `customer_team_updated_from_transaction`; `help_text_shared_with`: `creditmemo.syncpartnerteams`, `invoice.syncpartnerteams`, `salesorder.syncpartnerteams` |
| `taxdetailsoverride` | checkbox | Tax Details Override | no |  |  |  | `purpose`: `override_tax_details`; `requires_permission`: `tax_details_tab_edit` |
| `taxitem` | select | Tax | no | no |  |  | `purpose`: `tax_item`; `default_behavior`: `customer_tax_item`; `allowed_values_listed_in`: `sales_tax_items`; `can`: `override_per_transaction` |
| `taxrate` | float | Tax % | no | no |  |  | `purpose`: `tax_rate`; `default_behavior`: `tax_item_rate`; `can`: `override_per_transaction`; `value_unit`: `percent` |
| `taxregoverride` | checkbox | Tax Registration Override | no |  |  |  | `purpose`: `override_nexus_and_tax_registration`; `requires_permission`: `tax_details_tab_edit` |
| `taxtotal` | currency | Tax Total | no | no |  |  | `purpose`: `tax_total`; `value_source`: `tax_rate_times_taxable_total` |
| `terms` | select | Terms | no | no |  |  | `purpose`: `billing_terms`; `allowed_values_listed_in`: `accounting_list_terms` |
| `timediscamount` | currency | Amount | no | no |  |  | `purpose`: `discount_or_markup_amount`; `value_source`: `percent_times_selected_lines_total` (when `percent_rate`); `help_text_shared_with`: `invoice.expcostdiscamount`, `invoice.itemcostdiscamount` |
| `timediscount` | select | Discount/Markup | no | no |  |  | `purpose`: `discount_markup_for_billable_time`; `allowed_values_listed_in`: `discount_and_markup_items` |
| `timediscprint` | checkbox | Print | no | no |  |  | `purpose`: `print_discount_on_invoice`; `help_text_shared_with`: `cashsale.itemcostdiscprint`, `invoice.expcostdiscprint`, `invoice.itemcostdiscprint` |
| `timediscrate` | rate | Rate | no | no |  |  | `purpose`: `discount_markup_rate`; `default_behavior`: `selected_item_rate`; `accepts_forms`: `amount`, `percentage`; `can`: `override`; `help_text_shared_with`: `invoice.expcostdiscrate`, `invoice.itemcostdiscrate` |
| `timedisctaxable` | checkbox | Apply Before Tax | no | no |  |  | `purpose`: `apply_discount_markup_before_tax`; `help_text_shared_with`: `invoice.itemcostdisctaxable` |
| `timetaxrate1` | percent | Tax Rate | no | no |  |  | `purpose`: `time_tax_rate`; `value_source`: `tax_code_gst_rate`; `allowed_values_listed_in`: `tax_code_records` |
| `tobeemailed` | checkbox | To Be E-mailed | no | no |  |  | `purpose`: `email_transaction`; `format`: `semicolon_separated_no_spaces`; `options`: `Yes`, `No`, `Respect Customer Preference` (when `bulk_processing`); `option_meanings`: `respect_customer_preference:customer_send_transactions_via` |
| `tobefaxed` | checkbox | To Be Faxed | no | no |  |  | `purpose`: `fax_transaction`; `options`: `Yes`, `No`, `Respect Customer Preference` (when `bulk_processing`); `option_meanings`: `respect_customer_preference:customer_send_transactions_via`; `requires`: `fax_service_set_up` |
| `tobeprinted` | checkbox | To Be Printed | no | yes |  |  | `purpose`: `print_transaction`; `options`: `Yes`, `No`, `Respect Customer Preference` (when `bulk_processing`); `option_meanings`: `respect_customer_preference:customer_send_transactions_via` |
| `total` | currency | Total | no | no |  |  | `purpose`: `total`; `value_source`: `lines_tax_and_shipping` |
| `totalcostestimate` | currency | Est. Extended Cost | no | no |  |  | `purpose`: `estimated_cost`; `value_source`: `estimated_rate_times_quantity` |
| `trackingnumbers` | text | Additional Tracking # | no | no |  |  | `purpose`: `tracking_numbers`; `ui_effect`: `ups_fedex_numbers_become_links` |
| `trandate` | date | Date | yes | no |  |  | `purpose`: `posting_date`; `default_behavior`: `current_date`; `displayed_on`: `registers` |
| `tranid` | text | Invoice # | no | no |  |  | `purpose`: `invoice_number`; `default_behavior`: `highest_number_plus_one`; `caveat`: `next_number_reverts_to_standard_pattern` |
| `tranisvsoebundle` | checkbox | Transaction Is VSOE Bundle | no | no |  |  | `purpose`: `transaction_is_vsoe_bundle` |
| `vsoeautocalc` | checkbox | Auto Calculate VSOE Allocation | no | no |  |  | `purpose`: `auto_calculate_vsoe_allocation`; `effect`: `allocated_total_is_bundle_revenue` |
| `approvalstatus` | select | Approval Status | no | no |  |  |  |
| `billingaddress` | summary |  | no | no |  |  |  |
| `billisresidential` | text | Shipping address is residential | no | no |  |  |  |
| `bulkprocsubmission` | select | Submission Id | no | no |  |  |  |
| `canhavestackable` | checkbox | Can have Stackable Promotions | no | no |  |  |  |
| `consolidatebalance` | currency | Consolidated Balance | no | no |  |  |  |
| `createddate` | datetime |  | no | no |  |  |  |
| `currencyname` | text | Currency | no | no |  |  |  |
| `currencysymbol` | text | Currency Symbol | no | no |  |  |  |
| `email` | emails |  | no | yes |  |  |  |
| `entitynexus` | select | Nexus | no | no |  |  |  |
| `entitytaxregnum` | select | Tax Reg. Number | no |  |  |  |  |
| `expcosttaxcode` | select | Tax Code | no | no |  |  |  |
| `externalid` | text | ExternalId | no | no |  |  |  |
| `giftcertapplied` | currency | Gift Certificate | no | no |  |  |  |
| `isbasecurrency` | checkbox | Base Currency | no | no |  |  |  |
| `itemcosttaxcode` | select | Tax Code | no | no |  |  |  |
| `lastmodifieddate` | datetime |  | no | no |  |  |  |
| `linkedtrackingnumbers` | text | Tracking # | no | no |  |  |  |
| `muccpromocodeinstance` | text | Promo Code | no | no |  |  |  |
| `nextapprover` | select | Next Approver | no | no |  |  |  |
| `overrideinstallments` | checkbox | Override Installments | no | no |  |  |  |
| `paylink` | text | Paylink | no | no |  |  |  |
| `promocodepluginimpl` | text | Promo Code | no | no |  |  |  |
| `returntrackingnumbers` | text | Return Tracking # | no | no |  |  |  |
| `saascommitmentenddate` | date | SaaS Metric Commitment End Date | no | no |  | marked_new |  |
| `saascommitmentstartdate` | date | SaaS Metric Commitment Start Date | no | no |  | marked_new |  |
| `shipisresidential` | text | Shipping address is residential | no | no |  |  |  |
| `shipoverride` | text |  | no | no |  |  |  |
| `shippingaddress` | summary |  | no | no |  |  |  |
| `shippingcostoverridden` | text | Shipping Cost Overridden | no | no |  |  |  |
| `status` | text | Status | no | no |  |  |  |
| `statusRef` | text |  | no | no |  |  |  |
| `taxpointdate` | date | Tax Point Date | no | no |  |  |  |
| `taxpointdateoverride` | checkbox | Override | no | no |  |  |  |
| `timetaxcode` | select | Tax Code | no | no |  |  |  |
| `unbilledorders` | currency | Unbilled Orders | no | no |  |  |  |

## Sublist `expcost` (Billable Expenses)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `memo` | text | Memo | no |  | `purpose`: `memo`; `displayed_on`: `registers`, `customer_center`; `value_examples`: `Suite A installation`, `Suite B installation`; `help_text_shared_with`: `invoice.memo` |
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
| `istaxable` | checkbox | Tax | no |  | `purpose`: `invoiced_order_taxable`; `help_text_shared_with`: `invoice.istaxable` |
| `account` | text |  | no |  |  |
| `amortizationperiod` | text |  | no |  |  |
| `amortizationtype` | text |  | no |  |  |
| `amount` | currency | Amount | no |  |  |
| `billvariancestatus` | text |  | no |  |  |
| `catchupperiod` | select |  | no |  |  |
| `chargetype` | text | Charge Type | no |  |  |
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
| `rate` | rate | Unit Price | no |  |  |
| `rateschedule` | text |  | no |  |  |
| `revrecenddate` | date | Rev. Rec. End Date | no |  |  |
| `revrecschedule` | select | Rev. Rec. Schedule | no |  |  |
| `revrecstartdate` | date | Rev. Rec. Start Date | no |  |  |
| `shipaddress` | select | Ship To | no |  |  |
| `shipcarrier` | select | Carrier | no |  |  |
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

## Sublist `installment` (Installments)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `amount` | poscurrency | Amount | yes |  |  |
| `amountdue` | currency | Amount Due | no |  |  |
| `duedate` | date | Due Date | yes |  |  |
| `paymentsapplied` | text | Payments Applied | no |  |  |
| `seqnum` | integer | Installment Number | no |  |  |
| `status` | text | Status | no |  |  |

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
| `creditmemo` | `customform`, `entity`, `invoices`, `account` |
| `customerpayment` | `customform` |
| `customsale` |  |
| `returnauthorization` | `customform`, `cf` |

## Suspected source errors

> [!SOURCE-ERROR] `says_invoice_cannot_be_copied_or_transformed`
<!-- source-error: record:invoice | - | says_invoice_cannot_be_copied_or_transformed -->
