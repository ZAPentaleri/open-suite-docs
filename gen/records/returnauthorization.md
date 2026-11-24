# Return Authorization (`returnauthorization`)
<!-- id: record:returnauthorization -->

| Fact | Value |
|---|---|
| Internal ID | `returnauthorization` |
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
- `purpose`: `expected_customer_return_with_credit_or_refund_amounts`
- `same_as`: `rma`
- `effect`: `non_posting_transaction`
- `requires_feature`: `Return Authorizations` [`real_name`]
- `sublist_requires_feature`: `partners:multi_partner_management`
- `doc_inconsistency`: `fields_table_has_no_rows`
- `before_load_skipped_when`: `view_actions_email_with_popup_preference` [`email_popup_path`]
- `before_load_skipped_when`: `communications_subtab_email_with_popup_preference` [`email_popup_path`]
- `caveat`: `email_popup_actions_are_not_transaction_operations`
- `workaround`: `deploy_before_load_on_message_record` (when `emails_sent_from_popup`)
- `effect`: `save_and_email_always_triggers_before_load`
- `configured_by`: `use_popup_for_main_transaction_email_button_preference`

## Body fields

| Field ID | Type | Label | Required | submitField | Read-only | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `altsalestotal` | currency | Total (Alt. Sales) | no | no |  |  | `purpose`: `alternate_sales_total`; `value_source`: `system_displayed` |
| `billaddress` | address | Bill To | no | no |  |  | `purpose`: `billing_address`; `default_behavior`: `customer_default_billing_address`; `can`: `choose_other_or_new_address_in_selector` |
| `billaddresslist` | select | Bill To Select | no | no |  |  | `purpose`: `billing_address_selector`; `ui_effect`: `new_option_creates_record` |
| `class` | select | Class | no | no |  |  | `purpose`: `class`; `allowed_values_listed_in`: `class_records`; `ui_effect`: `new_option_creates_record` |
| `couponcode` | select | Coupon Code | no | no |  |  | `purpose`: `coupon_code` |
| `createdfrom` | select | Created From | no | no |  |  | `purpose`: `created_from_transaction`; `value_source`: `system_displayed` |
| `currency` | select | Currency | yes | no |  |  | `purpose`: `transaction_currency`; `default_behavior`: `customer_primary_currency`; `requires_feature`: `Multi-Currency Customers` (when `choosing_transaction_currency`) [`real_name`]; `ui_effect`: `reprices_items_and_recalculates_tax_and_profit`; `caveat`: `custom_prices_not_repriced` |
| `currencyname` | text | Currency | no | no |  |  | `purpose`: `customer_currency_name`; `effect`: `all_amounts_in_this_currency` |
| `customform` | select | Custom Form | yes | no |  |  | `purpose`: `custom_form`; `ui_effect`: `new_option_creates_record` |
| `deferredrevenue` | currency | Deferred Revenue | no | no |  |  | `purpose`: `deferred_revenue_amount` |
| `department` | select | Department | no | no |  |  | `purpose`: `department`; `allowed_values_listed_in`: `department_records`; `ui_effect`: `new_option_creates_record` |
| `discountitem` | select | Discount | no | no |  |  | `purpose`: `discount_item`; `ui_effect`: `fills_rate_and_discount_total`; `use_when`: `discount_used_on_original_sale` |
| `discountrate` | rate | Rate | no | no |  |  | `purpose`: `discount_rate`; `value_source`: `discount_item_rate`; `accepts_forms`: `amount`, `percentage` |
| `discounttotal` | currency | Discount | no | no |  |  | `purpose`: `discount_total`; `value_source`: `calculated`; `caveat`: `taxable_discount_before_tax_else_after` |
| `draccount` | select | Deferred Revenue Reclassification Account | no | no |  |  | `purpose`: `intercompany_deferred_revenue_reclass_account` |
| `entity` | select | Customer | yes | no |  |  | `purpose`: `return_customer`; `recommendation`: `select_job_not_parent` |
| `estgrossprofit` | currency | Est. Gross Profit | no | no | yes |  | `purpose`: `estimated_gross_profit`; `value_source`: `revenue_minus_estimated_cost` |
| `estgrossprofitpercent` | percent | Est. Gross Profit Percent | no | no | yes |  | `purpose`: `estimated_gross_margin_percent`; `value_source`: `estimated_profit_divided_by_revenue` |
| `exchangerate` | currency2 | Exchange Rate | yes | no |  |  | `purpose`: `exchange_rate`; `can`: `update_currency_record_rate` |
| `excludecommission` | checkbox | Exclude Commissions | no | yes |  |  | `purpose`: `exclude_from_commissions`; `effect`: `includes_subordinate_transactions` |
| `fxaccount` | select | Foreign Currency Adjustment Revenue Account | no | no |  |  | `purpose`: `intercompany_fx_adjustment_account` |
| `intercostatus` | select | Intercompany Status | no | no |  |  | `purpose`: `intercompany_status`; `allowed_values`: `Linked`, `Pending`, `Rejected`; `option_meanings`: `linked:paired_with_other_subsidiary_order`, `pending:not_paired`, `rejected:cannot_pair_incorrect_details` |
| `intercotransaction` | select | Paired Intercompany Transaction | no | no |  |  | `purpose`: `paired_intercompany_order` |
| `istaxable` | checkbox | Taxable | no | no |  |  | `purpose`: `return_taxable`; `help_text_shared_with`: `returnauthorization.item.istaxable` |
| `leadsource` | select | Lead Source | no | no |  |  | `purpose`: `lead_source`; `allowed_values_listed_in`: `campaigns` (when `marketing_automation`); `allowed_values_listed_in`: `crm_list_lead_source` (when `no_marketing_automation`) |
| `location` | select | Location | no | no |  |  | `purpose`: `transaction_location`; `allowed_values_listed_in`: `location_records`; `ui_effect`: `new_option_creates_record` |
| `memo` | text | Memo | no | yes |  |  | `purpose`: `memo`; `displayed_on`: `reports` |
| `message` | textarea | Customer Message | no | no |  |  | `purpose`: `customer_message`; `displayed_on`: `printed_forms`; `allowed_values_listed_in`: `accounting_list_customer_message` |
| `messagesel` | select | Select Message | no | no |  |  | `purpose`: `customer_message_selector`; `ui_effect`: `fills_message_field`; `allowed_values_listed_in`: `customer_message_list`; `value_examples`: `thank_you_for_your_business` |
| `nexus` | select | Nexus | no | no |  |  | `purpose`: `transaction_nexus`; `value_source`: `nexus_lookup`; `can`: `override_with_nexus_override` |
| `orderstatus` | select | Status | yes | no |  |  | `purpose`: `return_status`; `default_value`: `Pending Approval` (when `return_approval_process`); `default_value`: `Pending Receipt` (when `no_approval_process`); `configured_by`: `return_authorization_default_status_preference`; `can`: `override` |
| `otherrefnum` | text | PO # | no | no |  |  | `purpose`: `customer_po_number` |
| `partner` | select | Partner | no | yes |  |  | `purpose`: `partner`; `default_behavior`: `customer_partner` |
| `promocode` | select | Promotion | no | no |  |  | `purpose`: `promotion_code`; `configured_by`: `default_promotion_code_marketing_preference`; `constraint`: `partner_customers_use_partner_promotions_only`; `effect`: `partner_promotion_attributes_transaction_to_partner` |
| `recognizedrevenue` | currency | Recognized Revenue | no | no |  |  | `purpose`: `cumulative_recognized_revenue` |
| `revcommitstatus` | text | Rev. Commit. Status | no | no |  |  | `purpose`: `revenue_commitment_status`; `allowed_values`: `Pending Commitment`, `Partially Committed`, `Committed`; `value_source`: `related_invoices_or_credit_memos` (when `revreconrevcommitment_cleared`) |
| `revenuestatus` | text | Revenue Status | no | no |  |  | `purpose`: `revenue_recognition_status`; `allowed_values`: `Pending`, `In Progress`, `Completed`; `option_meanings`: `pending:nothing_recognized_all_deferred`, `in_progress:partly_recognized`, `completed:fully_recognized_none_deferred` |
| `revreconrevcommitment` | checkbox | Rev Rec on Rev Commit. | no | no |  |  | `purpose`: `create_revenue_commitment`; `effect`: `commitment_replaces_invoice_or_credit_memo`; `effect`: `creates_revenue_recognition_schedules` |
| `saleseffectivedate` | date | Sales Effective Date | no | no |  |  | `purpose`: `sales_effective_date`; `effect`: `selects_commission_plan_and_historical_team`; `default_behavior`: `created_from_order_effective_date` |
| `salesgroup` | select | Choose Team | no | no |  |  | `purpose`: `sales_team`; `allowed_values_listed_in`: `sales_team_groups` |
| `salesrep` | select | Sales Rep | no | yes |  |  | `purpose`: `sales_rep`; `default_behavior`: `customer_sales_rep`; `doc_inconsistency`: `return_help_says_cash_sale` |
| `shipaddress` | address | Ship To | no | no |  |  | `purpose`: `shipping_address`; `default_behavior`: `customer_default_shipping_address`; `can`: `choose_other_or_new_address_in_selector` |
| `shipaddresslist` | select | Ship To Select | no | no |  |  | `purpose`: `shipping_address_selector`; `ui_effect`: `new_option_creates_record` |
| `subsidiary` | select | Subsidiary | no | no |  |  | `purpose`: `subsidiary`; `value_source`: `system_displayed` |
| `subsidiarytaxregnum` | select | Tax Reg. Number | no |  |  |  | `purpose`: `subsidiary_tax_registration`; `value_source`: `nexus_lookup`; `can`: `override_with_nexus_override` |
| `subtotal` | currency | Subtotal | no | no |  |  | `purpose`: `subtotal_before_discount_shipping_tax`; `value_source`: `sum_of_item_lines` |
| `syncsalesteams` | checkbox | Update Customer | no | no |  |  | `purpose`: `sync_sales_team_to_customer`; `true_means`: `customer_team_updated_from_transaction`; `help_text_shared_with`: `creditmemo.syncpartnerteams`, `invoice.syncpartnerteams`, `salesorder.syncpartnerteams` |
| `taxdetailsoverride` | checkbox | Tax Details Override | no |  |  |  | `purpose`: `override_tax_details`; `requires_permission`: `tax_details_tab_edit` |
| `taxitem` | select | Tax | no | no |  |  | `purpose`: `tax_item`; `default_behavior`: `customer_tax_item`; `recommendation`: `copy_from_credited_sale`; `allowed_values_listed_in`: `sales_tax_items` |
| `taxrate` | float | Tax % | no | no |  |  | `purpose`: `tax_rate`; `default_behavior`: `tax_item_rate`; `recommendation`: `copy_from_credited_sale`; `value_unit`: `percent` |
| `taxregoverride` | checkbox | Tax Registration Override | no |  |  |  | `purpose`: `override_nexus_and_tax_registration`; `requires_permission`: `tax_details_tab_edit` |
| `taxtotal` | currency | Tax Total | no | no |  |  | `purpose`: `tax_total`; `value_source`: `tax_rate_times_taxable_total` |
| `tobeemailed` | checkbox | To Be E-mailed | no | no |  |  | `purpose`: `email_transaction`; `format`: `semicolon_separated_no_spaces` |
| `tobefaxed` | checkbox | To Be Faxed | no | no |  |  | `purpose`: `fax_transaction`; `requires`: `fax_service_set_up` |
| `tobeprinted` | checkbox | To Be Printed | no | yes |  |  | `purpose`: `print_later`; `true_means`: `queued_for_printing` |
| `total` | currency | Total | no | no |  |  | `purpose`: `total`; `value_source`: `lines_plus_tax`; `help_text_shared_with`: `creditmemo.apply.total` |
| `totalcostestimate` | currency | Est. Extended Cost | no | no |  |  | `purpose`: `estimated_cost`; `value_source`: `estimated_rate_times_quantity` |
| `trandate` | date | Date | yes | no |  |  | `purpose`: `transaction_date`; `default_behavior`: `current_date` |
| `tranid` | text | Rtn. Auth. # | no | no |  |  | `purpose`: `return_number`; `default_behavior`: `highest_number_plus_one`; `max_length`: `45` characters; `caveat`: `next_number_reverts_to_standard_pattern` |
| `tranisvsoebundle` | checkbox | Transaction Is VSOE Bundle | no | no |  |  | `purpose`: `transaction_is_vsoe_bundle` |
| `vsoeautocalc` | checkbox | Auto Calculate VSOE Allocation | no | no |  |  | `purpose`: `auto_calculate_vsoe_allocation`; `effect`: `allocated_total_is_bundle_revenue` |
| `billingaddress` | summary |  | no | no |  |  |  |
| `billisresidential` | text | Shipping address is residential | no | no |  |  |  |
| `createddate` | datetime |  | no | no |  |  |  |
| `currencysymbol` | text | Currency Symbol | no | no |  |  |  |
| `email` | emails |  | no | yes |  |  |  |
| `entitynexus` | select | Nexus | no | no |  |  |  |
| `entitytaxregnum` | select | Tax Reg. Number | no |  |  |  |  |
| `externalid` | text | ExternalId | no | no |  |  |  |
| `inputpnrefnum` | text |  | no | no |  |  |  |
| `isbasecurrency` | checkbox | Base Currency | no | no |  |  |  |
| `lastmodifieddate` | datetime |  | no | no |  |  |  |
| `muccpromocodeinstance` | text | Promo Code | no | no |  |  |  |
| `promocodepluginimpl` | text | Promo Code | no | no |  |  |  |
| `returntrackingnumbers` | text | Return Tracking Numbers | no | no |  |  |  |
| `shipisresidential` | text | Shipping address is residential | no | no |  |  |  |
| `shipoverride` | text |  | no | no |  |  |  |
| `shippingaddress` | summary |  | no | no |  |  |  |
| `source` | text | Source | no | no |  |  |  |
| `status` | text |  | no | no |  |  |  |
| `statusref` | text |  | no | no |  |  |  |
| `syncpartnerteams` | checkbox | Update Customer | no | no |  |  |  |
| `taxpointdate` | date | Tax Point Date | no | no |  |  |  |
| `taxpointdateoverride` | checkbox | Override | no | no |  |  |  |

