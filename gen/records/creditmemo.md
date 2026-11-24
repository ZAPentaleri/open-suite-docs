# Credit Memo (`creditmemo`)
<!-- id: record:creditmemo -->

| Fact | Value |
|---|---|
| Internal ID | `creditmemo` |
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
- `doc_inconsistency`: `functions_sentence_names_credit_card_refund`
- `purpose`: `reduces_customer_balance_applies_to_open_or_future_invoices`
- `body_only_scriptable_fields`: `estgrossprofit`, `estgrossprofitpercent`, `totalcostestimate`
- `sublist_requires_feature`: `partners:multi_partner_management`
- `before_load_runs_on_email_operation`: `true`
- `before_load_skipped_when`: `activity_email_with_popup_preference` [`email_popup_path`]
- `workaround`: `deploy_before_load_on_message_record` (when `emails_sent_from_popup`)
- `effect`: `communications_subtab_and_save_email_still_run_before_load`
- `disagreement`: `other_pages_say_communications_subtab_skips_before_load` [`page_value_kept`]
- `configured_by`: `use_popup_for_main_transaction_email_button_preference`
- `caveat`: `gross_profit_fields_not_scriptable_at_line_level`
- `supported_in`: `transaction_search` [`estgrossprofit`, `estgrossprofitpercent`, `totalcostestimate`]

## Body fields

