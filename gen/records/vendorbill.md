# Vendor Bill (`vendorbill`)
<!-- id: record:vendorbill -->

| Fact | Value |
|---|---|
| Internal ID | `vendorbill` |
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
- `body_only_scriptable_fields`: `estgrossprofit`, `estgrossprofitpercent`, `totalcostestimate`
- `field_id_pattern`: `landedcostamount<n>`, `landedcostsource<n>`
- `not_searchable_fields`: `usertotal`
- `purpose`: `payables_from_vendors_paid_from_payables_list`
- `ui_effect`: `payee_address_shown_as_sublist_on_bill_payment_form`
- `risk`: `scripts_using_old_payee_address_text_may_break`
- `caveat`: `gross_profit_fields_not_scriptable_at_line_level`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `account` | select | Account | no | no |  | `purpose`: `payables_account`; `ui_effect`: `new_option_creates_record` |
| `approvalstatus` | select | Approval Status | no | no |  | `purpose`: `bill_approval_status`; `allowed_values`: `Approved`, `Pending Approval`; `option_meanings`: `approved:payable`, `pending_approval:needs_approval_before_payment` |
| `availablevendorcredit` | currency | Available Vendor Credit | no | no |  | `purpose`: `available_vendor_credit`; `value_unit`: `transaction_currency`; `editable_only_when`: `edit_mode`; `requires`: `credit_limit_warnings_and_form_customization`; `ui_effect`: `warning_when_limit_exceeded_save_allowed` |
| `billaddresslist` | select | Vendor Select | no | no |  | `purpose`: `billing_address_selector`; `value_source`: `vendor_billing_address` |
| `class` | select | Class | no | no |  | `purpose`: `line_class`; `allowed_values_listed_in`: `class_records`; `ui_effect`: `new_option_creates_record`; `help_text_shared_with`: `check.expense.class`, `check.item.class`, `vendorbill.expense.class`, `vendorbill.item.class` |
| `creditlimit` | currency | Credit Limit | no | no |  | `purpose`: `vendor_credit_limit`; `value_source`: `vendor_record_credit_limit`; `configured_by`: `credit_limit_warnings_preference` |
| `currency` | select | Currency | yes | no |  | `purpose`: `transaction_currency`; `cannot`: `change` (when `created_from_purchase_order`) |
| `currencyname` | text | Currency | no | no |  | `purpose`: `vendor_currency_name`; `effect`: `all_amounts_in_this_currency` |
| `customform` | select | Custom Form | yes | no |  | `purpose`: `entry_form` |
| `department` | select | Department | no | no |  | `purpose`: `transaction_department`; `allowed_values_listed_in`: `department_records`; `ui_effect`: `new_option_creates_record` |
| `discountamount` | currency | Disc. Amt. | no | no |  | `purpose`: `discount_amount`; `value_source`: `purchase_order_terms`; `value_examples`: `2%-10_net_30_paid_in_10_days_gets_2_percent`; `caveat`: `taxable_discount_before_tax_else_after` |
| `discountdate` | date | Disc. Date | no | no |  | `purpose`: `discount_date`; `value_source`: `bill_terms` |
| `duedate` | date | Due Date | no | no |  | `purpose`: `due_date`; `default_behavior`: `current_date`; `not_displayed_on`: `aging_reports` (when `no_due_date`) |
| `entity` | select | Vendor | yes | no |  | `purpose`: `vendor`; `ui_effect`: `new_option_creates_record`; `default_behavior`: `vendor_primary_subsidiary` [`subsidiary`]; `can`: `choose_secondary_subsidiary` (when `shared_vendor`) |
| `entitytaxregnum` | select | Tax Reg. Number | no |  |  | `purpose`: `vendor_tax_registration`; `value_source`: `system_displayed` |
| `exchangerate` | currency2 | Exchange Rate | yes | no |  | `purpose`: `exchange_rate`; `can`: `update_currency_record_rate` |
| `landedcostperline` | checkbox | Landed Cost per Line | no | no |  | `purpose`: `landed_cost_per_line` |
| `location` | select | Location | yes | no |  | `purpose`: `line_location`; `allowed_values_listed_in`: `location_records`; `ui_effect`: `new_option_creates_record`; `help_text_shared_with`: `advintercompanyjournalentry.line.location`, `intercompanyjournalentry.line.location`, `journalentry.line.location`, `statisticaljournalentry.line.location`, `vendorbill.expense.location`, `vendorbill.item.location` |
| `memo` | text | Memo | no | yes |  | `purpose`: `memo`; `displayed_on`: `registers`; `help_text_shared_with`: `vendorbill.expense.memo` |
| `nextapprover` | select | Next Approver | no | no |  | `purpose`: `next_approver`; `value_source`: `approval_routing` |
| `nexus` | select | Nexus | no | no |  | `purpose`: `transaction_nexus`; `value_source`: `nexus_lookup`; `can`: `override_with_nexus_override` |
| `overrideinstallments` | checkbox | Override Installments | no | no |  | `purpose`: `override_installments`; `constraint`: `custom_installments_sum_to_total_due` |
| `paymenthold` | checkbox | Payment Hold | no | no |  | `purpose`: `payment_hold`; `effect`: `no_vendor_payment_possible`; `not_displayed_on`: `bill_payments_page`; `ui_effect`: `make_payment_button_hidden`; `caveat`: `electronic_payment_suiteapp_skips_held_bills` |
| `postingperiod` | select | Posting Period | yes | no |  | `purpose`: `posting_period`; `cannot`: `post_to_closed_period`; `value_source`: `set_on_approval` (when `subject_to_approval`) |
| `received` | text | Received Bill | no | no |  | `purpose`: `bill_received`; `caveat`: `unchecked_received_order_in_open_bills_without_due_date` |
| `subsidiary` | select | Subsidiary | no | no |  | `purpose`: `subsidiary` |
| `subsidiarytaxregnum` | select | Tax Reg. Number | no |  |  | `purpose`: `subsidiary_tax_registration`; `value_source`: `nexus_lookup`; `can`: `override_with_nexus_override` |
| `taxdetailsoverride` | checkbox | Tax Details Override | no |  |  | `purpose`: `override_tax_details`; `requires_permission`: `tax_details_tab_edit` |
| `taxpointdate` | date | Tax Point Date | no | no |  | `purpose`: `tax_point_date`; `value_kind`: `date_determining_taxability` |
| `taxpointdateoverride` | checkbox | Override | no | no |  | `purpose`: `override_tax_point_date` |
| `taxregoverride` | checkbox | Tax Registration Override | no |  |  | `purpose`: `override_nexus_and_tax_registration`; `requires_permission`: `tax_details_tab_edit` |
| `terms` | select | Terms | no | no |  | `purpose`: `terms`; `allowed_values_listed_in`: `accounting_list_terms`; `doc_inconsistency`: `unresolved_navigation_placeholders` |
| `tobeprinted` | checkbox | To Be Printed | no |  |  | `purpose`: `print_later`; `effect`: `bulk_print_on_print_bills_page` |
| `trandate` | date | Date | yes | no |  | `purpose`: `posting_date`; `default_behavior`: `current_date`; `displayed_on`: `registers` |
| `tranid` | text | Reference No. | no | no |  | `purpose`: `bill_reference`; `max_length`: `45` characters; `caveat`: `non_latin_characters_reduce_limit`; `displayed_on`: `registers`; `value_examples`: `vendor_invoice_number` |
| `transactionnumber` | text | Transaction Number | no | no |  | `purpose`: `transaction_number`; `caveat`: `lists_show_number_field_by_default`; `caveat`: `hidden_on_custom_forms_by_default` |
| `usertotal` | currency | Amount | no | no |  | `purpose`: `bill_amount`; `effect`: `updated_from_lines` |
| `balance` | text |  | no | no |  |  |
| `billingaddress` | summary |  | no | no |  |  |
| `createddate` | datetime |  | no | no |  |  |
| `currencysymbol` | text | Currency Symbol | no | no |  |  |
| `documentstatus` | text |  | no | no |  |  |
| `entitynexus` | select | Nexus | no | no |  |  |
| `externalid` | text | ExternalId | no | no |  |  |
| `isbasecurrency` | checkbox | Base Currency | no | no |  |  |
| `lastmodifieddate` | datetime |  | no | no |  |  |
| `status` | text | Status | no | no |  |  |
| `statusref` | text |  | no | no |  |  |
| `total` | currency | Amount | no | no |  |  |
| `unbilledorders` | text |  | no | no |  |  |
| `estgrossprofit` |  | Est. Gross Profit |  |  |  |  |
| `estgrossprofitpercent` |  | Est. Gross Profit Percent |  |  |  |  |
| `totalcostestimate` |  | Est. Extended Cost |  |  |  |  |

