# Cash Refund (`cashrefund`)
<!-- id: record:cashrefund -->

| Fact | Value |
|---|---|
| Internal ID | `cashrefund` |
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
- `body_only_scriptable_fields`: `estgrossprofit`, `estgrossprofitpercent`, `totalcostestimate`
- `before_load_runs_on_email_operation`: `true`
- `before_load_skipped_when`: `use_popup_for_main_transaction_email_button`
- `sublist_requires_feature`: `partners:multi_partner_management`
- `purpose`: `refund_of_immediately_paid_sale`
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
| `account` | select | Account | no | no |  |  | `purpose`: `refund_from_account` |
| `althandlingcost` | currency | Handling Cost | no | no |  |  | `purpose`: `handling_charged` |
| `altshippingcost` | currency | Shipping Cost | no | no |  |  | `purpose`: `shipping_charged` |
| `billaddress` | address | Bill To | no | no |  |  | `purpose`: `billing_address`; `default_behavior`: `customer_default_billing_address`; `can`: `choose_other_or_new_address_in_selector` |
| `billaddresslist` | select | Bill To Select | no | no |  |  | `purpose`: `billing_address_selector`; `ui_effect`: `new_option_creates_record` |
| `ccapproved` | checkbox | Credit Card Approved | no | no |  |  | `purpose`: `card_approved_externally`; `use_when`: `card_processed_outside_netsuite`; `value_source`: `set_on_approval` (when `netsuite_merchant_account`); `value_examples`: `card_swipe_terminal` |
| `ccexpiredate` | ccexpdate | Expires (MM/YYYY) | no | no |  |  | `purpose`: `card_expiration_date`; `format`: `MM/YYYY` |
| `ccname` | text | Name on Card | no | no |  |  | `purpose`: `cardholder_name`; `default_behavior`: `customer_default_card_holder`; `recommendation`: `fix_value_on_customer_record` |
| `ccnumber` | ccnumber | Credit Card # | no | no |  |  | `purpose`: `card_number`; `format`: `digits_without_spaces`; `caveat`: `visible_only_to_merchant_and_customer` |
| `ccstreet` | text | Card Street | no | no |  |  | `purpose`: `card_billing_street`; `value_source`: `customer_billing_address_street`; `recommendation`: `fix_value_on_customer_record` |
| `cczipcode` | text | Card Zip Code | no | no |  |  | `purpose`: `card_billing_postal_code`; `value_source`: `customer_billing_address_postal_code`; `recommendation`: `fix_value_on_customer_record` |
| `chargeit` | checkbox | Process Credit Card | no | no |  |  | `purpose`: `process_card_refund`; `requires`: `netsuite_merchant_account` |
| `class` | select | Class | no | no |  |  | `purpose`: `transaction_class`; `allowed_values_listed_in`: `class_records`; `ui_effect`: `new_option_creates_record` |
| `couponcode` | select | Coupon Code | no | no |  |  | `purpose`: `coupon_code`; `value_source`: `promotion` (when `multiple_use_coupon`) |
| `createdfrom` | select | Created From | no | no |  |  | `purpose`: `created_from_return_authorization`; `value_source`: `system_displayed` |
| `creditcard` | select | Credit Card Select | no | no |  |  | `purpose`: `stored_card_for_refund`; `allowed_values_listed_in`: `customer_stored_cards`; `ui_effect`: `new_option_creates_record` |
| `creditcardprocessor` | select | Credit Card Processor | no | no |  |  | `purpose`: `card_processor`; `allowed_values_listed_in`: `payment_processing_profiles` |
| `currency` | select | Currency | yes | no |  |  | `purpose`: `transaction_currency`; `default_behavior`: `customer_primary_currency`; `requires_feature`: `Multi-Currency Customers` (when `choosing_transaction_currency`) [`real_name`]; `ui_effect`: `reprices_items_and_recalculates_tax_and_profit`; `caveat`: `custom_prices_not_repriced` |
| `currencyname` | text | Currency | no | no |  |  | `purpose`: `customer_currency_name`; `effect`: `all_amounts_in_this_currency` |
| `customform` | select | Custom Form | yes | no |  |  | `purpose`: `custom_form`; `allowed_values_listed_in`: `custom_entry_forms`; `ui_effect`: `new_option_creates_record` |
| `debitcardissueno` | text | Issue No. | no | no |  |  | `purpose`: `debit_card_issue_number` |
| `deferredrevenue` | currency | Deferred Revenue | no | no |  |  | `purpose`: `deferred_revenue_amount` |
| `department` | select | Department | no | no |  |  | `purpose`: `transaction_department`; `allowed_values_listed_in`: `department_records`; `ui_effect`: `new_option_creates_record` |
| `discountitem` | select | Discount | no | no |  |  | `purpose`: `discount_item`; `ui_effect`: `fills_rate_and_discount_total`; `allowed_values_listed_in`: `discount_items`; `default_behavior`: `no_discount` (when `blank`); `use_when`: `discount_used_on_original_sale` |
| `discountrate` | rate | Rate | no | no |  |  | `purpose`: `discount_rate`; `value_source`: `discount_item_rate`; `accepts_forms`: `amount`, `percentage` |
| `discounttotal` | currency | Discount | no | no |  |  | `purpose`: `discount_total`; `value_source`: `calculated`; `caveat`: `taxable_discount_before_tax_else_after` |
| `entity` | select | Customer | yes | no |  |  | `purpose`: `refund_customer`; `recommendation`: `select_job_not_parent`; `allowed_values_listed_in`: `customer_and_project_records`; `ui_effect`: `new_option_creates_record` |
| `entitytaxregnum` | select | Tax Reg. Number | no |  |  |  | `purpose`: `customer_tax_registration` |
| `estgrossprofit` | currency | Est. Gross Profit | no | no | yes |  | `purpose`: `estimated_gross_profit`; `value_source`: `revenue_minus_estimated_cost` |
| `estgrossprofitpercent` | percent | Est. Gross Profit Percent | no | no | yes |  | `purpose`: `estimated_gross_margin_percent`; `value_source`: `estimated_profit_divided_by_revenue` |
| `exchangerate` | currency2 | Exchange Rate | yes | no |  |  | `purpose`: `exchange_rate`; `can`: `update_currency_record_rate` |
| `excludecommission` | checkbox | Exclude Commissions | no | yes |  |  | `purpose`: `exclude_from_commissions`; `effect`: `includes_subordinate_transactions` |
| `handlingcost` | currency | Handling Cost | no | no |  |  | `purpose`: `handling_charged` |
| `handlingtax1rate` | text | Tax Rate | no | no |  |  | `purpose`: `tax_rate_of_code`; `value_source`: `system_displayed`; `help_text_shared_with`: `cashrefund.shippingtax1rate`, `cashsale.shippingtax1rate`, `creditmemo.shippingtax1rate` |
| `handlingtaxcode` | select | Handling Tax Code | no | no |  |  | `purpose`: `handling_tax_code` |
| `istaxable` | checkbox | Taxable | no | no |  |  | `purpose`: `original_order_taxable`; `help_text_shared_with`: `cashrefund.item.istaxable` |
| `leadsource` | select | Lead Source | no | no |  |  | `purpose`: `lead_source`; `allowed_values_listed_in`: `campaigns` (when `marketing_automation`); `allowed_values_listed_in`: `crm_list_lead_source` (when `no_marketing_automation`) |
| `location` | select | Location | yes | no |  |  | `purpose`: `transaction_location`; `allowed_values_listed_in`: `location_records`; `ui_effect`: `new_option_creates_record` |
| `memo` | text | Memo | no | yes |  |  | `purpose`: `memo`; `displayed_on`: `registers`; `value_kind`: `free_text` |
| `message` | textarea | Customer Message | no | no |  |  | `purpose`: `customer_message`; `displayed_on`: `printed_emailed_faxed_transaction` |
| `nexus` | select | Nexus | no | no |  |  | `purpose`: `transaction_nexus`; `value_source`: `nexus_lookup`; `can`: `override_with_nexus_override` |
| `otherrefnum` | text | Check # | no | no |  |  | `purpose`: `original_check_number` |
| `partner` | select | Partner | no | yes |  |  | `purpose`: `partner`; `default_behavior`: `customer_partner` |
| `paymentmethod` | select | Payment Method | no | no |  |  | `purpose`: `original_payment_method` |
| `paypalauthid` | text | Authorization ID | no | no |  |  | `purpose`: `paypal_authorization_id`; `value_source`: `paypal_response` |
| `paypalprocess` | checkbox | Process PayPal Payment | no | no |  |  | `purpose`: `process_via_paypal`; `false_means`: `paypal_fields_stored_not_sent`; `default_value`: `true` (when `paypal_express_checkout_order`) |
| `paypalstatus` | text | PayPal Status | no | no |  |  | `purpose`: `paypal_payment_status`; `value_source`: `paypal_response`; `allowed_values`: `None`, `Canceled-Reversal`, `Completed`, `Denied`, `Expired`, `Failed`, `Pending`, `Refunded`, `Reversed`, `Processed`, `Voided`; `caveat`: `failed_only_for_bank_account_payments`; `caveat`: `completed_only_value_for_point_of_sale`; `option_meanings`: `canceled_reversal:reversal_canceled_funds_returned`, `completed:funds_added`, `denied:payment_denied`, `expired:authorization_expired`, `failed:bank_payment_failed`, `refunded:payment_refunded`, `reversed:chargeback_funds_removed`, `processed:payment_accepted`, `voided:authorization_voided` |
| `paypaltranid` | text | PayPal Tran. ID | no | no |  |  | `purpose`: `paypal_transaction_id`; `value_source`: `paypal_response` |
| `pnrefnum` | text | P/N Ref. | no | no |  |  | `purpose`: `verisign_authentication_code`; `value_source`: `set_on_approval` |
| `postingperiod` | select | Posting Period | yes | no |  |  | `purpose`: `posting_period`; `cannot`: `post_to_closed_period` |
| `promocode` | select | Promotion | no | no |  |  | `purpose`: `promotion_code`; `constraint`: `partner_customers_use_partner_promotions_only`; `effect`: `partner_promotion_attributes_transaction_to_partner`; `allowed_values_listed_in`: `promotion_code_records` |
| `recognizedrevenue` | currency | Recognized Revenue | no | no |  |  | `purpose`: `cumulative_recognized_revenue` |
| `refundcheck` | checkbox | Create Check | no | no |  |  | `purpose`: `refund_by_check`; `false_means`: `cash_or_card_refund` |
| `revenuestatus` | text | Revenue Status | no | no |  |  | `purpose`: `revenue_recognition_status`; `allowed_values`: `Pending`, `In Progress`, `Completed`; `option_meanings`: `pending:nothing_recognized_all_deferred`, `in_progress:partly_recognized`, `completed:fully_recognized_none_deferred` |
| `saleseffectivedate` | date | Sales Effective Date | no | no |  |  | `purpose`: `sales_effective_date`; `effect`: `selects_commission_plan_and_historical_team`; `default_behavior`: `created_from_sale_effective_date` |
| `salesrep` | select | Sales Rep | no | yes |  |  | `purpose`: `sales_rep`; `default_behavior`: `customer_sales_rep`; `allowed_values_listed_in`: `employee_sales_reps`; `can`: `override_per_transaction` |
| `shipmethod` | select | Ship Via | no | no |  |  | `purpose`: `ship_method` |
| `shippingcost` | currency | Shipping Cost | no | no |  |  | `purpose`: `shipping_charged` |
| `shippingtax1rate` | text | Tax Rate | no | no |  |  | `purpose`: `tax_rate_of_code`; `value_source`: `system_displayed`; `help_text_shared_with`: `cashrefund.handlingtax1rate`, `cashsale.handlingtax1rate`, `creditmemo.handlingtax1rate` |
| `shippingtaxcode` | select | Shipping Tax Code | no | no |  |  | `purpose`: `shipping_tax_code` |
| `softdescriptor` | text | Soft Descriptor | no | no |  |  | `purpose`: `card_statement_soft_descriptor`; `displayed_on`: `customer_card_statement`; `allowed_values_listed_in`: `credit_card_soft_descriptors` |
| `subsidiary` | select | Subsidiary | no | no |  |  | `purpose`: `subsidiary` |
| `subsidiarytaxregnum` | select | Tax Reg. Number | no |  |  |  | `purpose`: `subsidiary_tax_registration`; `value_source`: `nexus_lookup`; `can`: `override_with_nexus_override` |
| `subtotal` | currency | Subtotal | no | no |  |  | `purpose`: `subtotal_before_discount_shipping_tax`; `value_source`: `sum_of_item_lines` |
| `syncsalesteams` | checkbox | Update Customer | no | no |  |  | `purpose`: `sync_sales_team_to_customer`; `true_means`: `customer_team_updated_from_transaction`; `help_text_shared_with`: `creditmemo.syncpartnerteams`, `invoice.syncpartnerteams`, `salesorder.syncpartnerteams` |
| `taxdetailsoverride` | checkbox | Tax Details Override | no |  |  |  | `purpose`: `override_tax_details`; `requires_permission`: `tax_details_tab_edit` |
| `taxitem` | select | Tax | no | no |  |  | `purpose`: `tax_item`; `default_behavior`: `customer_tax_item`; `value_kind`: `single_rate_single_agency_tax`; `allowed_values_listed_in`: `sales_tax_items`; `can`: `override_per_transaction` |
| `taxpointdateoverride` | checkbox | Override | no | no |  |  | `purpose`: `override_tax_point_date` |
| `taxrate` | float | Tax % | no | no |  |  | `purpose`: `tax_rate`; `default_behavior`: `tax_item_rate`; `can`: `override_per_transaction`; `value_unit`: `percent` |
| `taxtotal` | currency | Tax Total | no | no |  |  | `purpose`: `tax_total`; `value_source`: `tax_rate_times_taxable_total` |
| `tobeemailed` | checkbox | To Be E-mailed | no | no |  |  | `purpose`: `email_transaction`; `format`: `semicolon_separated_no_spaces` |
| `tobefaxed` | checkbox | To Be Faxed | no | no |  |  | `purpose`: `fax_transaction`; `requires`: `fax_service_set_up`; `ui_effect`: `fax_number_shown_beside_checkbox` |
| `tobeprinted` | checkbox | To Be Printed | no | yes |  |  | `purpose`: `print_later`; `true_means`: `queued_for_printing` |
| `toprint2` | checkbox | Print Check | no | no |  |  | `purpose`: `print_refund_check` |
| `total` | currency | Total | no | no |  |  | `purpose`: `total`; `value_source`: `lines_plus_tax` |
| `totalcostestimate` | currency | Est. Extended Cost | no | no |  |  | `purpose`: `estimated_cost`; `value_source`: `estimated_rate_times_quantity` |
| `trandate` | date | Date | yes | no |  |  | `purpose`: `posting_date`; `default_behavior`: `current_date`; `displayed_on`: `registers` |
| `tranid` | text | Refund # | no | no |  |  | `purpose`: `refund_number`; `default_behavior`: `highest_number_plus_one`; `max_length`: `45` characters; `caveat`: `next_number_reverts_to_standard_pattern` |
| `validfrom` | ccvalidfrom | Valid From / Start Date (MM/YYYY) | no | no |  |  | `purpose`: `card_valid_from`; `format`: `MM/YYYY` |
| `vsoeautocalc` | checkbox | Auto Calculate VSOE Allocation | no | no |  |  | `purpose`: `auto_calculate_vsoe_allocation`; `effect`: `allocated_total_is_bundle_revenue` |
| `allowemptycards` | checkbox | Credits | no | no |  |  |  |
| `billingaddress` | summary |  | no | no |  |  |  |
| `billisresidential` | text | Shipping address is residential | no | no |  |  |  |
| `cchold` | text | Detail | no | no |  |  |  |
| `ccholdetails` | textarea | Detail | no | no |  |  |  |
| `ccispurchasecardbin` | checkbox | Purchase Card BIN | no | no |  |  |  |
| `ccprocessoraccount` | text | Credit Card Processing Profile | no | no |  |  |  |
| `checknumber` | text | Check # | no | no |  |  |  |
| `cleared` | checkbox | Cleared | no |  |  |  |  |
| `cleareddate` | date | Date Cleared | no |  |  |  |  |
| `createddate` | datetime |  | no | no |  |  |  |
| `currencysymbol` | text | Currency Symbol | no | no |  |  |  |
| `customercode` | text | Customer Code | no | no |  |  |  |
| `dynamicdescriptor` | text | Soft Descriptor | no | no |  |  |  |
| `email` | emails |  | no | yes |  |  |  |
| `entitynexus` | select | Nexus | no | no |  |  |  |
| `externalid` | text | ExternalId | no | no |  |  |  |
| `handlingmode` | select | Handling Mode | no | no |  |  |  |
| `inputauthcode` | text | Input Auth. Code | no | no |  |  |  |
| `inputpnrefnum` | text |  | no | no |  |  |  |
| `inputreferencecode` | text | Input P/N Ref. | yes | no |  |  |  |
| `isbasecurrency` | checkbox | Base Currency | no | no |  |  |  |
| `ispurchasecard` | text | Override Hold | no | no |  |  |  |
| `lastmodifieddate` | datetime |  | no | no |  |  |  |
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
| `revreconrevcommitment` | checkbox | Rev Rec on Rev Commit. | no | no |  |  |  |
| `salesgroup` | select | Choose Team | no | no |  |  |  |
| `shipaddress` | address | Ship To | no | no |  |  |  |
| `shipaddresslist` | select | Ship To Select | no | no |  |  |  |
| `shipisresidential` | text | Shipping address is residential | no | no |  |  |  |
| `shipoverride` | text |  | no | no |  |  |  |
| `shippingaddress` | summary |  | no | no |  |  |  |
| `shippingcostoverridden` | text | Shipping Cost Overridden | no | no |  |  |  |
| `source` | text | Source | no | no |  |  |  |
| `syncpartnerteams` | checkbox | Update Customer | no | no |  |  |  |
| `taxpointdate` | date | Tax Point Date | no | no |  |  |  |
| `taxregoverride` | checkbox | Tax Registration Override | no |  |  |  |  |
| `tranisvsoebundle` | checkbox | Transaction Is VSOE Bundle | no | no |  |  |  |

