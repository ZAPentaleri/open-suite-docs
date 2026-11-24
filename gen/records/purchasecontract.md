# Purchase Contract (`purchasecontract`)
<!-- id: record:purchasecontract -->

| Fact | Value |
|---|---|
| Internal ID | `purchasecontract` |
| Category | transaction |
| Client scripts | yes |
| Server scripts | yes |
| Searchable | yes |
| Search data via | Transaction |
| Tags | `custom_fields_supported`, `scripting_full` |

## Record facts

- `script_types`: `client`, `server`
- `script_functions`: `create`, `update`, `copy`, `delete`, `search`
- `user_events`: `beforeLoad`, `beforeSubmit`, `afterSubmit`
- `requires_feature`: `Purchase Contracts` [`real_name`]
- `user_events`: `beforeLoad`, `beforeSubmit`, `afterSubmit`
- `purpose`: `contracted_quantity_terms_and_discounts_for_purchase_orders`
- `configured_by`: `enable_features_transactions_subtab` [`purchase_contracts`]
- `ui_location`: `transactions_purchases_enter_purchase_contracts`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `billedamount` | currency | Billed Amount | no | no |  |  |
| `billinginstructions` | textarea | Billing Instructions | no | no |  |  |
| `carrier` | text | Carrier | no | no |  |  |
| `class` | select | Class | no | no |  |  |
| `createddate` | datetime |  | no | no |  |  |
| `createdfrom` | select | Created From | no | no |  |  |
| `currency` | select | Currency | no | no |  |  |
| `currencyname` | text | Currency | no | no |  |  |
| `currencysymbol` | text | Currency Symbol | no | no |  |  |
| `customform` | select | Custom Form | no | no |  |  |
| `department` | select | Department | no | no |  |  |
| `effectivitybasedon` | select | Effectivity Based On | yes | no |  |  |
| `employee` | select | Employee | no | no |  |  |
| `enddate` | date | End Date | no | no |  |  |
| `entity` | select | Vendor | yes | no |  |  |
| `entitynexus` | select | Nexus | no | no |  |  |
| `exchangerate` | currency2 | Exchange Rate | yes | no |  |  |
| `externalid` | text | ExternalId | no | no |  |  |
| `incoterm` | select | Incoterm | no | no |  |  |
| `isbasecurrency` | checkbox | Base Currency | no | no |  |  |
| `lastmodifieddate` | datetime |  | no | no |  |  |
| `location` | select | Location | no | no |  |  |
| `maximumamount` | poscurrency | Maximum Amount | no | no |  |  |
| `memo` | text | Memo | no | yes |  |  |
| `minimumamount` | poscurrency | Minimum Amount | no | no |  |  |
| `nexus` | select | Nexus | no | no |  |  |
| `oldrevenuecommitment` | text |  | no | no |  |  |
| `orderstatus` | text |  | no | no |  |  |
| `otherrefnum` | text | Vendor # | no | no |  |  |
| `packinglistinstructions` | textarea | Packing List Instructions | no | no |  |  |
| `productlabelinginstructions` | textarea | Product Labeling Instructions | no | no |  |  |
| `purchasedamount` | currency | Purchased Amount | no | no |  |  |
| `purchaseorderinstructions` | textarea | Purchase Order Instructions | no | no |  |  |
| `receivedamount` | currency | Received Amount | no | no |  |  |
| `source` | text | Source | no | no |  |  |
| `startdate` | date | Start Date | yes | no |  |  |
| `status` | text | Status | no | no |  |  |
| `statusRef` | text |  | no | no |  |  |
| `subsidiary` | select | Subsidiary | no | no |  |  |
| `terms` | select | Payment Terms | no | no |  |  |
| `trandate` | date | Date | yes | no |  |  |
| `tranid` | text | Purchase Contract # | no | no |  |  |
| `updatecurrency` | text | Update Currency | no | no |  |  |
| `updateitemvendor` | checkbox | Update Item Vendor | no | no |  |  |

## Sublist `accountingbookdetail` (Accounting Books)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `accountingbook` | select | Secondary Book | no |  |  |
| `exchangerate` | currency2 | Exchange Rate | yes |  |  |

## Sublist `discount` (Discount)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `amountordered` | nonnegfloat | Amount Ordered | no |  |  |
| `fromamount` | currency | From Amount | yes |  |  |
| `id` | integer |  | no |  |  |
| `memo` | text | Memo | no |  |  |
| `percent` | percent | Percent Discount | yes |  |  |

## Sublist `item` (Item)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `billvariancestatus` | text |  | no |  |  |
| `catchupperiod` | select |  | no |  |  |
| `class` | select | Class | no |  |  |
| `deferrevrec` | checkbox |  | no |  |  |
| `department` | select | Department | no |  |  |
| `description` | textarea | Description | no |  |  |
| `id` | text |  | no |  |  |
| `isvsoebundle` | text |  | no |  |  |
| `item` | select | Item | yes |  |  |
| `itempricing` | summary | Additional Pricing | no |  |  |
| `itemsubtype` | text |  | no |  |  |
| `itemtype` | text |  | no |  |  |
| `line` | text |  | no |  |  |
| `linenumber` | integer |  | no |  |  |
| `location` | select | Location | no |  |  |
| `matrixtype` | text |  | no |  |  |
| `options` | text |  | no |  |  |
| `quantity` | float |  | no |  |  |
| `rate` | rate | Base Rate | yes |  |  |
| `rateschedule` | text |  | no |  |  |
| `units` | select | Units | no |  |  |
| `vendorname` | text | Vendor Name | no |  |  |

## Sublist `links` (Purchase Orders)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `id` | text |  | no |  |  |

## Tabs

| Tab ID | Label |
|---|---|
| `cmmnctntab` | Communication |
| `custom` | Custom |
| `items` | Items |
| `rlrcdstab` | Related Records |
| `rltnshptab` | Relationships |
| `s_sysinfo` | System Information |
| `specialinstrs` | Special Instructions |
| `terms` | Terms |
| `workflow` | Workflow |
