# Vendor Credit (`vendorcredit`)
<!-- id: record:vendorcredit -->

| Fact | Value |
|---|---|
| Internal ID | `vendorcredit` |
| Category | transaction |
| Client scripts | yes |
| Server scripts | yes |
| Searchable | yes |
| Search data via | Transaction |
| Tags | `custom_fields_supported`, `scripting_full` |

## Record facts

- `script_types`: `client`, `server`
- `script_functions`: `create`, `update`, `copy`, `delete`, `search`
- `not_searchable_fields`: `usertotal`
- `purpose`: `vendor_credit_applied_to_payables`
- `value_examples`: `returned_items`, `negotiated_discount`
- `not_supported`: `usertotal_in_lookup`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `account` | select | Account | no | no |  | `purpose`: `payables_account`; `ui_effect`: `new_option_creates_record`; `help_text_shared_with`: `vendorpayment.apacct` |
| `applied` | currency | Applied | no | no |  | `purpose`: `applied_amount`; `value_source`: `sum_of_apply_lines` |
| `autoapply` | checkbox | Auto Apply | no | no |  | `purpose`: `auto_apply`; `effect`: `applies_to_oldest_open_bills`; `effect`: `setting_remembered_for_next_vendor_credit` |
| `billaddresslist` | select | Vendor Select | no | no |  | `purpose`: `billing_address_selector`; `value_source`: `vendor_billing_address` |
| `class` | select | Class | no | no |  | `purpose`: `transaction_class`; `allowed_values_listed_in`: `class_records`; `ui_effect`: `new_option_creates_record` |
| `createdfrom` | select | Created From | no | no |  | `purpose`: `created_from_return_authorization`; `value_source`: `system_displayed` |
| `currency` | select | Currency | yes | no |  | `purpose`: `credit_currency` |
| `department` | select | Department | no | no |  | `purpose`: `transaction_department`; `allowed_values_listed_in`: `department_records`; `ui_effect`: `new_option_creates_record` |
| `duedate` | date | Due Date | no |  |  | `purpose`: `credit_due_date`; `supported_in`: `bill_credit_page`, `bill_payment_page`, `saved_searches`, `reports`, `workbooks` |
| `entity` | select | Vendor | yes | no |  | `purpose`: `vendor`; `ui_effect`: `new_option_creates_record`; `effect`: `creditable_items_follow_vendor_and_subsidiary` |
| `entitytaxregnum` | select | Tax Reg. Number | no |  |  | `purpose`: `vendor_tax_registration`; `value_source`: `system_displayed` |
| `exchangerate` | currency2 | Exchange Rate | yes | no |  | `purpose`: `exchange_rate`; `can`: `update_currency_record_rate` |
| `location` | select | Location | yes | no |  | `purpose`: `line_location`; `allowed_values_listed_in`: `location_records`; `ui_effect`: `new_option_creates_record`; `help_text_shared_with`: `advintercompanyjournalentry.line.location`, `intercompanyjournalentry.line.location`, `journalentry.line.location`, `statisticaljournalentry.line.location`, `vendorbill.expense.location`, `vendorbill.item.location` |
| `memo` | text | Memo | no | yes |  | `purpose`: `memo`; `displayed_on`: `registers` |
| `nexus` | select | Nexus | no | no |  | `purpose`: `transaction_nexus`; `value_source`: `nexus_lookup`; `can`: `override_with_nexus_override` |
| `postingperiod` | select | Posting Period | yes | no |  | `purpose`: `posting_period`; `cannot`: `post_to_closed_period` |
| `subsidiary` | select | Subsidiary | no | no |  | `purpose`: `subsidiary`; `value_source`: `system_displayed` |
| `subsidiarytaxregnum` | select | Tax Reg. Number | no |  |  | `purpose`: `subsidiary_tax_registration`; `value_source`: `nexus_lookup`; `can`: `override_with_nexus_override` |
| `taxdetailsoverride` | checkbox | Tax Details Override | no |  |  | `purpose`: `override_tax_details`; `requires_permission`: `tax_details_tab_edit` |
| `taxpointdate` | date | Tax Point Date | no | no |  | `purpose`: `tax_point_date`; `value_kind`: `date_determining_taxability` |
| `taxpointdateoverride` | checkbox | Override | no | no |  | `purpose`: `override_tax_point_date` |
| `taxregoverride` | checkbox | Tax Registration Override | no |  |  | `purpose`: `override_nexus_and_tax_registration`; `requires_permission`: `tax_details_tab_edit` |
| `tobeprinted` | checkbox | To Be Printed | no |  |  | `purpose`: `print_later`; `effect`: `bulk_print_on_print_bill_credits_page` |
| `trandate` | date | Date | yes | no |  | `purpose`: `posting_date`; `default_behavior`: `current_date`; `displayed_on`: `registers` |
| `tranid` | text | Reference No. | no | no |  | `purpose`: `credit_reference`; `max_length`: `45` characters; `displayed_on`: `registers` |
| `transactionnumber` | text | Transaction Number | no | no |  | `purpose`: `transaction_number`; `default_value`: `To Be Generated` (when `auto_generated_numbering`); `read_only_when`: `account_based_or_external_reference_types`; `caveat`: `differs_from_number_for_account_based_types` |
| `unapplied` | currency | Unapplied | no | no |  | `purpose`: `unapplied_amount`; `value_source`: `updated_from_apply_lines` |
| `usertotal` | currency | Amount | no | no |  | `purpose`: `credit_total`; `effect`: `updated_from_lines`; `constraint`: `not_negative` |
| `billingaddress` | summary |  | no | no |  |  |
| `createddate` | datetime |  | no | no |  |  |
| `currencyname` | text | Currency | no | no |  |  |
| `currencysymbol` | text | Currency Symbol | no | no |  |  |
| `customform` | select | Custom Form | yes | no |  |  |
| `entitynexus` | select | Nexus | no | no |  |  |
| `externalid` | text | ExternalId | no | no |  |  |
| `isbasecurrency` | checkbox | Base Currency | no | no |  |  |
| `lastmodifieddate` | datetime |  | no | no |  |  |
| `total` | currency | Amount | no | no |  |  |

