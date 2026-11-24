# Item Receipt (`itemreceipt`)
<!-- id: record:itemreceipt -->

| Fact | Value |
|---|---|
| Internal ID | `itemreceipt` |
| Category | transaction |
| Client scripts | yes |
| Server scripts | yes |
| Search data via | Transaction |
| Tags | `custom_fields_supported`, `no_copy`, `no_create` |

## Record facts

- `initialization_default_value`: `entity`, `entity_internal_id` (refs `N/record.create`, `N/record.copy`, `N/record.transform`, `N/record.load`)
- `script_types`: `client`, `server`
- `not_supported`: `create`, `copy`
- `script_functions`: `update`, `delete`, `search`
- `field_id_pattern`: `landedcostamount<n>`, `landedcostsource<n>`
- `requires_feature`: `Advanced Receiving` [`real_name`]
- `list_sublists`: `item`
- `create_via`: `transform_from_purchaseorder` (refs `N/record.transform`)
- `cannot`: `create_standalone`
- `purpose`: `receipt_of_items_returned_by_customers`
- `effect`: `return_items_marked_received`, `stock_levels_updated`, `inventory_asset_accounts_updated`, `return_status_updated`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `class` | select | Class | no | no |  | `purpose`: `transaction_class`; `allowed_values_listed_in`: `class_records`; `ui_effect`: `new_option_creates_record` |
| `createdfrom` | select | Created From | no | no |  | `purpose`: `created_from_purchase_order`; `value_source`: `system_displayed` |
| `currency` | select | Currency | no | no |  | `purpose`: `currency`; `value_source`: `purchase_order_currency` |
| `department` | select | Department | no | no |  | `purpose`: `transaction_department`; `allowed_values_listed_in`: `department_records`; `ui_effect`: `new_option_creates_record` |
| `entity` | select | Vendor | no | no |  | `purpose`: `vendor_entity`; `value_source`: `system_displayed` |
| `exchangerate` | currency2 | Exchange Rate | yes | no |  | `purpose`: `exchange_rate`; `can`: `update_currency_record_rate` |
| `landedcostperline` | checkbox | Landed Cost per Line | no | no |  | `purpose`: `landed_cost_per_line_default`; `doc_inconsistency`: `receipt_field_described_as_global_default` |
| `memo` | text | Memo | no | yes |  | `purpose`: `memo`; `supported_in`: `search` |
| `postingperiod` | select | Posting Period | no | no |  | `purpose`: `posting_period`; `cannot`: `post_to_closed_period` |
| `subsidiary` | select | Subsidiary | no | no |  | `purpose`: `subsidiary`; `value_source`: `system_displayed` |
| `trandate` | date | Date | yes | no |  | `purpose`: `receipt_date` |
| `tranid` | text | Reference # | no | no |  | `purpose`: `receipt_number`; `default_behavior`: `highest_number_plus_one`; `caveat`: `next_number_reverts_to_standard_pattern` |
| `createddate` | datetime |  | no | no |  |  |
| `currencyname` | text | Currency | no | no |  |  |
| `currencysymbol` | text | Currency Symbol | no | no |  |  |
| `customform` | select | Custom Form | yes | no |  |  |
| `externalid` | text | ExternalId | no | no |  |  |
| `inboundshipment` | select | Inbound Shipment | no |  |  |  |
| `isbasecurrency` | checkbox | Base Currency | no | no |  |  |
| `itemfulfillment` | select | Item Fulfillment | no | no |  |  |
| `lastmodifieddate` | datetime |  | no | no |  |  |
| `location` | select | To Location | no | no |  |  |

## Sublist `accountingbookdetail` (Accounting Books)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `accountingbook` | select | Secondary Book | no |  |  |
| `exchangerate` | currency2 | Exchange Rate | yes |  |  |

## Sublist `expense` (Expenses)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `amount` | currency | Amount | no |  |  |
| `markreceived` | checkbox | Mark Received | no |  |  |

## Sublist `item` (Items)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `class` | text | Class | no |  |  |
| `countryofmanufacture` | text |  | no |  |  |
| `department` | text | Department | no |  |  |
| `inventorydetail` | summary | Inventory Detail | no |  |  |
| `item` | text |  | no |  |  |
| `itemdescription` | text | Description | no |  |  |
| `itemkey` | text | Item | no |  |  |
| `itemname` | text | Item | no |  |  |
| `itemreceive` | checkbox | Receive | no |  |  |
| `itemtype` | text |  | no |  |  |
| `line` | text |  | no |  |  |
| `location` | select | To Location | yes |  |  |
| `manufactureraddr1` | text |  | no |  |  |
| `manufacturercity` | text |  | no |  |  |
| `manufacturerstate` | text |  | no |  |  |
| `manufacturertariff` | text |  | no |  |  |
| `manufacturertaxid` | text |  | no |  |  |
| `manufacturerzip` | text |  | no |  |  |
| `multmanufactureaddr` | text |  | no |  |  |
| `onhand` | float | On Hand | no |  |  |
| `options` | namevaluelist | Options | no |  |  |
| `preferencecriterion` | text |  | no |  |  |
| `producer` | text |  | no |  |  |
| `quantity` | posfloat | Quantity | no |  |  |
| `quantityremaining` | float |  | no |  |  |
| `restock` | checkbox | Restock? | no |  |  |
| `schedulebcode` | text |  | no |  |  |
| `schedulebnumber` | text |  | no |  |  |
| `schedulebquantity` | text |  | no |  |  |
| `sitemname` | text | Item | no |  |  |
| `units` | text |  | no |  |  |

## Tabs

| Tab ID | Label |
|---|---|
| `custom` | Custom |
| `history` | History |
| `items` | Items |
| `landedcost` | Landed Cost |
| `workflow` | Workflow |

## Suspected source errors

> [!SOURCE-ERROR] `receipt_field_described_as_global_default` (on `landedcostperline`)
<!-- source-error: record:itemreceipt | landedcostperline | receipt_field_described_as_global_default -->
