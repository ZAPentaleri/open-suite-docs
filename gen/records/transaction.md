# Transaction (`transaction`)
<!-- id: record:transaction -->

| Fact | Value |
|---|---|
| Internal ID | `transaction` |
| Category | transaction |
| Client scripts | yes |
| Server scripts | yes |
| Searchable | yes |
| Tags | `custom_fields_supported`, `deleted_record_search_supported`, `search_only`, `supports_search` |

## Record facts

- `script_types`: `client`, `server`
- `unreadable_in_before_submit_for_external_roles`: `ccnumber`
- `script_functions`: `search`
- `filter_alias`: `entity:name`
- `purpose`: `nearly_all_transaction_searches`
- `difference`: `entity_and_name_filters_both_match_name_id_field`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `ccnumber` |  | Credit Card # |  |  |  |  |
| `entity` |  | Name |  |  |  |  |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `account` | select | Account |  |
| `accounttype` | select | Account Type |  |
| `acctcorpcardexp` | select | Account for Corporate Card Expenses |  |
| `actualproductionenddate` | date | Actual Production End Date |  |
| `actualproductionstartdate` | date | Actual Production Start Date |  |
| `actualshipdate` | date | Actual Ship/Receipt Date |  |
| `altsalesamount` | currency | Amount (Alt.Sales) |  |
| `altsalesnetamount` | currency | Amount (Alt.Sales Net) |  |
| `amount` | currency | Amount |  |
| `amountpaid` | currency | Amount Paid |  |
| `amountremaining` | currency | Amount Remaining |  |
| `amountunbilled` | currency | Amount Unbilled |  |
| `anylineitem` | multiselect | Item On Any Line |  |
| `appliedtoforeignamount` | currency | Applied To Link Amount (Foreign Currency) |  |
| `appliedtoisfxvariance` | checkbox | Applied To Is FxVariance |  |
| `appliedtolinkamount` | currency | Applied To Link Amount |  |
| `appliedtolinktype` | select | Applied To Link Type |  |
| `appliedtotransaction` | select | Applied To Transaction |  |
| `applyingforeignamount` | currency | Applying Link Amount (Foreign Currency) |  |
| `applyingisfxvariance` | checkbox | Applying Is FxVariance |  |
| `applyinglinkamount` | currency | Applying Link Amount |  |
| `applyinglinktype` | select | Applying Link Type |  |
| `applyingtransaction` | select | Applying Transaction |  |
| `approvalstatus` | select | Approval Status |  |
| `authcode` | text | Auth. Code |  |
| `autocalculatelag` | checkbox | Auto-calculate Lag |  |
| `avsstreetmatch` | select | AVS Street Match |  |
| `avszipmatch` | select | AVS Zip Match |  |
| `billable` | checkbox | Billable |  |
| `billcounty` | text | Billing County/State |  |
| `billeddate` | date | Date Billed |  |
| `billingschedule` | select | Billing Schedule |  |
| `billingstatus` | checkbox | Billing Status |  |
| `billingtransaction` | select | Billing Transaction |  |
| `billvariancestatus` | select | Bill Variance Status |  |
| `binnumber` | text | Transaction Bin Number |  |
| `binnumberquantity` | float | Transaction Bin Quantity |  |
| `bookspecifictransaction` | checkbox | Book Specific Transaction |  |
| `buildentireassembly` | checkbox | Build Sub-Assemblies |  |
| `buildvariance` | float | Build Variance |  |
| `built` | float | Built |  |
| `canhavestackablepromotions` | checkbox | Can have stackable promotions |  |
| `catchupperiod` | select | Catch Up Period |  |
| `cccustomercode` | text | CC Customer Code |  |
| `ccexpiredate` | date | CC Expire Date |  |
| `ccname` | text | Credit Cardholder Name |  |
| `ccnumber` | ccnumber | Credit Card Number |  |
| `chargetype` | select | Charge Type |  |
| `class` | select | Class |  |
| `cleared` | checkbox | Cleared |  |
| `closed` | checkbox | Closed |  |
| `closedate` | date | Date Closed |  |
| `cogs` | checkbox | COGS Line |  |
| `commissioneffectivedate` | date | Commission Effective Date |  |
| `commit` | select | Commit |  |
| `confirmationnumber` | text | Confirmation Number |  |
| `contribution` | integer | Contribution % |  |
| `costcomponentamount` | currency | Cost Component Amount |  |
| `costcomponentcategory` | select | Cost Component Category |  |
| `costcomponentitem` | select | Cost Component Item |  |
| `costcomponentquantity` | float | Cost Component Quantity |  |
| `costcomponentstandardcost` | float | Cost Component Standard Cost |  |
| `costestimate` | currency | Est. Extended Cost (Line) |  |
| `costestimaterate` | currency | Est. Unit Cost |  |
| `costestimatetype` | select | Cost Estimate Type |  |
| `createdby` | select | Created By |  |
| `createdfrom` | select | Created From |  |
| `creditamount` | currency | Amount (Credit) |  |
| `cscmatch` | select | CSC Match |  |
| `currency` | select | Currency |  |
| `customersubof` | select | Customer Sub of |  |
| `customform` | select | Custom Form |  |
| `customgl` | checkbox | Custom GL |  |
| `custtype` | select | Customer Category |  |
| `datecreated` | datetime | Date Created |  |
| `daysopen` | integer | Days Open |  |
| `daysoverdue` | integer | Days Overdue |  |
| `debitamount` | currency | Amount (Debit) |  |
| `deferredrevenue` | currency | Amount (Deferred) |  |
| `deferrevrec` | checkbox | Delay Rev. Rec. |  |
| `department` | select | Department |  |
| `depositdate` | date | Date Deposited/Refunded |  |
| `deposittransaction` | select | Deposit/Refund Transaction |  |
| `draccount` | select | Deferred Revenue Reclassification Account |  |
| `duedate` | date | Due Date/Receive By |  |
| `email` | text | Email |  |
| `employee` | select | Employee |  |
| `enddate` | date | End Date |  |
| `entity` | select | Name |  |
| `entitystatus` | select | Estimate/Opportunity Status |  |
| `estgrossprofit` | currency | Est. Gross Profit (Line) |  |
| `estgrossprofitpct` | percent | Est. Gross Profit Percent (Line) |  |
| `exchangerate` | currency | Exchange Rate |  |
| `excludecommission` | checkbox | Exclude Commissions |  |
| `excludefromraterequest` | checkbox | Exclude Item from Rate Request |  |
| `expectedclosedate` | date | Expected Close Date |  |
| `expectedreceiptdate` | date | Expected Receipt Date |  |
| `expensedate` | date | Expense Date |  |
| `expreports` | select | Expense Category |  |
| `externalid` | select | External ID |  |
| `externalidstring` | text | External ID (Text) |  |
| `finchrg` | checkbox | Finance Charge |  |
| `firmed` | checkbox | Firmed |  |
| `forecasttype` | select | Forecast Type |  |
| `formuladate` | date | Formula (Date) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulatext` | text | Formula (Text) |  |
| `fulfillingtransaction` | select | Fulfilling/Receiving Transaction |  |
| `fxaccount` | select | Foreign Currency Adjustment Revenue Account |  |
| `fxamount` | currency | Amount (Foreign Currency) |  |
| `fxcostestimate` | currency | Est. Extended Cost (Line) (Foreign Currency) |  |
| `fxcostestimaterate` | currency | Est. Unit Cost (Foreign Currency) |  |
| `fxestgrossprofit` | currency | Est. Gross Profit (Line) (Foreign Currency) |  |
| `fxtrancostestimate` | currency | Est. Extended Cost (Transaction) (Foreign Currency) |  |
| `fxvsoeallocation` | currency | VSOE Allocation (Foreign Currency) |  |
| `fxvsoeamount` | currency | VSOE Amount (Foreign Currency) |  |
| `fxvsoeprice` | currency | VSOE Price (Foreign Currency) |  |
| `gcoavailabeltocharge` | checkbox | Available to Charge |  |
| `gcoavailabletorefund` | currency | Funds Available to Refund |  |
| `gcoavsstreetmatch` | text | AVS Street Match (Google) |  |
| `gcoavszipmatch` | text | AVS Zip Match (Google) |  |
| `gcobuyeraccountage` | integer | Buyer Account Age |  |
| `gcobuyerip` | text | Buyer IP Address |  |
| `gcochargeamount` | currency | Requested Charge Amount |  |
| `gcochargebackamount` | currency | Chargeback Amount |  |
| `gcoconfirmedchargedtotal` | currency | Charge Amount Confirmed |  |
| `gcoconfirmedrefundedtotal` | currency | Confirmed Refund Amount |  |
| `gcocreditcardnumber` | text | Credit Card (Google) |  |
| `gcocscmatch` | text | CSC Match (Google) |  |
| `gcofinancialstate` | text | Google Order Financial State |  |
| `gcofulfillmentstate` | text | Google Order Fulfillment State |  |
| `gcoorderid` | text | Google Order ID |  |
| `gcoordertotal` | currency | Google Order Total |  |
| `gcopromotionamount` | currency | Google Checkout Promotion Amount |  |
| `gcopromotionname` | text | Google Checkout Promotion |  |
| `gcorefundamount` | currency | Requested Refund Amount |  |
| `gcoshippingtotal` | currency | Google Shipping Total |  |
| `gcostatechangeddetail` | text | Reason for Cancellation |  |
| `giftcertificate` | text | Gift Certificate |  |
| `grossamount` | currency | Amount (Gross) |  |
| `includeinforecast` | checkbox | Include in Forecast |  |
| `incoterm` | select | Incoterm |  |
| `intercostatus` | select | Intercompany Status |  |
| `intercotransaction` | select | Paired Intercompany Transaction |  |
| `internalid` | select | Internal ID |  |
| `internalidnumber` | integer | Internal ID (Number) |  |
| `inventorylocation` | select | Inventory Location |  |
| `inventorysubsidiary` | select | Inventory Subsidiary |  |
| `invsoebundle` | checkbox | In VSOE Bundle |  |
| `isallocation` | checkbox | Is Allocation |  |
| `isbackflush` | checkbox | Backflush |  |
| `iscustomapproval` | checkbox | Custom Approval |  |
| `isgcochargeback` | checkbox | Chargeback |  |
| `isgcochargeconfirmed` | checkbox | Charge Confirmed |  |
| `isgcopaymentguaranteed` | checkbox | Payment Guaranteed by Google |  |
| `isgcorefundconfirmed` | checkbox | Refund Confirmed |  |
| `isinsidedelivery` | checkbox | Is Inside Delivery |  |
| `isinsidepickup` | checkbox | Is Inside Pickup |  |
| `isintercompanyadjustment` | checkbox | Is Intercompany Adjustment |  |
| `isintransitpayment` | checkbox | Is In-Transit |  |
| `ispaypalmeth` | checkbox | Is PayPal Payment Method |  |
| `isreversal` | checkbox | Is Reversal |  |
| `isrevrectransaction` | checkbox | Is Amortization/Revenue Recognition |  |
| `isscrap` | checkbox | Is Scrap |  |
| `isshipaddress` | checkbox | Residential Address |  |
| `istransferpricecosting` | checkbox | Use Item Cost as Transfer Cost |  |
| `isvsoealloc` | checkbox | VSOE Allocation Line |  |
| `iswip` | checkbox | WIP |  |
| `item` | select | Item |  |
| `itemfulfillmentchoice` | select | Fulfillment Choice (Line Level) |  |
| `itemrevision` | select | Item Revision |  |
| `itemsubof` | select | Item Sub of |  |
| `lastmodifieddate` | datetime | Date Last Modified |  |
| `leadsource` | select | Lead Source |  |
| `line` | integer | Line |  |
| `lineuniquekey` | integer | Line Unique Key |  |
| `location` | select | Location |  |
| `mainline` | checkbox | Main Line |  |
| `mainname` | select | Main Line Name |  |
| `matchbilltoreceipt` | checkbox | Match Bill To Receipt |  |
| `memo` | text | Memo |  |
| `memomain` | text | Memo (Main) |  |
| `memorized` | checkbox | Memorized |  |
| `merchantaccount` | text | Credit Card Processor |  |
| `message` | text | Message |  |
| `multisubsidiary` | checkbox | Intercompany |  |
| `netamount` | currency | Amount (Net) |  |
| `nextapprover` | select | Next Approver |  |
| `nextbilldate` | date | Next Bill Date |  |
| `nexus` | select | Nexus |  |
| `nonreimbursable` | checkbox | Non-reimbursable |  |
| `number` | integer | Number |  |
| `opportunity` | select | Opportunity |  |
| `orderallocationstrategy` | select | Order Allocation Strategy |  |
| `otherrefnum` | text | PO/Check Number |  |
| `overrideinstallments` | checkbox | Override Installments |  |
| `packagecount` | integer | Package Count |  |
| `paidtransaction` | select | Paid Transaction |  |
| `partner` | select | Partner |  |
| `partnercontribution` | integer | Partner Contribution % |  |
| `partnerrole` | select | Partner Role |  |
| `partnerteammember` | select | Partner Team Member |  |
| `payingtransaction` | select | Paying Transaction |  |
| `paymenteventdate` | datetime | Payment Event Date |  |
| `paymenteventholdreason` | select | Payment Event Hold Reason |  |
| `paymenteventpurchasecardused` | checkbox | Payment Event Purchase Card Used |  |
| `paymenteventpurchasedatasent` | checkbox | Payment Event Purchase Data Sent |  |
| `paymenteventresult` | select | Payment Event Result |  |
| `paymenteventtype` | select | Payment Event Type |  |
| `paymenthold` | checkbox | Payment Hold |  |
| `paymentmethod` | select | Payment Method |  |
| `paymentoption` | text | Payment Option |  |
| `paypalpending` | checkbox | Pending PayPal Authorization |  |
| `paypalstatus` | text | PayPal Payment Status |  |
| `paypaltranid` | text | PayPal Transaction ID |  |
| `pnrefnum` | text | P/N Ref |  |
| `poastext` | text | PO/Check ID |  |
| `policyviolated` | checkbox | Policy Violated |  |
| `posting` | checkbox | Posting |  |
| `postingperiod` | period | Period |  |
| `pricelevel` | select | Price Level |  |
| `printedpickingticket` | checkbox | Printed Picking Ticket |  |
| `probability` | integer | Probability |  |
| `projectedamount` | currency | Projected Total |  |
| `projecttask` | select | Project Task |  |
| `promocode` | select | Promotion |  |
| `purchaseorder` | select | Purchase Order |  |
| `quantity` | float | Quantity |  |
| `quantitybilled` | float | Quantity Billed |  |
| `quantitycommitted` | float | Quantity Committed |  |
| `quantitypacked` | float | Quantity Packed |  |
| `quantitypicked` | float | Quantity Picked |  |
| `quantityrevcommitted` | float | Quantity Rev. Committed |  |
| `quantityshiprecv` | float | Quantity Shipped/Received |  |
| `recognizedrevenue` | currency | Amount (Recognized) |  |
| `recordtype` | text | Record Type |  |
| `requesteddate` | date | Supply Required By Date |  |
| `revcommitstatus` | select | Rev. Commit. Status |  |
| `revcommittingstatus` | checkbox | Revenue Committing Status |  |
| `revcommittingtransaction` | select | Revenue Committing Transaction |  |
| `revenuestatus` | select | Revenue Status |  |
| `reversaldate` | date | Reversal Date |  |
| `reversalnumber` | text | Reversal Number |  |
| `revrecenddate` | date | Rev. Rec. End Date |  |
| `revreconrevcommitment` | checkbox | Rev. Rec. On Rev. Committment |  |
| `revrecstartdate` | date | Rev. Rec. Start Date |  |
| `revrecterminmonths` | integer | Rev. Rec. Term in Months |  |
| `saascommitmentenddate` | date | SaaS Metric Commitment End Date | marked_new |
| `saascommitmentstartdate` | date | SaaS Metric Commitment Start Date | marked_new |
| `saleseffectivedate` | date | Sales Effective Date |  |
| `salesorder` | select | Sales Order |  |
| `salesrep` | select | Sales Rep |  |
| `salesteammember` | select | Sales Team Member |  |
| `salesteamrole` | select | Sales Team Role |  |
| `schedulingmethod` | select | Scheduling Method |  |
| `serialnumber` | text | Transaction Serial/Lot Number |  |
| `serialnumbercost` | currency | Transaction Serial/Lot Amount |  |
| `serialnumbercostadjustment` | currency | Transaction Serial/Lot Cost Adjustment |  |
| `serialnumberquantity` | float | Transaction Serial/Lot Number Quantity |  |
| `shipcarrier` | select | Shipping Carrier |  |
| `shipcomplete` | checkbox | Ship Complete |  |
| `shipcounty` | text | Shipping County/State |  |
| `shipdate` | date | Ship Date |  |
| `shipgroup` | integer | Ship Group |  |
| `shipmethod` | select | Ship Via |  |
| `shipping` | checkbox | Shipping Line |  |
| `shiprecvstatus` | checkbox | Fulfillment/Receipt Status |  |
| `shiprecvstatusline` | checkbox | Fulfillment/Receipt Status (Line Level) |  |
| `shipto` | select | Ship To |  |
| `source` | select | Source |  |
| `startdate` | date | Start Date |  |
| `status` | select | Status |  |
| `subsidiary` | select | Subsidiary |  |
| `subsidiarytaxregnum` | select | Subsidiary Tax Registration Number |  |
| `taxitem` | select | Tax Item |  |
| `taxline` | checkbox | Tax Line |  |
| `taxperiod` | period | Tax Period |  |
| `taxpointdate` | date | Tax Point Date |  |
| `taxrate` | currency | Rate |  |
| `terms` | select | Terms |  |
| `termsofsale` | select | Terms Of Sale |  |
| `title` | text | Title |  |
| `tobeemailed` | checkbox | To Be Emailed |  |
| `tobeprinted` | checkbox | To Be Printed |  |
| `tosubsidiary` | select | To Subsidiary |  |
| `totalamount` | currency | Amount (Transaction Total) |  |
| `trackingnumbers` | text | Tracking Number |  |
| `trancostestimate` | currency | Est. Extended Cost (Transaction) |  |
| `trandate` | date | Date |  |
| `tranestgrossprofit` | currency | Est. Gross Profit (Transaction) |  |
| `tranestgrossprofitpct` | percent | Est. Gross Profit Percent (Transaction) |  |
| `tranfxestgrossprofit` | currency | Est. Gross Profit (Transaction) (Foreign Currency) |  |
| `tranid` | text | Number/ID |  |
| `tranisvsoebundle` | checkbox | Transaction Is VSOE Bundle |  |
| `transactiondiscount` | checkbox | Transaction discount |  |
| `transactionlinetype` | select | Transaction Line Type |  |
| `transactionnumber` | text | Transaction Number |  |
| `transferlocation` | select | To Location |  |
| `transferorderquantitycommitted` | float | Transfer Order Quantity Committed |  |
| `transferorderquantitypacked` | float | Transfer Order Quantity Packed |  |
| `transferorderquantitypicked` | float | Transfer Order Quantity Picked |  |
| `transferorderquantityreceived` | float | Transfer Order Quantity Received |  |
| `transferorderquantityshipped` | float | Transfer Order Quantity Fulfilled |  |
| `type` | select | Type |  |
| `unit` | select | Units |  |
| `unitcostoverride` | currency | Unit Cost Override |  |
| `vendtype` | select | Vendor Category |  |
| `visibletocustomer` | checkbox | Available In Customer Center |  |
| `voided` | checkbox | Voided |  |
| `vsoeallocation` | currency | VSOE Allocation |  |
| `vsoeamount` | currency | VSOE Amount |  |
| `vsoedeferral` | select | Deferral |  |
| `vsoedelivered` | checkbox | Delivered |  |
| `vsoepermitdiscount` | select | Permit Discount |  |
| `vsoeprice` | currency | VSOE Price |  |
| `website` | select | Web Site |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `abbrev` | text | Abbrev. Type |  |
| `account` | select | Account |  |
| `accounttype` | text | Account Type |  |
| `acctcorpcardexp` | select | Account for Corporate Card Expenses |  |
| `actualproductionenddate` | date | Actual Production End Date |  |
| `actualproductionstartdate` | date | Actual Production Start Date |  |
| `actualshipdate` | date | Actual Ship/Receipt Date |  |
| `altsalesamount` | currency | Amount (Alt.Sales) |  |
| `altsalesnetamount` | currency | Amount (Alt.Sales Net) |  |
| `amount` | currency | Amount |  |
| `amountpaid` | currency | Amount Paid |  |
| `amountremaining` | currency | Amount Remaining |  |
| `amountunbilled` | currency | Amount Unbilled |  |
| `appliedtoforeignamount` | currency | Applied To Link Amount (Foreign Currency) |  |
| `appliedtoisfxvariance` | checkbox | Applied To Link Is FxVariance |  |
| `appliedtolinkamount` | currency | Applied To Link Amount |  |
| `appliedtolinktype` | select | Applied To Link Type |  |
| `appliedtotransaction` | select | Applied To Transaction |  |
| `applyingforeignamount` | currency | Applying Link Amount (Foreign Currency) |  |
| `applyingisfxvariance` | checkbox | Applying Link Is FxVariance |  |
| `applyinglinkamount` | currency | Applying Link Amount |  |
| `applyinglinktype` | select | Applying Link Type |  |
| `applyingtransaction` | select | Applying Transaction |  |
| `approvalstatus` | select | Approval Status |  |
| `authcode` | text | Auth. Code |  |
| `autocalculatelag` | checkbox | Auto-calculate Lag |  |
| `avsstreetmatch` | text | AVS Street Match |  |
| `avszipmatch` | text | AVS Zip Match |  |
| `billable` | checkbox | Billable |  |
| `billcountrycode` | text | Billing Country Code |  |
| `billeddate` | date | Date Billed |  |
| `billingamount` | currency2 | Billing Amount |  |
| `billingschedule` | select | Billing Schedule |  |
| `billingtransaction` | select | Billing Transaction |  |
| `billvariancestatus` | text | Bill Variance Status |  |
| `binnumber` | text | Transaction Bin Number |  |
| `binnumberquantity` | float | Transaction Bin Quantity |  |
| `buildentireassembly` | checkbox | Build Sub-Assemblies |  |
| `buildvariance` | float | Build Variance |  |
| `built` | float | Built |  |
| `canhavestackablepromotions` | checkbox | Can have stackable promotions |  |
| `catchupperiod` | text | Catch Up Period |  |
| `cccustomercode` | text | CC Customer Code |  |
| `ccexpdate` | mmyydate | CC Expire Date |  |
| `ccholdername` | text | Credit Cardholder Name |  |
| `ccnumber` | ccnumber | Credit Card Number |  |
| `ccstreet` | text | Card Street |  |
| `cczipcode` | text | Card Zip Code |  |
| `class` | select | Class |  |
| `classnohierarchy` | select | Class (no hierarchy) |  |
| `cleared` | checkbox | Cleared |  |
| `closed` | checkbox | Closed |  |
| `closedate` | date | Date Closed |  |
| `cogsamount` | currency2 | COGS Amount |  |
| `commissioneffectivedate` | date | Commission Effective Date |  |
| `commit` | select | Commit |  |
| `confirmationnumber` | text | Confirmation Number |  |
| `contribution` | percent | Contribution % |  |
| `contributionprimary` | percent | Primary Sales Rep Contribution % |  |
| `costcomponentamount` | currency | Cost Component Amount |  |
| `costcomponentcategory` | text | Cost Component Category |  |
| `costcomponentitem` | text | Cost Component Item |  |
| `costcomponentquantity` | float | Cost Component Quantity |  |
| `costcomponentstandardcost` | float | Cost Component Standard Cost |  |
| `costestimate` | currency | Est. Extended Cost (Line) |  |
| `costestimaterate` | currency | Est. Unit Cost |  |
| `costestimatetype` | text | Cost Estimate Type |  |
| `createdby` | select | Created By |  |
| `createdfrom` | select | Created From |  |
| `creditamount` | currency | Amount (Credit) |  |
| `cscmatch` | text | CSC Match |  |
| `currency` | select | Currency |  |
| `customform` | select | Custom Form |  |
| `customgl` | checkbox | Custom GL |  |
| `custtype` | select | Customer Category |  |
| `datecreated` | datetime | Date Created |  |
| `daysopen` | integer | Days Open |  |
| `daysoverdue` | integer | Days Overdue |  |
| `debitamount` | currency | Amount (Debit) |  |
| `deferredrevenue` | currency | Amount (Deferred) |  |
| `deferrevrec` | checkbox | Delay Rev. Rec. |  |
| `department` | select | Department |  |
| `departmentnohierarchy` | select | Department (no hierarchy) |  |
| `depositdate` | date | Date Deposited/Refunded |  |
| `deposittransaction` | select | Deposit/Refund Transaction |  |
| `discountamount` | currency | Amount Discount |  |
| `docunit` | text | Transaction Units |  |
| `draccount` | text | Deferred Revenue Reclassification Account |  |
| `duedate` | date | Due Date/Receive By |  |
| `effectiverate` | currency2 | Effective Rate |  |
| `email` | email | Email |  |
| `enddate` | date | End Date |  |
| `entity` | select | Name |  |
| `entitystatus` | select | Estimate/Opportunity Status |  |
| `estgrossprofit` | currency | Est. Gross Profit (Line) |  |
| `estgrossprofitpct` | percent | Est. Gross Profit Percent (Line) |  |
| `estgrossprofitpercent` | percent | Est. Gross Profit Percent (Transaction) |  |
| `exchangerate` | currency2 | Exchange Rate |  |
| `excludecommission` | checkbox | Exclude Commissions |  |
| `excludefromraterequest` | checkbox | Exclude Item from Rate Request |  |
| `expectedclosedate` | date | Expected Close |  |
| `expectedreceiptdate` | date | Expected Receipt Date |  |
| `expensecategory` | select | Expense Category |  |
| `expensedate` | date | Expense Date |  |
| `externalid` | select | External ID |  |
| `firmed` | checkbox | Firmed |  |
| `forecasttype` | select | Forecast Type |  |
| `formulacurrency` | currency | Formula (Currency) |  |
| `formuladate` | date | Formula (Date) |  |
| `formuladatetime` | datetime | Formula (Date/Time) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulapercent` | percent | Formula (Percent) |  |
| `formulatext` | text | Formula (Text) |  |
| `fulfillingtransaction` | select | Fulfilling/Receiving Transaction |  |
| `fxaccount` | text | Foreign Currency Adjustment Revenue Account |  |
| `fxamount` | currency | Amount (Foreign Currency) |  |
| `fxcostestimate` | currency | Est. Extended Cost (Line) (Foreign Currency) |  |
| `fxcostestimaterate` | currency | Est. Unit Cost (Foreign Currency) |  |
| `fxestgrossprofit` | currency | Est. Gross Profit (Line) (Foreign Currency) |  |
| `fxtrancostestimate` | currency | Est. Extended Cost (Transaction) (Foreign Currency) |  |
| `fxvsoeallocation` | currency | VSOE Allocation (Foreign Currency) |  |
| `fxvsoeamount` | currency | VSOE Amount (Foreign Currency) |  |
| `fxvsoeprice` | currency2 | VSOE Price (Foreign Currency) |  |
| `gcoavailabeltocharge` | currency | Funds Available to Process |  |
| `gcoavailabletorefund` | currency | Funds Available to Refund |  |
| `gcoavsstreetmatch` | text | AVS Street Match (Google) |  |
| `gcoavszipmatch` | text | AVS Zip Match (Google) |  |
| `gcobuyeraccountage` | integer | Buyer Account Age |  |
| `gcobuyerip` | text | Buyer IP Address |  |
| `gcochargeamount` | currency | Pending Charges |  |
| `gcochargebackamount` | currency | Chargeback Amount |  |
| `gcoconfirmedchargedtotal` | currency | Charge Amount Confirmed |  |
| `gcoconfirmedrefundedtotal` | currency | Confirmed Refund Amount |  |
| `gcocreditcardnumber` | text | Credit Card (Google) |  |
| `gcocscmatch` | text | CSC Match (Google) |  |
| `gcofinancialstate` | text | Google Order Financial State |  |
| `gcofulfillmentstate` | text | Google Order Fulfillment State |  |
| `gcoorderid` | text | Google Order ID |  |
| `gcoordertotal` | currency | Google Order Total |  |
| `gcopromotionamount` | currency | Google Checkout Promotion Amount |  |
| `gcopromotionname` | text | Google Checkout Promotion |  |
| `gcorefundamount` | currency | Pending Refund Amount |  |
| `gcoshippingtotal` | currency | Google Shipping Total |  |
| `gcostatechangeddetail` | text | Reason for Cancellation |  |
| `giftcert` | text | Gift Certificate |  |
| `grossamount` | currency | Amount (Gross) |  |
| `includeinforecast` | checkbox | Include in Forecast |  |
| `incoterm` | select | Incoterm |  |
| `intercostatus` | select | Intercompany Status |  |
| `intercotransaction` | text | Paired Intercompany Transaction |  |
| `internalid` | select | Internal ID |  |
| `inventorylocation` | select | Inventory Location |  |
| `inventorysubsidiary` | select | Inventory Subsidiary |  |
| `invsoebundle` | checkbox | In VSOE Bundle |  |
| `isallocation` | checkbox | Is Allocation |  |
| `isbackflush` | checkbox | Backflush |  |
| `iscustomapproval` | checkbox | Custom Approval |  |
| `isgcochargeback` | checkbox | Chargeback |  |
| `isgcochargeconfirmed` | checkbox | Charge Confirmed |  |
| `isgcopaymentguaranteed` | checkbox | Payment Guaranteed by Google |  |
| `isgcorefundconfirmed` | checkbox | Refund Confirmed |  |
| `isinsidedelivery` | checkbox | Is Inside Delivery |  |
| `isinsidepickup` | checkbox | Is Inside Pickup |  |
| `isintercompanyadjustment` | checkbox | Is Intercompany Adjustment |  |
| `isintransitpayment` | checkbox | Is In-Transit |  |
| `isreversal` | checkbox | Is Reversal |  |
| `isrevrectransaction` | checkbox | Is Amortization/Revenue Recognition |  |
| `isscrap` | checkbox | Is Scrap |  |
| `isshipaddress` | checkbox | Residential Address |  |
| `istransferpricecosting` | checkbox | Use Item Cost as Transfer Cost |  |
| `iswip` | checkbox | WIP |  |
| `item` | select | Item |  |
| `itemfulfillmentchoice` | select | Fulfillment Choice (Line Level) |  |
| `itemrevision` | select | Item Revision |  |
| `lastmodifieddate` | datetime | Last Modified |  |
| `lastviewed` | datetime | Last Viewed |  |
| `leadsource` | select | Lead Source |  |
| `line` | integer | Line ID |  |
| `linesequencenumber` | integer | Line Sequence Number |  |
| `lineuniquekey` | integer | Line Unique Key |  |
| `location` | select | Location |  |
| `locationnohierarchy` | select | Location (no hierarchy) |  |
| `mainline` | text | * |  |
| `mainname` | select | Main Line Name |  |
| `matchbilltoreceipt` | checkbox | Match Bill To Receipt |  |
| `memo` | text | Memo |  |
| `memomain` | text | Memo (Main) |  |
| `memorized` | checkbox | Memorized |  |
| `merchantaccount` | select | Credit Card Processor |  |
| `message` | text | Message |  |
| `multisubsidiary` | checkbox | Intercompany |  |
| `netamount` | currency | Amount (Net) |  |
| `netamountnotax` | currency | Amount (Net of Tax) |  |
| `nextapprover` | select | Next Approver |  |
| `nextbilldate` | date | Next Bill Date |  |
| `nexus` | select | Nexus |  |
| `nonreimbursable` | checkbox | Non-reimbursable |  |
| `opportunity` | select | Opportunity |  |
| `options` | namevaluelist | Options |  |
| `orderallocationstrategy` | select | Order Allocation Strategy |  |
| `originator` | select | Originator |  |
| `otherrefnum` | text | PO/Check Number |  |
| `overrideinstallments` | checkbox | Override Installments |  |
| `packagecount` | integer | Package Count |  |
| `paidamount` | currency2 | Paid Amount |  |
| `paidtransaction` | select | Paid Transaction |  |
| `partner` | select | Partner |  |
| `partnercontribution` | percent | Partner Contribution % |  |
| `partnerrole` | select | Partner Role |  |
| `partnerteammember` | select | Partner Team Member |  |
| `payingamount` | currency2 | Paying Amount |  |
| `payingtransaction` | select | Paying Transaction |  |
| `paymenteventdate` | datetime | Payment Event Date |  |
| `paymenteventholdreason` | text | Payment Event Hold Reason |  |
| `paymenteventpurchasecardused` | checkbox | Payment Event Purchase Card Used |  |
| `paymenteventpurchasedatasent` | checkbox | Payment Event Purchase Data Sent |  |
| `paymenteventresult` | text | Payment Event Result |  |
| `paymenteventtype` | text | Payment Event Type |  |
| `paymenthold` | checkbox | Payment Hold |  |
| `paymentmethod` | select | Payment Method |  |
| `paymentoption` | select | Payment Option |  |
| `paypalpending` | checkbox | Pending PayPal Authorization |  |
| `paypalstatus` | text | PayPal Payment Status |  |
| `paypaltranid` | text | PayPal Transaction ID |  |
| `payrollbatch` | text | Payroll Batch |  |
| `pnrefnum` | text | P/N Ref |  |
| `policyviolated` | checkbox | Policy Violated |  |
| `porate` | currency2 | PO Rate |  |
| `posting` | checkbox | Posting |  |
| `postingperiod` | select | Period |  |
| `pricelevel` | select | Price Level |  |
| `print` | text | Print |  |
| `probability` | percent | Probability |  |
| `projectedamount` | currency | Projected Total |  |
| `projecttask` | select | Project task |  |
| `promocode` | select | Promotion |  |
| `purchaseorder` | select | Purchase Order |  |
| `quantity` | float | Quantity |  |
| `quantitybilled` | float | Quantity Billed |  |
| `quantitycommitted` | float | Quantity Committed |  |
| `quantitypacked` | float | Quantity Packed |  |
| `quantitypicked` | float | Quantity Picked |  |
| `quantityrevcommitted` | float | Quantity Rev. Committed |  |
| `quantityshiprecv` | float | Quantity Fulfilled/Received |  |
| `quantityuom` | float | Quantity in Transaction Units |  |
| `rate` | currency2 | Item Rate |  |
| `realizedgainpostingtransaction` | select | Realized Gain Posting Transaction |  |
| `recognizedrevenue` | currency | Amount (Recognized) |  |
| `recordtype` | text | Record Type |  |
| `requesteddate` | date | Supply Required By Date |  |
| `revcommitstatus` | text | Rev. Commit. Status |  |
| `revcommittingtransaction` | select | Revenue Committing Transaction |  |
| `revenuestatus` | text | Revenue Status |  |
| `reversaldate` | date | Reversal Date |  |
| `reversalnumber` | text | Reversal Number |  |
| `revrecenddate` | date | Rev. Rec. End Date |  |
| `revreconrevcommitment` | checkbox | Rev. Rec. On Rev. Committment |  |
| `revrecstartdate` | date | Rev. Rec. Start Date |  |
| `rgaccount` | select | Realized Gain Account |  |
| `rgamount` | currency | Realized Gain Amount |  |
| `saascommitmentenddate` | date | SaaS Metric Commitment End Date | marked_new |
| `saascommitmentstartdate` | date | SaaS Metric Commitment Start Date | marked_new |
| `saleseffectivedate` | date | Sales Effective Date |  |
| `salesorder` | select | Sales Order |  |
| `salesrep` | select | Sales Rep |  |
| `salesteammember` | select | Sales Team Member |  |
| `salesteamrole` | select | Sales Team Role |  |
| `schedulingmethod` | text | Scheduling Method |  |
| `serialnumber` | text | Transaction Serial/Lot Number |  |
| `serialnumbercost` | currency | Transaction Serial/Lot Number Amount |  |
| `serialnumbercostadjustment` | currency | Transaction Serial/Lot Number Cost Adjustment |  |
| `serialnumberquantity` | float | Transaction Serial/Lot Number Quantity |  |
| `serialnumbers` | text | Serial/Lot Numbers |  |
| `shipcarrier` | select | Shipping Carrier |  |
| `shipcomplete` | checkbox | Ship Complete |  |
| `shipcountrycode` | text | Shipping Country Code |  |
| `shipdate` | date | Ship Date |  |
| `shipgroup` | integer | Ship Group |  |
| `shipmethod` | select | Ship Via |  |
| `shippingamount` | currency | Amount (Shipping) |  |
| `shiprecvstatusline` | checkbox | Fulfilled/Received (Line Level) |  |
| `shipto` | select | Ship To |  |
| `signedamount` | currency | Amount (Signed) |  |
| `source` | select | Source |  |
| `startdate` | date | Start Date |  |
| `statusref` | select | Status |  |
| `subsidiary` | select | Subsidiary |  |
| `subsidiarynohierarchy` | select | Subsidiary (no hierarchy) |  |
| `subsidiarytaxregnum` | text | Subsidiary Tax Registration Number |  |
| `taxamount` | currency | Amount (Tax) |  |
| `taxcode` | select | Tax Item |  |
| `taxline` | checkbox | Tax Line |  |
| `taxperiod` | select | Tax Period |  |
| `taxpointdate` | date | Tax Point Date |  |
| `taxtotal` | currency | Amount (Tax Total) |  |
| `terminmonths` | integer | Rev. Rec. Term in Months |  |
| `terms` | select | Terms |  |
| `termsofsale` | text | Terms Of Sale |  |
| `title` | text | Title |  |
| `tobeemailed` | checkbox | To Be Emailed |  |
| `tobeprinted` | checkbox | To Be Printed |  |
| `tosubsidiary` | select | To Subsidiary |  |
| `tosubsidiarynohierarchy` | select | To Subsidiary (no hierarchy) |  |
| `total` | currency | Amount (Transaction Total) |  |
| `totalcostestimate` | currency | Est. Extended Cost (Transaction) |  |
| `trackingnumbers` | text | Tracking Numbers |  |
| `trandate` | date | Date |  |
| `tranestgrossprofit` | currency | Est. Gross Profit (Transaction) |  |
| `tranfxestgrossprofit` | currency | Est. Gross Profit (Transaction) (Foreign Currency) |  |
| `tranid` | text | Number |  |
| `tranisvsoebundle` | checkbox | Transaction Is VSOE Bundle |  |
| `transactiondiscount` | checkbox | Transaction Discount |  |
| `transactionlinetype` | select | Transaction Line Type |  |
| `transactionnumber` | text | Transaction Number |  |
| `transferlocation` | select | To Location |  |
| `transferorderitemline` | text | Transfer Order Item Line Id |  |
| `transferorderquantitycommitted` | float | Transfer Order Quantity Committed |  |
| `transferorderquantitypacked` | float | Transfer Order Quantity Packed |  |
| `transferorderquantitypicked` | float | Transfer Order Quantity Picked |  |
| `transferorderquantityreceived` | float | Transfer Order Quantity Received |  |
| `transferorderquantityshipped` | float | Transfer Order Quantity Shipped |  |
| `type` | select | Type |  |
| `unit` | text | Units |  |
| `unitcostoverride` | currency | Unit Cost Override |  |
| `vendtype` | select | Vendor Category |  |
| `visibletocustomer` | checkbox | Available In Customer Center |  |
| `vsoeallocation` | currency | VSOE Allocation |  |
| `vsoeamount` | currency | VSOE Amount |  |
| `vsoedeferral` | text | Deferral |  |
| `vsoedelivered` | checkbox | Delivered |  |
| `vsoepermitdiscount` | text | Permit Discount |  |
| `vsoeprice` | currency2 | VSOE Price |  |
| `website` | text | Web Site |  |