## Sublist `accountingbookdetail` (Accounting Books)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `accountingbook` | select | Secondary Book | no |  |  |
| `exchangerate` | currency2 | Exchange Rate | yes |  |  |

## Sublist `apply` (Apply)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `amount` | currency | Payment | no |  |  |
| `apply` | checkbox | Apply | no |  |  |
| `applydate` | date | Date Due | no |  |  |
| `createdfrom` | text |  | no |  |  |
| `doc` | text |  | no |  |  |
| `due` | currency | Amt. Due | no |  |  |
| `duedate` | date |  | no |  |  |
| `internalid` | text | Hidden | no |  |  |
| `line` | integer |  | no |  |  |
| `refnum` | text | Ref No. | no |  |  |
| `total` | currency | Orig. Amt. | no |  |  |
| `url` | text |  | no |  |  |

## Sublist `expense` (Expenses)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `account` | select | Account | yes |  |  |
| `amortizationenddate` | date | Amort. End | no |  |  |
| `amortizationresidual` | rate | Residual | no |  |  |
| `amortizationsched` | select | Amort. Schedule | no |  |  |
| `amortizationtype` | text |  | no |  |  |
| `amortizstartdate` | date | Amort. Start | no |  |  |
| `amount` | currency | Amount | yes |  |  |
| `category` | text |  | no |  |  |
| `class` | select | Class | no |  |  |
| `customer` | select | Customer | no |  |  |
| `department` | select | Department | no |  |  |
| `isbillable` | checkbox | Billable | no |  |  |
| `line` | text |  | no |  |  |
| `location` | select | Location | no |  |  |
| `memo` | text | Memo | no |  |  |
| `scheduletype` | text |  | no |  |  |

## Sublist `item` (Items)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `amortizationenddate` | date | Amort. End | no |  |  |
| `amortizationresidual` | rate | Residual | no |  |  |
| `amortizationsched` | select | Amort. Schedule | no |  |  |
| `amortizationtype` | text |  | no |  |  |
| `amortizstartdate` | date | Amort. Start | no |  |  |
| `amount` | currency | Amount | no |  |  |
| `billvariancestatus` | text |  | no |  |  |
| `catchupperiod` | select |  | no |  |  |
| `class` | select | Class | no |  |  |
| `customer` | select | Customer | no |  |  |
| `deferrevrec` | checkbox |  | no |  |  |
| `department` | select | Department | no |  |  |
| `description` | textarea | Description | no |  |  |
| `id` | text |  | no |  |  |
| `inventorydetail` | summary | Inventory Detail | no |  |  |
| `isbillable` | checkbox | Billable | no |  |  |
| `isvsoebundle` | text |  | no |  |  |
| `item` | select | Item | yes |  |  |
| `itemsubtype` | text |  | no |  |  |
| `itemtype` | text |  | no |  |  |
| `line` | text |  | no |  |  |
| `linenumber` | integer |  | no |  |  |
| `location` | select | Location | yes |  |  |
| `matrixtype` | text |  | no |  |  |
| `options` | namevaluelist | Options | no |  |  |
| `quantity` | float | Quantity | no |  |  |
| `rate` | rate | Rate | no |  |  |
| `rateschedule` | text |  | no |  |  |
| `scheduletype` | text |  | no |  |  |
| `units` | select | Units | no |  |  |
| `vendorname` | text | Vendor Name | no |  |  |

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
| `workflow` | Workflow |
