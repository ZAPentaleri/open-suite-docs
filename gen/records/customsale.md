# Custom Sale (`customsale`)
<!-- id: record:customsale -->

| Fact | Value |
|---|---|
| Internal ID | `customsale` |
| Search data via | Transaction |

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `account` | select | Account | no | no |  | `purpose`: `posting_account`; `effect`: `debited_or_credited_on_save`; `editable_only_when`: `list_style_basic` |
| `althandlingcost` | currency | Handling Cost | no | no |  |  |
| `altshippingcost` | currency | Shipping Cost | no | no |  |  |
| `billaddress` | address | Bill To | no | no |  |  |
| `billaddresslist` | select | Bill To Select | no | no |  |  |
| `billingaddress` | summary |  | no | no |  |  |
| `billisresidential` | text | Shipping address is residential | no | no |  |  |
| `class` | select | Class | no | no |  |  |
| `couponcode` | select | Coupon Code | no | no |  |  |
| `createddate` | datetime |  | no | no |  |  |
| `createdfrom` | select | Created From | no | no |  |  |
| `currency` | select | Currency | no | no |  |  |
| `currencyname` | text | Currency | no | no |  |  |
| `currencysymbol` | text | Currency Symbol | no | no |  |  |
| `customform` | select | Custom Form | yes | no |  |  |
| `deferredrevenue` | currency | Deferred Revenue | no | no |  |  |
| `department` | select | Department | no | no |  |  |
| `discountamount` | currency | Disc. Amt. | no | no |  |  |
| `discountdate` | date | Disc. Date | no | no |  |  |
| `discountitem` | select | Discount Item | no | no |  |  |
| `discountrate` | rate | Rate | no | no |  |  |
| `discounttotal` | currency | Discount Item | no | no |  |  |
| `duedate` | date | Due Date | no | no |  |  |
| `email` | emails |  | no | no |  |  |
| `enddate` | date | End Date | no | no |  |  |
| `entity` | select | Customer:Job | yes | no |  |  |
| `entitytaxregnum` | select | Customer Tax Reg. Number | no | no |  |  |
| `estgrossprofit` | currency | Est. Gross Profit | no | no |  |  |
| `estgrossprofitpercent` | percent | Est. Gross Profit Percent | no | no |  |  |
| `exchangerate` | currency2 | Exchange Rate | yes | no |  |  |
| `externalid` | text | ExternalId | no | no |  |  |
| `fob` | text | FOB | no | no |  |  |
| `giftcertapplied` | currency | Gift Certificate | no | no |  |  |
| `handlingcost` | currency | Handling Cost | no | no |  |  |
| `isbasecurrency` | checkbox | Base Currency | no | no |  |  |
| `ismultishipto` | checkbox | Enable Item Line Shipping | no | no |  |  |
| `job` | select | Job | no | no |  |  |
| `lastmodifieddate` | datetime |  | no | no |  |  |
| `linkedtrackingnumbers` | text | Tracking # | no | no |  |  |
| `location` | select | Location | yes | no |  |  |
| `memo` | text | Memo | no | no |  |  |
| `message` | textarea | Customer Message | no | no |  |  |
| `messagesel` | select | Select Message | no | no |  |  |
| `muccpromocodeinstance` | text | Promo Code | no | no |  |  |
| `nexus` | select | Nexus | no | no |  |  |
| `opportunity` | select | Opportunity | no | no |  |  |
| `otherrefnum` | text | PO # | no | no |  |  |
| `overrideinstallments` | checkbox | Override | no | no |  |  |
| `partner` | select | Partner | no | no |  |  |
| `postingperiod` | select | Posting Period | yes | no |  |  |
| `promocode` | select | Promotion | no | no |  |  |
| `recognizedrevenue` | currency | Recognized Revenue | no | no |  |  |
| `returntrackingnumbers` | text | Return Tracking # | no | no |  |  |
| `revenuestatus` | text | Revenue Status | no | no |  |  |
| `revreconrevcommitment` | checkbox | Rev Rec on Rev Commit. | no | no |  |  |
| `salesgroup` | select | Choose Team | no | no |  |  |
| `salesrep` | select | Sales Rep | no | no |  |  |
| `shipaddress` | address | Ship To | no | no |  |  |
| `shipaddresslist` | select | Ship To Select | no | no |  |  |
| `shipdate` | date | Ship Date | no | no |  |  |
| `shipisresidential` | text | Shipping address is residential | no | no |  |  |
| `shipmethod` | select | Shipping Method | no | no |  |  |
| `shipoverride` | text |  | no | no |  |  |
| `shippingaddress` | summary |  | no | no |  |  |
| `shippingcost` | currency | Shipping Cost | no | no |  |  |
| `shippingcostoverridden` | text | Shipping Cost Overridden | no | no |  |  |
| `source` | text | Source | no | no |  |  |
| `startdate` | date | Start Date | no | no |  |  |
| `status` | text | Status | no | no |  |  |
| `subsidiary` | select | Subsidiary | no | no |  |  |
| `subsidiarytaxregnum` | select | Subsidiary Tax Reg. Number | no | no |  |  |
| `subtotal` | currency | Subtotal | no | no |  |  |
| `syncpartnerteams` | checkbox | Update Customer | no | no |  |  |
| `taxdetailsoverride` | checkbox | Tax Details Override | no | no |  |  |
| `taxpointdate` | date | Tax Point Date | no | no |  |  |
| `taxpointdateoverride` | checkbox | Override | no | no |  |  |
| `taxregoverride` | checkbox | Nexus Override | no | no |  |  |
| `taxtotal` | currency | Tax Total | no | no |  |  |
| `terms` | select | Terms | no | no |  |  |
| `tobeemailed` | checkbox | To Be E-mailed | no | no |  |  |
| `tobefaxed` | checkbox | To Be Faxed | no | no |  |  |
| `tobeprinted` | checkbox | To Be Printed | no | no |  |  |
| `total` | currency | Total | no | no |  |  |
| `totalcostestimate` | currency | Est. Extended Cost | no | no |  |  |
| `trackingnumbers` | text | Additional Tracking # | no | no |  |  |
| `trandate` | date | Date | yes | no |  |  |
| `tranid` | text | Document # | no | no |  |  |
| `tranisvsoebundle` | checkbox | Transaction Is VSOE Bundle | no | no |  |  |
| `transtatus` | select | Status | no | no |  |  |
| `vsoeautocalc` | checkbox | Auto Calculate VSOE Allocation | no | no |  |  |

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
| `amount` | currency | Amount | no |  |  |
| `catchupperiod` | select |  | no |  |  |
| `class` | select | Class | no |  |  |
| `costestimate` | currency | Est. Extended Cost | no |  |  |
| `costestimaterate` | rate |  | no |  |  |
| `costestimatetype` | select | Cost Estimate Type | no |  |  |
| `currentpercent` | percent | Current % | no |  |  |
| `daysbeforeexpiration` | text |  | no |  |  |
| `deferrevrec` | checkbox |  | no |  |  |
| `department` | select | Department | no |  |  |
| `description` | textarea | Description | no |  |  |
| `excludefromraterequest` | checkbox | Exclude Item from Rate Request | no |  |  |
| `giftcertfrom` | text | From | no |  |  |
| `giftcertmessage` | textarea | Gift Message | no |  |  |
| `giftcertrecipientemail` | email | Recipient Email | no |  |  |
| `giftcertrecipientname` | text | Recipient Name | no |  |  |
| `grossamt` | currency | Gross Amt | no |  |  |
| `id` | text |  | no |  |  |
| `inventorydetail` | summary | Inventory Detail | no |  |  |
| `isvsoebundle` | text |  | no |  |  |
| `item` | select | Item | yes |  |  |
| `itemsubtype` | text | Item subtype | no |  |  |
| `itemtype` | text | Item Type | no |  |  |
| `licensecode` | textarea | License Code | no |  |  |
| `line` | text |  | no |  |  |
| `linenumber` | integer | Line Number | no |  |  |
| `location` | select | Location | yes |  |  |
| `options` | namevaluelist | Options | no |  |  |
| `percentcomplete` | percent | % Complete | no |  |  |
| `price` | select | Price Level | no |  |  |
| `quantity` | float | Quantity | no |  |  |
| `quantityavailable` | float |  | no |  |  |
| `quantityonhand` | float | On Hand | no |  |  |
| `quantityremaining` | float | Back Ordered | no |  |  |
| `rate` | rate | Rate | no |  |  |
| `rateschedule` | text |  | no |  |  |
| `revrecenddate` | date | Rev. Rec. End Date | no |  |  |
| `revrecschedule` | select | Rev. Rec. Schedule | no |  |  |
| `revrecstartdate` | date | Rev. Rec. Start Date | no |  |  |
| `revrecterminmonths` | integer | Rev. Term In Months | no |  |  |
| `shipaddress` | select | Ship To | no |  |  |
| `shipcarrier` | select | Carrier | no |  |  |
| `shipmethod` | select | Ship Via | no |  |  |
| `shippingaddress` | summary | Ship To | no |  |  |
| `taxamount` | currency | Tax Amount | no |  |  |
| `taxdetailsreference` | text | Tax Details Reference | no |  |  |
| `units` | select | Units | no |  |  |
| `vsoeallocation` | currency | Allocation Amount | no |  |  |
| `vsoeamount` | currency | Calculated Amount | no |  |  |
| `vsoedeferral` | select | Deferral | no |  |  |
| `vsoedelivered` | checkbox | Delivered | no |  |  |
| `vsoeisestimate` | checkbox | Estimate | no |  |  |
| `vsoepermitdiscount` | select | Permit Discount | no |  |  |
| `vsoeprice` | currency2 | Allocation Price | no |  |  |
| `vsoesopgroup` | select | Allocation Type | no |  |  |

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
| `promocode` | select | Promotion | yes |  |  |
| `purchasediscount` | rtext | Purchase Discount | no |  |  |
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
| `customerpayment` | `customform` |
| `customerrefund` | `customform` |
| `customsale` | `customform` |
| `invoice` |  |