## Sublist `expense` (Expenses)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `account` | select | Account | yes |  | `purpose`: `expense_line_account`; `ui_effect`: `new_option_creates_record`; `doc_inconsistency`: `expense_line_help_says_payables_account` |
| `class` | select | Class | no |  | `purpose`: `line_class`; `allowed_values_listed_in`: `class_records`; `ui_effect`: `new_option_creates_record`; `help_text_shared_with`: `vendorbill.class` |
| `department` | select | Department | no |  | `purpose`: `line_department`; `allowed_values_listed_in`: `department_records`; `ui_effect`: `new_option_creates_record`; `help_text_shared_with`: `journalentry.department` |
| `location` | select | Location | no |  | `purpose`: `line_location`; `allowed_values_listed_in`: `location_records`; `ui_effect`: `new_option_creates_record`; `help_text_shared_with`: `vendorbill.location`, `vendorcredit.location` |
| `memo` | text | Memo | no |  | `purpose`: `memo`; `displayed_on`: `registers`; `help_text_shared_with`: `vendorbill.memo` |
| `amortizationenddate` | date | Amort. End | no |  |  |
| `amortizationresidual` | rate | Residual | no |  |  |
| `amortizationsched` | select | Amort. Schedule | no |  |  |
| `amortizationtype` | text |  | no |  |  |
| `amortizstartdate` | date | Amort. Start | no |  |  |
| `amount` | currency | Amount | yes |  |  |
| `category` | select | Category | no |  |  |
| `customer` | select | Customer | no |  |  |
| `grossamt` | currency | Gross Amt | no |  |  |
| `isbillable` | checkbox | Billable | no |  |  |
| `line` | text |  | no |  |  |
| `orderdoc` | text |  | no |  |  |
| `orderline` | text |  | no |  |  |
| `projecttask` | select | Project Task | no |  |  |
| `scheduletype` | text |  | no |  |  |