## Sublist `item` (Items)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `istaxable` | checkbox | Tax | no |  | `purpose`: `return_taxable`; `help_text_shared_with`: `returnauthorization.istaxable` |
| `altsalesamt` | currency | Alt. Sales | no |  |  |
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
| `inventorylocation` | select | Inventory Location | no |  |  |
| `inventorysubsidiary` | select | Inventory Subsidiary | no |  |  |
| `isclosed` | checkbox | Closed | no |  |  |
| `isdropshipment` | checkbox | Drop Shipment | no |  |  |
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
| `quantitybilled` | float | Refunded | no |  |  |
| `quantityreceived` | float | Returned | no |  |  |
| `quantityrevcommitted` | float | Rev. Commitment Reversed | no |  |  |
| `rate` | rate | Unit Price | no |  |  |
| `rateschedule` | text |  | no |  |  |
| `revrecenddate` | date | Rev. Rec. End Date | no |  |  |
| `revrecschedule` | select | Rev. Rec. Schedule | no |  |  |
| `revrecstartdate` | date | Rev. Rec. Start Date | no |  |  |
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

## Sublist `partners` (T)

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
| `teamselling` | Sales Team |
| `workflow` | Workflow |

## Transform targets

| Target record | Default fields |
|---|---|
| `cashrefund` | `customform` |
| `creditmemo` | `customform`, `entity`, `invoices`, `account` |
| `itemreceipt` | `customform` |

## Suspected source errors

> [!SOURCE-ERROR] `fields_table_has_no_rows`
<!-- source-error: record:returnauthorization | - | fields_table_has_no_rows -->

> [!SOURCE-ERROR] `return_help_says_cash_sale` (on `salesrep`)
<!-- source-error: record:returnauthorization | salesrep | return_help_says_cash_sale -->
