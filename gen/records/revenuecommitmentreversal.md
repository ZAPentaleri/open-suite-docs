# Revenue Commitment Reversal (`revenuecommitmentreversal`)
<!-- id: record:revenuecommitmentreversal -->

| Fact | Value |
|---|---|
| Internal ID | `revenuecommitmentreversal` |
| Category | transaction |
| Client scripts | yes |
| Server scripts | yes |
| Search data via | Transaction |
| Tags | `custom_fields_supported`, `no_create` |

## Record facts

- `script_types`: `client`, `server`
- `not_supported`: `create`
- `script_functions`: `update`, `copy`, `delete`, `search`
- `create_via`: `transform_from_returnauthorization` (refs `N/record.transform`)
- `requires`: `return_authorization_approved_and_received`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `accountingbook` | select | Accounting Book | no | no |  |  |
| `billaddress` | address | Bill To | no | no |  |  |
| `billaddresslist` | select | Bill To Select | no | no |  |  |
| `billingaddress` | summary |  | no | no |  |  |
| `billisresidential` | text | Shipping address is residential | no | no |  |  |
| `class` | select | Class | no | no |  |  |
| `couponcode` | select | Coupon Code | no | no |  |  |
| `createddate` | datetime |  | no | no |  |  |
| `createdfrom` | select | Created From | no | no |  |  |
| `currency` | select | Currency | yes | no |  |  |
| `currencyname` | text | Currency | no | no |  |  |
| `currencysymbol` | text | Currency Symbol | no | no |  |  |
| `customform` | select | Custom Form | yes | no |  |  |
| `deferredrevenue` | currency | Deferred Revenue | no | no |  |  |
| `department` | select | Department | no | no |  |  |
| `discountamount` | currency | Disc. Amt. | no | no |  |  |
| `discountdate` | date | Disc. Date | no | no |  |  |
| `discountitem` | select | Discount | no | no |  |  |
| `discountrate` | rate | Rate | no | no |  |  |
| `discounttotal` | currency | Discount | no | no |  |  |
| `entity` | select | Customer | yes | no |  |  |
| `entitynexus` | select | Nexus | no | no |  |  |
| `exchangerate` | currency2 | Exchange Rate | yes | no |  |  |
| `externalid` | text | ExternalId | no | no |  |  |
| `fob` | text | FOB | no | no |  |  |
| `isbasecurrency` | checkbox | Base Currency | no | no |  |  |
| `lastmodifieddate` | datetime |  | no | no |  |  |
| `linkedtrackingnumbers` | text | Tracking # | no | no |  |  |
| `location` | select | Location | no | no |  |  |
| `memo` | text | Memo | no | yes |  |  |
| `muccpromocodeinstance` | text | Promo Code | no | no |  |  |
| `nexus` | select | Nexus | no | no |  |  |
| `otherrefnum` | text | PO # | no | no |  |  |
| `partner` | select | Partner | no | yes |  |  |
| `postingperiod` | select | Posting Period | yes | no |  |  |
| `promocode` | select | Promotion | no | no |  |  |
| `promocodepluginimpl` | text | Promo Code | no | no |  |  |
| `recognizedrevenue` | currency | Recognized Revenue | no | no |  |  |
| `returntrackingnumbers` | text | Return Tracking # | no | no |  |  |
| `revenuestatus` | text | Revenue Status | no | no |  |  |
| `salesgroup` | select | Choose Team | no | no |  |  |
| `salesrep` | select | Sales Rep | no | yes |  |  |
| `shipaddress` | address | Ship To | no | no |  |  |
| `shipaddresslist` | select | Ship To Select | no | no |  |  |
| `shipisresidential` | text | Shipping address is residential | no | no |  |  |
| `shipoverride` | text |  | no | no |  |  |
| `shippingaddress` | summary |  | no | no |  |  |
| `source` | text | Source | no | no |  |  |
| `subsidiary` | select | Subsidiary | no | no |  |  |
| `subtotal` | currency | Subtotal | no | no |  |  |
| `syncpartnerteams` | checkbox | Update Customer | no | no |  |  |
| `syncsalesteams` | checkbox | Update Customer | no | no |  |  |
| `total` | currency | Total | no | no |  |  |
| `trackingnumbers` | text | Additional Tracking # | no | no |  |  |
| `trandate` | date | Date | yes | no |  |  |
| `tranid` | text | Reversal # | no | no |  |  |
| `tranisvsoebundle` | checkbox | Transaction Is VSOE Bundle | no | no |  |  |
| `vsoeautocalc` | checkbox | Auto Calculate VSOE Allocation | no | no |  |  |

## Sublist `accountingbookdetail` (Accounting Books)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `accountingbook` | select | Secondary Book | no |  |  |
| `exchangerate` | currency2 | Exchange Rate | yes |  |  |

## Sublist `item` (Items)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `amortizationperiod` | text |  | no |  |  |
| `amortizationtype` | text |  | no |  |  |
| `amount` | currency | Amount | no |  |  |
| `billvariancestatus` | text |  | no |  |  |
| `catchupperiod` | select |  | no |  |  |
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
| `licensecode` | textarea | License Code | no |  |  |
| `line` | text |  | no |  |  |
| `linenumber` | integer |  | no |  |  |
| `matrixtype` | text |  | no |  |  |
| `options` | namevaluelist | Options | no |  |  |
| `price` | select | Price Level | no |  |  |
| `printitems` | text |  | no |  |  |
| `quantity` | float | Quantity | no |  |  |
| `quantityavailable` | float |  | no |  |  |
| `rate` | rate | Unit Price | no |  |  |
| `rateschedule` | text |  | no |  |  |
| `revrecenddate` | date | Rev. Rec. End Date | no |  |  |
| `revrecschedule` | select | Rev. Rec. Schedule | no |  |  |
| `revrecstartdate` | date | Rev. Rec. Start Date | no |  |  |
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

## Tabs

| Tab ID | Label |
|---|---|
| `address` | Address |
| `custom` | Custom |
| `history` | History |
| `items` | Items |
| `multipartner` | Partners |
| `revenue` | Revenue |
| `teamselling` | Sales Team |
| `workflow` | Workflow |