## Search joins

| Join ID | Label | Joined record |
|---|---|---|
| `account` | Account | Account |
| `accountingperiod` | Accounting Period | Accounting Period |
| `accountingtransaction` | Multi-Book Accounting Transaction | Multi-Book Accounting Transaction |
| `advancetoapplyaccount` | Advance to Apply Account | Account |
| `billingaddress` | Billing Address | Address |
| `case` | Case | Case |
| `contactprimary` | Contact (Primary) | Contact |
| `customer` | Customer | Customer |
| `customermain` | Customer (Main Line) | Customer |
| `department` | Department | Department |
| `employee` | Employee | Employee |
| `expensecategory` | Expense Category | Expense Category |
| `inventorydetail` | Inventory Detail | Inventory Detail |
| `itemnumber` | Item Number | Inventory Numbers |
| `job` | Job | Job |
| `jobmain` | Job (Main Line) | Job |
| `leadsource` | Lead Source | Campaign |
| `location` | Location | Location |
| `messages` | Messages | Message |
| `mfgplannedtime` | Manufacturing Planned Time | Manufacturing Planned Time |
| `payrollitem` | Payroll Item | Payroll Item |
| `projecttask` | Project task | Project Task |
| `requestor` | Requestor | Employee |
| `revision` | Revision | Item Revision |
| `revrecschedule` | Rev Rec Schedule | Revenue Recognition Schedules |
| `salesrep` | Sales Rep | Employee |
| `shippingaddress` | Shipping Address | Address |
| `subsidiary` | Subsidiary | Subsidiary |
| `task` | Task | Task |
| `taxdetail` | Tax Detail | Tax Detail |
| `taxitem` | Tax Item | Tax Code |
| `user` | User | Employee |
| `vendor` | Vendor | Vendor |
