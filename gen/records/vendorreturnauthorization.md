# Vendor Return Authorization (`vendorreturnauthorization`)
<!-- id: record:vendorreturnauthorization -->

| Fact | Value |
|---|---|
| Internal ID | `vendorreturnauthorization` |
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
- `purpose`: `track_returns_to_vendor`
- `effect`: `non_posting_transaction`
- `workflow`: `create_authorization`, `approve_or_cancel`, `ship_items`, `credit_return`
- `requires_feature`: `Vendor Return Authorizations` [`real_name`]

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `billaddresslist` | select | Vendor Select | no | no |  | `purpose`: `billing_address_selector`; `value_source`: `vendor_billing_address` |
| `class` | select | Class | no | no |  | `purpose`: `class`; `ui_effect`: `new_option_creates_record` |
| `currency` | select | Currency | yes | no |  | `purpose`: `return_currency`; `cannot`: `change` (when `created_from_purchase_order`) |
| `department` | select | Department | no | no |  | `purpose`: `department`; `ui_effect`: `new_option_creates_record` |
| `entity` | select | Vendor | yes | no |  | `purpose`: `return_vendor`; `allowed_values`: `vendors_of_selected_subsidiary` (when `oneworld`) |
| `entitytaxregnum` | select | Tax Reg. Number | no |  |  | `purpose`: `vendor_tax_registration` |
| `exchangerate` | currency2 | Exchange Rate | yes | no |  | `purpose`: `exchange_rate`; `can`: `update_currency_record_rate` |
| `intercostatus` | select | Intercompany Status | no | no |  | `purpose`: `intercompany_status`; `allowed_values`: `Linked`, `Pending`, `Rejected`; `option_meanings`: `linked:paired_with_other_subsidiary_order`, `pending:not_paired`, `rejected:cannot_pair_incorrect_details` |
| `intercotransaction` | select | Paired Intercompany Transaction | no | no |  | `purpose`: `paired_intercompany_order` |
| `location` | select | Location | no | no |  | `purpose`: `location`; `ui_effect`: `new_option_creates_record` |
| `memo` | text | Memo | no | yes |  | `purpose`: `memo`; `displayed_on`: `reports` |
| `orderstatus` | select | Status | yes | no |  | `purpose`: `return_status`; `default_value`: `Pending Approval` (when `vendor_return_approval`); `default_value`: `Pending Receipt` (when `no_approval_process`); `can`: `override` |
| `subsidiary` | select | Subsidiary | no | no |  | `purpose`: `subsidiary`; `requires_feature`: `OneWorld` [`real_name`]; `can`: `change` (when `shared_vendor`) |
| `subsidiarytaxregnum` | select | Tax Reg. Number | no |  |  | `purpose`: `subsidiary_tax_registration`; `value_source`: `nexus_lookup`; `can`: `override_with_nexus_override` |
| `trandate` | date | Date | yes | no |  | `purpose`: `transaction_date` |
| `tranid` | text | Reference No. | no | no |  | `purpose`: `vendor_return_number`; `max_length`: `45` characters |
| `usertotal` | currency | Amount | no | no |  | `purpose`: `return_total`; `value_source`: `sum_of_lines` |
| `billingaddress` | summary |  | no | no |  |  |
| `createddate` | datetime |  | no | no |  |  |
| `createdfrom` | select | Created From | no | no |  |  |
| `currencyname` | text | Currency | no | no |  |  |
| `currencysymbol` | text | Currency Symbol | no | no |  |  |
| `customform` | select | Custom Form | yes | no |  |  |
| `entitynexus` | select | Nexus | no | no |  |  |
| `externalid` | text | ExternalId | no | no |  |  |
| `isbasecurrency` | checkbox | Base Currency | no | no |  |  |
| `lastmodifieddate` | datetime |  | no | no |  |  |
| `nexus` | select | Nexus | no | no |  |  |
| `status` | text |  | no | no |  |  |
| `statusref` | text |  | no | no |  |  |
| `taxdetailsoverride` | checkbox | Tax Details Override | no |  |  |  |
| `taxpointdate` | date | Tax Point Date | no | no |  |  |
| `taxpointdateoverride` | checkbox | Override | no | no |  |  |
| `taxregoverride` | checkbox | Tax Registration Override | no |  |  |  |
| `total` | currency | Amount | no | no |  |  |

## Sublist `accountingbookdetail` (Accounting Books)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `accountingbook` | select | Secondary Book | no |  |  |
| `exchangerate` | currency2 | Exchange Rate | yes |  |  |

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
| `isclosed` | checkbox | Closed | no |  |  |
| `isdropshipment` | checkbox | Drop Shipment | no |  |  |
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
