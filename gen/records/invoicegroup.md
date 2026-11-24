# Invoice Group (`invoicegroup`)
<!-- id: record:invoicegroup -->

| Fact | Value |
|---|---|
| Internal ID | `invoicegroup` |

## Record facts

- `record_actions`: `groupinvoices`, `removeinvoicesfromgroup`
- `record_actions`: `groupinvoices`, `removeinvoicesfromgroup`
- `requires_feature`: `Invoice Groups` [`real_name`]
- `script_functions`: `create`, `update`, `delete`, `search`
- `not_supported`: `copy`, `transform`
- `action_errors`: `invoices_missing`, `group_missing`, `group_paid`, `invoices_paid`, `no_permission`, `too_many_invoices`
- `parameters`: `record_id`, `invoice_list`
- `ui_location`: `group_invoices_and_remove_from_group_buttons`
- `effect`: `groups_by_customer_and_common_criteria`

## Body fields

| Field ID | Type | Label | Required | submitField | Read-only | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `amountdue` | currency | Amount Due | no |  |  |  | `purpose`: `group_amount_due`; `value_source`: `system_displayed` |
| `amountpaid` | currency | Amount Paid | no |  |  |  | `purpose`: `group_amount_paid`; `value_source`: `system_displayed` |
| `billaddress` | address | Bill To | no |  |  |  | `purpose`: `billing_address`; `default_behavior`: `customer_default_billing_address`; `options`: `New`, `Custom`; `option_meanings`: `new:saved_to_entity`, `custom:transaction_only`; `can`: `edit_existing_address_with_edit_icon` |
| `billaddresslist` | select | Bill To Select | no |  |  |  | `purpose`: `billing_address_selector`; `options`: `New`, `Custom`; `option_meanings`: `new:saved_to_entity`, `custom:transaction_only`; `can`: `edit_existing_address_with_edit_icon` |
| `currency` | select | Currency | yes |  | yes |  | `purpose`: `group_currency` |
| `customer` | select | Customer | yes |  | yes |  | `purpose`: `group_customer` |
| `discounttotal` | currency | Discount Total | no |  |  |  | `purpose`: `group_discount_total`; `value_source`: `sum_of_group_invoices` |
| `duedate` | date | Due Date | yes |  |  |  | `purpose`: `group_due_date`; `value_source`: `terms_applied_to_transaction_date`; `effect`: `invoices_follow_group_due_date`; `can`: `override` |
| `groupedbypo` | checkbox | Grouped By PO # | no |  | yes |  | `purpose`: `grouped_by_po_number` |
| `invoicegroupnumber` | text | Invoice Group Number | no |  |  |  | `purpose`: `invoice_group_number`; `default_behavior`: `highest_number_plus_one` |
| `itemtotal` | currency | Item Total | no |  |  |  | `purpose`: `group_item_total`; `value_source`: `sum_of_group_invoices` |
| `memo` | text | Memo | no |  |  |  | `purpose`: `group_memo` |
| `ponumber` | text | PO # | no |  | yes |  | `purpose`: `group_po_number` |
| `shippingcost` | currency | Shipping Cost | no |  |  |  | `purpose`: `shipping_cost`; `value_source`: `calculated_from_ship_method`; `caveat`: `ups_real_time_splits_over_150_lb`; `value_source`: `shipping_item_rates`; `help_text_shared_with`: `estimate.altshippingcost` |
| `status` | select | Status | no |  |  |  | `purpose`: `invoice_group_status`; `allowed_values`: `Undefined`, `Open`, `Partially Paid`, `Fully Paid`, `Void` |
| `subsidiary` | select | Subsidiary | yes |  | yes |  | `purpose`: `group_subsidiary` |
| `taxtotal` | currency | Tax | no |  |  |  | `purpose`: `tax_total`; `value_source`: `tax_rate_times_taxable_total` |
| `terms` | select | Terms | no |  | yes |  | `purpose`: `group_terms` |
| `total` | currency | Total | no |  |  |  | `purpose`: `total`; `value_source`: `lines_tax_and_shipping` |
| `trandate` | date | Date | yes |  |  |  | `purpose`: `posting_date`; `default_behavior`: `current_date` |
| `customername` | select | Customer Name | no |  |  |  |  |
| `datedriven` | radio |  | no |  |  |  |  |
| `dayofmonthnetdue` | integer |  | no |  |  |  |  |
| `daysuntilnetdue` | integer |  | no |  |  |  |  |
| `duenextmonthifwithindays` | integer |  | no |  |  |  |  |
| `externalid` | text | ExternalId | no |  |  |  |  |
| `handlingcost` | currency | Handling Cost | no |  |  |  |  |
| `id` | integer | ID | no |  |  |  |  |
| `lastmodifiedby` | select | Modified By | no |  |  |  |  |
| `lastmodifieddate` | datetime | Last Modified | no |  |  |  |  |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `currency` | select | Currency |  |
| `customer` | select | Customer |  |
| `discounttotal` | currency | Discount Total |  |
| `duedate` | date | Due Date |  |
| `externalid` | select | External ID |  |
| `externalidstring` | text | External ID (Text) |  |
| `formuladate` | date | Formula (Date) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulatext` | text | Formula (Text) |  |
| `fxamount` | currency | Amount (Foreign Currency) |  |
| `fxamountdue` | currency | Amount Due (Foreign Currency) |  |
| `fxamountpaid` | currency | Amount Paid (Foreign Currency) |  |
| `groupedbypo` | checkbox | Grouped By PO # |  |
| `handlingcost` | currency | Handling Cost |  |
| `internalid` | integer | Internal ID (Number) |  |
| `invoicegroupnumber` | text | Invoice Group # |  |
| `invoicegroupstatus` | select | Status |  |
| `itemtotal` | currency | Item Total |  |
| `memo` | text | Memo |  |
| `ponumber` | text | PO # |  |
| `shippingcost` | currency | Shipping Cost |  |
| `subsidiary` | select | Subsidiary |  |
| `taxtotal` | currency | Tax Total |  |
| `terms` | select | Terms |  |
| `trandate` | date | Date Created |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `billaddress` | text | Billing Address |  |
| `currency` | select | Currency |  |
| `customer` | select | Customer |  |
| `discounttotal` | currency | Discount Total |  |
| `duedate` | date | Due Date |  |
| `externalid` | select | External ID |  |
| `formulacurrency` | currency | Formula (Currency) |  |
| `formuladate` | date | Formula (Date) |  |
| `formuladatetime` | datetime | Formula (Date/Time) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulapercent` | percent | Formula (Percent) |  |
| `formulatext` | text | Formula (Text) |  |
| `fxamount` | currency | Amount (Foreign Currency) |  |
| `fxamountdue` | currency | Amount Due (Foreign Currency) |  |
| `fxamountpaid` | currency | Amount Paid (Foreign Currency) |  |
| `groupedbypo` | checkbox | Grouped By PO # |  |
| `handlingcost` | currency | Handling Cost |  |
| `internalid` | integer | Internal ID (Number) |  |
| `invoicecount` | integer | Invoice Count |  |
| `invoicegroupnumber` | text | Invoice Group # |  |
| `invoicegroupstatus` | text | Status |  |
| `itemtotal` | currency | Item Total |  |
| `memo` | text | Memo |  |
| `ponumber` | text | PO # |  |
| `shippingcost` | currency | Shipping Cost |  |
| `subsidiary` | select | Subsidiary |  |
| `taxtotal` | currency | Tax Total |  |
| `terms` | select | Terms |  |
| `trandate` | date | Date |  |
