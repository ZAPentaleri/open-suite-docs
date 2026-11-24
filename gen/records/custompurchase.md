# Custom Purchase (`custompurchase`)
<!-- id: record:custompurchase -->

| Fact | Value |
|---|---|
| Internal ID | `custompurchase` |
| Search data via | Transaction |

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `account` | select | Account | no | no |  | `purpose`: `posting_account`; `effect`: `debited_or_credited_on_save`; `editable_only_when`: `list_style_basic` |
| `availablevendorcredit` | currency | Available Vendor Credit | no | no |  |  |
| `billaddresslist` | select | Vendor Select | no | no |  |  |
| `billingaddress` | summary |  | no | no |  |  |
| `class` | select | Class | no | no |  |  |
| `createddate` | datetime |  | no | no |  |  |
| `currency` | select | Currency | no | no |  |  |
| `currencyname` | text | Currency | no | no |  |  |
| `currencysymbol` | text | Currency Symbol | no | no |  |  |
| `customform` | select | Custom Form | yes | no |  |  |
| `department` | select | Department | no | no |  |  |
| `discountamount` | currency | Disc. Amt. | no | no |  |  |
| `discountdate` | date | Disc. Date | no | no |  |  |
| `duedate` | date | Due Date | no | no |  |  |
| `entity` | select | Vendor | yes | no |  |  |
| `exchangerate` | currency2 | Exchange Rate | yes | no |  |  |
| `externalid` | text | ExternalId | no | no |  |  |
| `incoterm` | select | Incoterm | no | no |  |  |
| `landedcostmethod` | select | Cost Allocation Method | yes | no |  |  |
| `landedcostperline` | checkbox | Landed Cost per Line | no | no |  |  |
| `lastmodifieddate` | datetime |  | no | no |  |  |
| `location` | select | Location | yes | no |  |  |
| `memo` | text | Memo | no | no |  |  |
| `nexus` | select | Nexus | no | no |  |  |
| `overrideinstallments` | checkbox | Override | no | no |  |  |
| `paymenthold` | checkbox | Payment Hold | no | no |  |  |
| `postingperiod` | select | Posting Period | yes | no |  |  |
| `status` | text | Status | no | no |  |  |
| `subsidiary` | select | Subsidiary | no | no |  |  |
| `taxdetailsoverride` | checkbox | Tax Details Override | no | no |  |  |
| `taxregoverride` | checkbox | Nexus Override | no | no |  |  |
| `taxtotal` | currency | Tax | no | no |  |  |
| `trandate` | date | Date | yes | no |  |  |
| `tranid` | text | Reference No. | no | no |  |  |
| `transactionnumber` | text | Transaction Number | no | no |  |  |
| `transtatus` | select | Status | no | no |  |  |
| `usertotal` | currency | Amount | no | no |  |  |

## Sublist `accountingbookdetail` (Accounting Books)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
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
| `category` | select | Category | no |  |  |
| `class` | select | Class | no |  |  |
| `customer` | select | Customer:Job | no |  |  |
| `department` | select | Department | no |  |  |
| `grossamt` | currency | Gross Amount | no |  |  |
| `isbillable` | checkbox | Billable | no |  |  |
| `line` | integer | Line | no |  |  |
| `location` | select | Location | no |  |  |
| `memo` | text | Memo | no |  |  |
| `taxamount` | currency | Tax Amount | no |  |  |
| `taxdetailsreference` | text | Tax Details Reference | no |  |  |

## Sublist `installment` (Installments)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `amount` | poscurrency | Amount | yes |  |  |
| `amountdue` | currency | Amount Due | no |  |  |
| `duedate` | date | Due Date | yes |  |  |
| `seqnum` | integer | Installment Number | no |  |  |
| `status` | text | Status | no |  |  |

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
| `customer` | select | Customer:Job | no |  |  |
| `deferrevrec` | checkbox |  | no |  |  |
| `department` | select | Department | no |  |  |
| `description` | textarea | Description | no |  |  |
| `grossamt` | currency | Gross Amt | no |  |  |
| `id` | text |  | no |  |  |
| `inventorydetail` | summary | Inventory Detail | no |  |  |
| `isbillable` | checkbox | Billable | no |  |  |
| `item` | select | Item | yes |  |  |
| `landedcost` | summary | Landed Cost | no |  |  |
| `landedcostcategory` | select | Landed Cost Category | no |  |  |
| `line` | text |  | no |  |  |
| `linenumber` | integer | Line Number | no |  |  |
| `location` | select | Location | yes |  |  |
| `options` | namevaluelist | Options | no |  |  |
| `quantity` | float | Quantity | no |  |  |
| `rate` | rate | Rate | no |  |  |
| `taxamount` | currency | Tax Amount | no |  |  |
| `taxdetailsreference` | text | Tax Details Reference | no |  |  |
| `units` | select | Units | no |  |  |
| `vendorname` | text | Vendor Name | no |  |  |

## Sublist `taxdetails` (Taxes)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `calcdetail` | textarea | Details | no |  |  |
| `linename` | text | Name | no |  |  |
| `linetype` | text | Line type | no |  |  |
| `netamount` | currency | Net Amount | no |  |  |
| `taxamount` | currency | Tax Amount | yes |  |  |
| `taxbasis` | currency | Tax Basis | yes |  |  |
| `taxcode` | select | Tax Code | yes |  |  |
| `taxdetailsreference` | text | Tax Details Reference | yes |  |  |
| `taxrate` | percent | Tax Rate | yes |  |  |

## Transform targets

| Target record | Default fields |
|---|---|
| `custompurchase` | `customform` |