| Field ID | Type | Label | Required | submitField | Read-only | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `account` | select | Account | no | no |  |  | `purpose`: `receivables_account`; `allowed_values_listed_in`: `accounts_receivable_accounts`; `ui_effect`: `new_option_creates_record` |
| `althandlingcost` | currency | Handling Cost | no | no |  |  | `purpose`: `handling_cost`; `value_source`: `calculated_from_ship_method`; `value_source`: `shipping_item_rates` |
| `altshippingcost` | currency | Shipping Cost | no | no |  |  | `purpose`: `shipping_cost`; `value_source`: `calculated_from_ship_method`; `caveat`: `ups_real_time_splits_over_150_lb`; `value_source`: `shipping_item_rates` |
| `applied` | currency | Applied | no | no |  |  | `purpose`: `applied_amount`; `value_source`: `sum_of_apply_lines` |
| `autoapply` | checkbox | Auto Apply | no | no |  |  | `purpose`: `auto_apply`; `effect`: `applies_to_top_receivable_first`; `default_behavior`: `unapplied` (when `cleared`); `effect`: `setting_remembered_for_next_credit_memo` |
| `billaddress` | address | Bill To | no | no |  |  | `purpose`: `billing_address`; `default_behavior`: `customer_default_billing_address`; `can`: `choose_other_or_new_address_in_selector` |
| `billaddresslist` | select | Bill To Select | no | no |  |  | `purpose`: `billing_address_selector`; `ui_effect`: `new_option_creates_record` |
| `class` | select | Class | no | no |  |  | `purpose`: `transaction_class`; `allowed_values_listed_in`: `class_records`; `ui_effect`: `new_option_creates_record` |
| `couponcode` | select | Coupon Code | no | no |  |  | `purpose`: `coupon_code` |
| `currency` | select | Currency | yes | no |  |  | `purpose`: `transaction_currency`; `default_behavior`: `customer_primary_currency`; `requires_feature`: `Multi-Currency Customers` (when `choosing_transaction_currency`) [`real_name`]; `ui_effect`: `reprices_items_and_recalculates_tax_and_profit`; `caveat`: `custom_prices_not_repriced` |
| `currencyname` | text | Currency | no | no |  |  | `purpose`: `customer_currency_name`; `effect`: `all_amounts_in_this_currency` |
| `customform` | select | Custom Form | yes | no |  |  | `purpose`: `custom_form`; `allowed_values_listed_in`: `custom_entry_forms`; `ui_effect`: `new_option_creates_record` |
| `deferredrevenue` | currency | Deferred Revenue | no | no |  |  | `purpose`: `deferred_revenue_amount` |
| `department` | select | Department | no | no |  |  | `purpose`: `transaction_department`; `allowed_values_listed_in`: `department_records`; `ui_effect`: `new_option_creates_record` |
| `discountitem` | select | Discount | no | no |  |  | `purpose`: `discount_item`; `ui_effect`: `fills_rate_and_discount_total` |
| `discountrate` | rate | Rate | no | no |  |  | `purpose`: `discount_rate`; `value_source`: `discount_item_rate`; `accepts_forms`: `amount`, `percentage` |
| `discounttotal` | currency | Discount | no | no |  |  | `purpose`: `discount_total`; `value_source`: `calculated`; `caveat`: `taxable_discount_before_tax_else_after` |
| `entity` | select | Customer | yes | no |  |  | `purpose`: `credit_customer`; `recommendation`: `select_project_not_parent`; `allowed_values_listed_in`: `customer_and_project_records`; `ui_effect`: `new_option_creates_record` |
| `estgrossprofit` | currency | Est. Gross Profit | no | no | yes |  | `purpose`: `estimated_gross_profit`; `value_source`: `revenue_minus_estimated_cost` |
| `estgrossprofitpercent` | percent | Est. Gross Profit Percent | no | no | yes |  | `purpose`: `estimated_gross_margin_percent`; `value_source`: `estimated_profit_divided_by_revenue` |
| `exchangerate` | currency2 | Exchange Rate | yes | no |  |  | `purpose`: `exchange_rate`; `can`: `update_currency_record_rate` |
| `excludecommission` | checkbox | Exclude Commissions | no | yes |  |  | `purpose`: `exclude_from_commissions`; `effect`: `includes_subordinate_transactions` |
| `handlingcost` | currency | Handling Cost | no | no |  |  | `purpose`: `handling_charged` |
| `handlingtax1rate` | text | Handling Tax Rate | no | no |  |  | `purpose`: `tax_rate_of_code`; `value_source`: `system_displayed`; `help_text_shared_with`: `cashrefund.shippingtax1rate`, `cashsale.shippingtax1rate`, `creditmemo.shippingtax1rate` |
| `handlingtaxcode` | select | Handling Tax Code | no | no |  |  | `purpose`: `handling_tax_code` |
| `ismultishipto` | checkbox | Enable Item Line Shipping | no | no |  |  | `purpose`: `multiple_ship_to`; `ui_effect`: `adds_ship_to_carrier_ship_via_to_item_lines` |
| `istaxable` | checkbox | Taxable | no | no |  |  | `purpose`: `credited_order_taxable`; `help_text_shared_with`: `creditmemo.item.istaxable` |
| `leadsource` | select | Lead Source | no | no |  |  | `purpose`: `lead_source`; `allowed_values_listed_in`: `campaigns` (when `marketing_automation`); `allowed_values_listed_in`: `crm_list_lead_source` (when `no_marketing_automation`) |
| `location` | select | Location | yes | no |  |  | `purpose`: `transaction_location`; `allowed_values_listed_in`: `location_records`; `ui_effect`: `new_option_creates_record` |
| `memo` | text | Memo | no | yes |  |  | `purpose`: `memo`; `displayed_on`: `registers`, `customer_center`; `value_kind`: `free_text` |
| `message` | textarea | Customer Message | no | no |  |  | `purpose`: `customer_message`; `displayed_on`: `printed_forms`; `allowed_values_listed_in`: `accounting_list_customer_message` |
| `messagesel` | select | Select Message | no | no |  |  | `purpose`: `customer_message_selector`; `ui_effect`: `fills_message_field`; `allowed_values_listed_in`: `accounting_list_customer_message`; `value_examples`: `thank_you_for_your_business`; `can`: `edit_for_this_transaction` |
| `nexus` | select | Nexus | no | no |  |  | `purpose`: `transaction_nexus`; `value_source`: `nexus_lookup`; `can`: `override_with_nexus_override` |
| `otherrefnum` | text | PO # | no | no |  |  | `purpose`: `customer_po_number` |
| `partner` | select | Partner | no | yes |  |  | `purpose`: `partner`; `default_behavior`: `customer_partner` |
| `postingperiod` | select | Posting Period | yes | no |  |  | `purpose`: `posting_period`; `cannot`: `post_to_closed_period` |
| `promocode` | select | Promotion | no | no |  |  | `purpose`: `promotion_code`; `constraint`: `partner_customers_use_partner_promotions_only`; `effect`: `partner_promotion_attributes_transaction_to_partner`; `allowed_values_listed_in`: `promotion_code_records` |
| `recognizedrevenue` | currency | Recognized Revenue | no | no |  |  | `purpose`: `cumulative_recognized_revenue` |
| `revenuestatus` | text | Revenue Status | no | no |  |  | `purpose`: `revenue_recognition_status`; `allowed_values`: `Pending`, `In Progress`, `Completed`; `option_meanings`: `pending:nothing_recognized_all_deferred`, `in_progress:partly_recognized`, `completed:fully_recognized_none_deferred` |
| `revreconrevcommitment` | checkbox | Rev Rec on Rev Commit. | no | no |  |  | `purpose`: `create_revenue_commitment`; `effect`: `commitment_replaces_invoice_or_credit_memo`; `effect`: `creates_revenue_recognition_schedules` |
| `saleseffectivedate` | date | Sales Effective Date | no | no |  |  | `purpose`: `sales_effective_date`; `effect`: `selects_commission_plan_and_historical_team`; `default_behavior`: `created_from_return_effective_date` |
| `salesrep` | select | Sales Rep | no | yes |  |  | `purpose`: `sales_rep`; `default_behavior`: `customer_sales_rep`; `allowed_values_listed_in`: `employee_sales_reps`; `can`: `override_per_transaction` |
| `shipmethod` | select | Shipping Method | no | no |  |  | `purpose`: `ship_method` |
| `shippingcost` | currency | Shipping Cost | no | no |  |  | `purpose`: `shipping_charged` |
| `shippingtax1rate` | text | Shipping Tax Rate | no | no |  |  | `purpose`: `tax_rate_of_code`; `value_source`: `system_displayed`; `help_text_shared_with`: `cashrefund.handlingtax1rate`, `cashsale.handlingtax1rate`, `creditmemo.handlingtax1rate` |
| `shippingtaxcode` | select | Shipping Tax Code | no | no |  |  | `purpose`: `shipping_tax_code` |
| `subsidiary` | select | Subsidiary | no | no |  |  | `purpose`: `subsidiary` |
| `subsidiarytaxregnum` | select | Tax Reg. Number | no |  |  |  | `purpose`: `subsidiary_tax_registration`; `value_source`: `nexus_lookup`; `can`: `override_with_nexus_override` |
| `subtotal` | currency | Subtotal | no | no |  |  | `purpose`: `subtotal_before_discount_shipping_tax`; `value_source`: `sum_of_item_lines` |
| `syncpartnerteams` | checkbox | Update Customer | no | no |  |  | `purpose`: `sync_sales_team_to_customer`; `true_means`: `customer_team_updated_from_transaction`; `help_text_shared_with`: `cashrefund.syncsalesteams`, `cashsale.syncsalesteams`, `creditmemo.syncsalesteams`, `estimate.syncsalesteams`, `invoice.syncsalesteams`, `returnauthorization.syncsalesteams`, `salesorder.syncsalesteams` |
| `syncsalesteams` | checkbox | Update Customer | no | no |  |  | `purpose`: `sync_sales_team_to_customer`; `true_means`: `customer_team_updated_from_transaction`; `help_text_shared_with`: `creditmemo.syncpartnerteams`, `invoice.syncpartnerteams`, `salesorder.syncpartnerteams` |
| `taxdetailsoverride` | checkbox | Tax Details Override | no |  |  |  | `purpose`: `override_tax_details`; `requires_permission`: `tax_details_tab_edit` |
| `taxitem` | select | Tax | no | no |  |  | `purpose`: `tax_item`; `default_behavior`: `customer_tax_item`; `recommendation`: `copy_from_credited_sale`; `allowed_values_listed_in`: `sales_tax_items` |
| `taxrate` | float | Tax % | no | no |  |  | `purpose`: `tax_rate`; `default_behavior`: `tax_item_rate`; `recommendation`: `copy_from_credited_sale`; `value_unit`: `percent` |
| `taxregoverride` | checkbox | Tax Registration Override | no |  |  |  | `purpose`: `override_nexus_and_tax_registration`; `requires_permission`: `tax_details_tab_edit` |
| `taxtotal` | currency | Tax Total | no | no |  |  | `purpose`: `tax_total`; `value_source`: `tax_rate_times_taxable_total` |
| `tobeemailed` | checkbox | To Be E-mailed | no | no |  |  | `purpose`: `email_transaction`; `format`: `semicolon_separated_no_spaces` |
| `tobefaxed` | checkbox | To Be Faxed | no | no |  |  | `purpose`: `fax_transaction`; `requires`: `fax_service_set_up`; `ui_effect`: `fax_number_shown_beside_checkbox` |
| `tobeprinted` | checkbox | To Be Printed | no | yes |  |  | `purpose`: `print_later`; `true_means`: `queued_for_printing` |
| `total` | currency | Total | no | no |  |  | `purpose`: `total`; `value_source`: `lines_plus_tax`; `help_text_shared_with`: `creditmemo.apply.total` |
| `totalcostestimate` | currency | Est. Extended Cost | no | no |  |  | `purpose`: `estimated_cost`; `value_source`: `estimated_rate_times_quantity` |
| `trandate` | date | Date | yes | no |  |  | `purpose`: `transaction_date`; `default_behavior`: `current_date` |
| `tranid` | text | Credit # | no | no |  |  | `purpose`: `credit_memo_number`; `default_behavior`: `highest_number_plus_one`; `max_length`: `45` characters; `caveat`: `next_number_reverts_to_standard_pattern` |
| `tranisvsoebundle` | checkbox | Transaction Is VSOE Bundle | no | no |  |  | `purpose`: `transaction_is_vsoe_bundle` |
| `unapplied` | currency | Unapplied | no | no |  |  | `purpose`: `unapplied_amount`; `value_source`: `credit_remaining_to_apply` |
| `vsoeautocalc` | checkbox | Auto Calculate VSOE Allocation | no | no |  |  | `purpose`: `auto_calculate_vsoe_allocation`; `effect`: `allocated_total_is_bundle_revenue` |
| `amountpaid` | currency | Amount Used | no | no |  |  |  |
| `amountremaining` | currency | Amount Remaining | no | no |  |  |  |
| `balance` | currency | Balance | no | no |  |  |  |
| `billingaddress` | summary |  | no | no |  |  |  |
| `billisresidential` | text | Shipping address is residential | no | no |  |  |  |
| `bulkprocsubmission` | select | Submission Id | no | no |  |  |  |
| `consolidatebalance` | currency | Consolidated Balance | no | no |  |  |  |
| `createddate` | datetime |  | no | no |  |  |  |
| `createdfrom` | select | Created From | no | no |  |  |  |
| `currencysymbol` | text | Currency Symbol | no | no |  |  |  |
| `email` | emails |  | no | yes |  |  |  |
| `entitynexus` | select | Nexus | no | no |  |  |  |
| `entitytaxregnum` | select | Tax Reg. Number | no |  |  |  |  |
| `externalid` | text | ExternalId | no | no |  |  |  |
| `isbasecurrency` | checkbox | Base Currency | no | no |  |  |  |
| `lastmodifieddate` | datetime |  | no | no |  |  |  |
| `muccpromocodeinstance` | text | Promo Code | no | no |  |  |  |
| `promocodepluginimpl` | text | Promo Code | no | no |  |  |  |
| `salesgroup` | select | Choose Team | no | no |  |  |  |
| `shipaddress` | address | Ship To | no | no |  |  |  |
| `shipaddresslist` | select | Ship To Select | no | no |  |  |  |
| `shipisresidential` | text | Shipping address is residential | no | no |  |  |  |
| `shipoverride` | text |  | no | no |  |  |  |
| `shippingaddress` | summary |  | no | no |  |  |  |
| `shippingcostoverridden` | text | Shipping Cost Overridden | no | no |  |  |  |
| `source` | text | Source | no | no |  |  |  |
| `status` | text | Status | no | no |  |  |  |
| `statusref` | text |  | no | no |  |  |  |
| `taxpointdate` | date | Tax Point Date | no | no |  |  |  |
| `taxpointdateoverride` | checkbox | Override | no | no |  |  |  |
| `unbilledorders` | currency | Unbilled Orders | no | no |  |  |  |