## Sublist `item` (Items)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `istaxable` | checkbox | Tax | no |  | `purpose`: `original_order_taxable`; `help_text_shared_with`: `cashrefund.istaxable` |
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
| `line` | text |  | no |  |  |
| `linenumber` | integer |  | no |  |  |
| `matrixtype` | text |  | no |  |  |
| `options` | namevaluelist | Options | no |  |  |
| `price` | select | Price Level | no |  |  |
| `printitems` | text |  | no |  |  |
| `quantity` | float | Quantity | no |  |  |
| `rate` | rate | Rate | no |  |  |
| `rateschedule` | text |  | no |  |  |
| `revrecenddate` | date | Rev. Rec. End Date | no |  |  |
| `revrecschedule` | select | Rev. Rec. Schedule | no |  |  |
| `revrecstartdate` | date | Rev. Rec. Start Date | no |  |  |
| `taxamount` | currency | Tax Amount | no |  |  |
| `taxcode` | select | Tax Code | yes |  |  |
| `taxdetailsreference` | text | Tax Details Reference | no |  |  |
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
| `payment` | Payment |
| `revenue` | Revenue |
| `shipping` | Shipping |
| `teamselling` | Sales Team |
| `workflow` | Workflow |

## Transform targets

| Target record | Default fields |
|---|---|
| `creditmemo` | `customform`, `entity`, `invoices`, `account` |