## Sublist `item` (Items)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `class` | select | Class | no |  | `purpose`: `line_class`; `allowed_values_listed_in`: `class_records`; `ui_effect`: `new_option_creates_record`; `help_text_shared_with`: `vendorbill.class` |
| `department` | select | Department | no |  | `purpose`: `line_department`; `allowed_values_listed_in`: `department_records`; `ui_effect`: `new_option_creates_record`; `help_text_shared_with`: `journalentry.department` |
| `location` | select | Location | yes |  | `purpose`: `line_location`; `allowed_values_listed_in`: `location_records`; `ui_effect`: `new_option_creates_record`; `help_text_shared_with`: `vendorbill.location`, `vendorcredit.location` |
| `amortizationenddate` | date | Amort. End | no |  |  |
| `amortizationresidual` | rate | Residual | no |  |  |
| `amortizationsched` | select | Amort. Schedule | no |  |  |
| `amortizationtype` | text |  | no |  |  |
| `amortizstartdate` | date | Amort. Start | no |  |  |
| `amount` | currency | Amount | no |  |  |
| `billreceipts` | select | Receipts | no |  |  |
| `billvariancestatus` | text |  | no |  |  |
| `catchupperiod` | select |  | no |  |  |
| `customer` | select | Customer | no |  |  |
| `deferrevrec` | checkbox |  | no |  |  |
| `description` | textarea | Description | no |  |  |
| `grossamt` | currency | Gross Amt | no |  |  |
| `id` | text |  | no |  |  |
| `inventorydetail` | summary | Inventory Detail | no |  |  |
| `isbillable` | checkbox | Billable | no |  |  |
| `isvsoebundle` | text |  | no |  |  |
| `item` | select | Item | yes |  |  |
| `itemsubtype` | text |  | no |  |  |
| `itemtype` | text |  | no |  |  |
| `landedcostcategory` | select | Landed Cost Category | no |  |  |
| `line` | text |  | no |  |  |
| `linenumber` | integer |  | no |  |  |
| `matrixtype` | text |  | no |  |  |
| `options` | namevaluelist | Options | no |  |  |
| `orderdoc` | text |  | no |  |  |
| `orderline` | text | {1} | no |  |  |
| `quantity` | float | Quantity | no |  |  |
| `rate` | rate | Rate | no |  |  |
| `rateschedule` | text |  | no |  |  |
| `scheduletype` | text |  | no |  |  |
| `units` | select | Units | no |  |  |
| `vendorname` | text | Vendor Name | no |  |  |

## Sublist `accountingbookdetail` (Accounting Books)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `accountingbook` | select | Secondary Book | no |  |  |
| `exchangerate` | currency2 | Exchange Rate | yes |  |  |

## Sublist `installment` (Installments)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `amount` | poscurrency | Amount | yes |  |  |
| `amountdue` | currency | Amount Due | no |  |  |
| `duedate` | date | Due Date | yes |  |  |
| `seqnum` | integer | Installment Number | no |  |  |
| `status` | text | Status | no |  |  |

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
| `custom` | Custom |
| `history` | History |
| `items` | Items |
| `landedcost` | Landed Cost |
| `workflow` | Workflow |

## Suspected source errors

> [!SOURCE-ERROR] `unresolved_navigation_placeholders` (on `terms`)
<!-- source-error: record:vendorbill | terms | unresolved_navigation_placeholders -->

> [!SOURCE-ERROR] `expense_line_help_says_payables_account` (on `expense.account`)
<!-- source-error: record:vendorbill | expense.account | expense_line_help_says_payables_account -->
