# Revenue Commitment (`revenuecommitment`)
<!-- id: record:revenuecommitment -->

| Fact | Value |
|---|---|
| Internal ID | `revenuecommitment` |
| Category | transaction |
| Client scripts | yes |
| Server scripts | yes |
| Search data via | Transaction |
| Tags | `custom_fields_supported`, `no_create` |

## Record facts

- `script_types`: `client`, `server`
- `not_supported`: `create`
- `script_functions`: `update`, `copy`, `delete`, `search`
- `sublist_requires_feature`: `partners:multi_partner_management`
- `create_via`: `transform_from_salesorder` (refs `N/record.transform`)

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `createdfrom` | select | Created From | no | no |  | `purpose`: `created_from_record`; `value_source`: `system_displayed` |
| `currency` | select | Currency | yes | no |  | `purpose`: `currency`; `value_source`: `sales_order` |
| `enddate` | date | End Date | no | no |  | `purpose`: `recognition_end_date`; `value_source`: `sales_order` |
| `entity` | select | Customer | yes | no |  | `purpose`: `customer_or_project`; `value_source`: `sales_order`; `help_text_shared_with`: `revenuecommitment.job` |
| `exchangerate` | currency2 | Exchange Rate | yes | no |  | `purpose`: `exchange_rate_to_base_currency` |
| `job` | select | Project | no | no |  | `purpose`: `customer_or_project`; `value_source`: `sales_order`; `help_text_shared_with`: `revenuecommitment.entity` |
| `otherrefnum` | text | PO # | no | no |  | `purpose`: `field_from_sales_order`; `value_source`: `sales_order`; `help_text_shared_with`: `revenuecommitment.salesrep` |
| `postingperiod` | select | Posting Period | yes | no |  | `purpose`: `journal_generation_period`; `caveat`: `commitment_is_non_posting` |
| `recognizedrevenue` | currency | Recognized Revenue | no | no |  | `purpose`: `cumulative_recognized_revenue` |
| `revenuestatus` | text | Revenue Status | no | no |  | `purpose`: `revenue_status`; `allowed_values`: `Pending`, `In Progress`, `Completed`; `option_meanings`: `pending:nothing_recognized_all_deferred`, `in_progress:partly_recognized`, `completed:fully_recognized_none_deferred` |
| `salesrep` | select | Sales Rep | no | yes |  | `purpose`: `field_from_sales_order`; `value_source`: `sales_order`; `help_text_shared_with`: `revenuecommitment.otherrefnum` |
| `startdate` | date | Start Date | no | no |  | `purpose`: `recognition_start_date`; `value_source`: `sales_order` |
| `tranid` | text | Rev. Commitment # | no | no |  | `purpose`: `commitment_number`; `value_source`: `auto_generated_on_save` (when `auto_generated_numbering`) |
| `tranisvsoebundle` | checkbox | Transaction Is VSOE Bundle | no | no |  | `purpose`: `vsoe_bundle` |
| `vsoeautocalc` | checkbox | Auto Calculate VSOE Allocation | no | no |  | `purpose`: `auto_calculate_allocation`; `requires`: `eitf_08_01_suiteapp` |
| `accountingbook` | select | Accounting Book | no | no |  |  |
| `billaddress` | address | Bill To | no | no |  |  |
| `billaddresslist` | select | Bill To Select | no | no |  |  |
| `billingaddress` | summary |  | no | no |  |  |
| `billisresidential` | text | Shipping address is residential | no | no |  |  |
| `class` | select | Class | no | no |  |  |
| `couponcode` | select | Coupon Code | no | no |  |  |
| `createddate` | datetime |  | no | no |  |  |
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
| `entitynexus` | select | Nexus | no | no |  |  |
| `externalid` | text | ExternalId | no | no |  |  |
| `fob` | text | FOB | no | no |  |  |
| `isbasecurrency` | checkbox | Base Currency | no | no |  |  |
| `lastmodifieddate` | datetime |  | no | no |  |  |
| `linkedtrackingnumbers` | text | Tracking # | no | no |  |  |
| `location` | select | Location | no | no |  |  |
| `memo` | text | Memo | no | yes |  |  |
| `muccpromocodeinstance` | text | Promo Code | no | no |  |  |
| `nexus` | select | Nexus | no | no |  |  |
| `opportunity` | select | Opportunity | no | no |  |  |
| `partner` | select | Partner | no | yes |  |  |
| `promocode` | select | Promotion | no | no |  |  |
| `promocodepluginimpl` | text | Promo Code | no | no |  |  |
| `returntrackingnumbers` | text | Return Tracking # | no | no |  |  |
| `salesgroup` | select | Choose Team | no | no |  |  |
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