## Sublist `apply` (Apply)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `total` | currency | Orig. Amt. | no |  | `purpose`: `total`; `value_source`: `lines_plus_tax`; `help_text_shared_with`: `creditmemo.total`, `estimate.total`, `returnauthorization.total` |
| `amount` | currency | Payment | no |  |  |
| `apply` | checkbox | Apply | no |  |  |
| `applydate` | date | Date | no |  |  |
| `createdfrom` | text |  | no |  |  |
| `doc` | text |  | no |  |  |
| `due` | currency | Amt. Due | no |  |  |
| `duedate` | date |  | no |  |  |
| `internalid` | text | Hidden | no |  |  |
| `job` | integer |  | no |  |  |
| `line` | integer |  | no |  |  |
| `refnum` | text | Ref No. | no |  |  |
| `url` | text |  | no |  |  |

## Sublist `item` (Items)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `istaxable` | checkbox | Tax | no |  | `purpose`: `credited_order_taxable`; `help_text_shared_with`: `creditmemo.istaxable` |
| `account` | text |  | no |  |  |
| `amortizationperiod` | text |  | no |  |  |
| `amortizationtype` | text |  | no |  |  |
| `amount` | currency | Amount | no |  |  |
| `billvariancestatus` | text |  | no |  |  |
| `catchupperiod` | select |  | no |  |  |
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
| `isdropshipment` | text |  | no |  |  |
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
| `revenue` | Revenue |
| `shipping` | Shipping |
| `teamselling` | Sales Team |
| `workflow` | Workflow |

## Suspected source errors

> [!SOURCE-ERROR] `functions_sentence_names_credit_card_refund`
<!-- source-error: record:creditmemo | - | functions_sentence_names_credit_card_refund -->

> [!SOURCE-ERROR] `other_pages_say_communications_subtab_skips_before_load`
<!-- source-error: record:creditmemo | - | other_pages_say_communications_subtab_skips_before_load -->
