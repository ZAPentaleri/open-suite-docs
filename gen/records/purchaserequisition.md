# Requisition (`purchaserequisition`)
<!-- id: record:purchaserequisition -->

| Fact | Value |
|---|---|
| Internal ID | `purchaserequisition` |
| Category | transaction |
| Client scripts | yes |
| Server scripts | yes |
| Searchable | yes |
| Search data via | Transaction |
| Tags | `custom_fields_supported`, `scripting_full` |

## Record facts

- `script_types`: `client`, `server`
- `script_functions`: `create`, `update`, `copy`, `delete`, `search`, `transform`
- `user_events`: `beforeLoad`, `beforeSubmit`, `afterSubmit`
- `requires_feature`: `Requisitions` [`real_name`]
- `purpose`: `start_purchase_process_for_internal_needs`
- `configured_by`: `enable_features_transactions_subtab` [`requisitions`]
- `ui_location`: `transactions_purchases_vendors_enter_requisitions`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `approvalstatus` | select | Approval Status | no | no |  | `purpose`: `approval_status`; `value_source`: `system_displayed` |
| `class` | select | Class | no | no |  | `purpose`: `class`; `constraint`: `from_requestor_subsidiary` (when `oneworld`) |
| `currency` | select | Currency | no | no |  | `purpose`: `currency`; `value_source`: `requestor_currency`; `effect`: `all_amounts_in_this_currency` |
| `customform` | select | Custom Form | yes | no |  | `purpose`: `entry_form`; `special_values`: `Standard Requisition`; `ui_effect`: `new_option_creates_record` |
| `department` | select | Department | no | no |  | `purpose`: `department`; `constraint`: `from_requestor_subsidiary` (when `oneworld`) |
| `duedate` | date | Receive By | no | no |  | `purpose`: `receive_by_date` |
| `entity` | select | Requestor | yes | no |  | `purpose`: `requestor`; `value_examples`: `consultant` |
| `estimatedtotal` | currency | Estimated Total | no | no |  | `purpose`: `estimated_total`; `value_source`: `sum_of_items_and_expenses` |
| `location` | select | Location | yes | no |  | `purpose`: `location`; `constraint`: `from_requestor_subsidiary` (when `oneworld`) |
| `memo` | text | Memo | no | yes |  | `purpose`: `memo`; `supported_in`: `search` |
| `nextapprover` | select | Next Approver | no | no |  | `purpose`: `next_approver`; `value_source`: `approval_workflow`; `can`: `override`; `caveat`: `changes_during_approval_chain`; `applies_when`: `suiteflow_purchase_approvals` |
| `subsidiary` | select | Subsidiary | no | no |  | `purpose`: `subsidiary`; `can`: `change_to_vendor_assigned_subsidiaries` (when `oneworld_shared_vendor`) |
| `total` | currency | Total | no | no |  | `purpose`: `actual_total`; `value_source`: `created_purchase_orders` |
| `trandate` | date | Date | yes | no |  | `purpose`: `requisition_date`; `default_behavior`: `current_date` |
| `tranid` | text | Req # | no | no |  | `purpose`: `requisition_number`; `default_behavior`: `highest_number_plus_one`; `max_length`: `45` characters; `doc_inconsistency`: `stray_html_tag_in_help` |
| `createddate` | datetime |  | no | no |  |  |
| `createdfrom` | select | Created From | no | no |  |  |
| `currencyname` | text | Currency | no | no |  |  |
| `currencysymbol` | text | Currency Symbol | no | no |  |  |
| `entity_nexus_country` | text | Country | no | no |  |  |
| `entitynexus` | select | Nexus | no | no |  |  |
| `exchangerate` | currency2 | Exchange rate | yes | no |  |  |
| `externalid` | text | ExternalId | no | no |  |  |
| `fob` | text | FOB | no | no |  |  |
| `isbasecurrency` | checkbox | Base Currency | no | no |  |  |
| `lastmodifieddate` | datetime |  | no | no |  |  |
| `linkedtrackingnumbers` | text | Tracking # | no | no |  |  |
| `message` | textarea | Vendor Message | no | no |  |  |
| `messagesel` | select | Select Message | no | no |  |  |
| `nexus` | select | Nexus | no | no |  |  |
| `nexus_country` | text | Country | no | no |  |  |
| `oldrevenuecommitment` | text |  | no | no |  |  |
| `returntrackingnumbers` | text | Return Tracking # | no | no |  |  |
| `shipdate` | date | Ship Date | no | no |  |  |
| `source` | text | Source | no | no |  |  |
| `status` | text | Status | no | no |  |  |
| `statusRef` | text |  | no | no |  |  |
| `terms` | select | Terms | no | no |  |  |
| `trackingnumbers` | text | Additional Tracking # | no | no |  |  |
| `updatecurrency` | text | Update Currency | no | no |  |  |

## Sublist `accountingbookdetail` (Accounting Books)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `accountingbook` | select | Secondary Book | no |  |  |
| `exchangerate` | currency2 | Exchange Rate | yes |  |  |

## Sublist `expense` (Expenses)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `account` | select | Account | yes |  |  |
| `amount` | currency | Amount | yes |  |  |
| `category` | select | Category | no |  |  |
| `class` | select | Class | no |  |  |
| `customer` | select | Customer | no |  |  |
| `department` | select | Department | no |  |  |
| `estimatedamount` | currency |  | no |  |  |
| `isbillable` | checkbox | Billable | no |  |  |
| `isclosed` | checkbox | Closed | no |  |  |
| `line` | text |  | no |  |  |
| `linkedorder` | select | Linked Order | no |  |  |
| `linkedorderstatus` | text | Linked Order Status | no |  |  |
| `location` | select | Location | no |  |  |
| `memo` | text | Memo | no |  |  |
| `povendor` | select |  | no |  |  |

## Sublist `item` (Items)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `amount` | currency | Amount | no |  |  |
| `billvariancestatus` | text |  | no |  |  |
| `catchupperiod` | select |  | no |  |  |
| `class` | select | Class | no |  |  |
| `createpo` | text |  | no |  |  |
| `custcol_contact` | select | Recipient | no |  |  |
| `custcol_css_core_api_sourced_column` | text | Item Sourced Field | no |  |  |
| `custcol_formula` | integer | Formula ( Default Value) | no |  |  |
| `custcol_textanormal` | text | Text A Normal | no |  |  |
| `customer` | select | Customer | no |  |  |
| `deferrevrec` | checkbox |  | no |  |  |
| `department` | select | Department | no |  |  |
| `description` | textarea | Description | no |  |  |
| `estimatedamount` | currency | Estimated Amount | no |  |  |
| `estimatedrate` | currency | Estimated Rate | no |  |  |
| `expectedreceiptdate` | date | Expected Receipt Date | no |  |  |
| `id` | text |  | no |  |  |
| `inventorydetail` | summary | Inventory Detail | no |  |  |
| `isbillable` | checkbox | Billable | no |  |  |
| `isclosed` | checkbox | Closed | no |  |  |
| `isvsoebundle` | text |  | no |  |  |
| `item` | select | Item | yes |  |  |
| `itemsubtype` | text |  | no |  |  |
| `itemtype` | text |  | no |  |  |
| `line` | text |  | no |  |  |
| `linenumber` | integer |  | no |  |  |
| `linkedorder` | select | Linked Order | no |  |  |
| `linkedorderstatus` | text | Linked Order Status | no |  |  |
| `matrixtype` | text |  | no |  |  |
| `options` | namevaluelist | Options | no |  |  |
| `porate` | currency2 |  | no |  |  |
| `povendor` | select | Vendor | no |  |  |
| `quantity` | float | Quantity | no |  |  |
| `quantityreceived` | float | Received | no |  |  |
| `rate` | rate | Rate | no |  |  |
| `rateschedule` | text |  | no |  |  |
| `serialnumbers` | textarea | Serial/Lot Numbers | no |  |  |
| `units` | select | Units | no |  |  |
| `vendorname` | text | Vendor Name | no |  |  |

## Sublist `links` (Purchase Orders)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `id` | text |  | no |  |  |

## Tabs

| Tab ID | Label |
|---|---|
| `accntingtab` | Accounting |
| `address` | Address |
| `cmmnctntab` | Communication |
| `custom` | Custom |
| `history` | History |
| `items` | Items |
| `rlrcdstab` | Related Records |
| `rltnshptab` | Relationships |
| `s_sysinfo` | System Information |
| `shipping` | Shipping |
| `workflow` | Workflow |

## Transform targets

| Target record | Default fields |
|---|---|
| `purchaseorder` | `customform` |

## Suspected source errors

> [!SOURCE-ERROR] `stray_html_tag_in_help` (on `tranid`)
<!-- source-error: record:purchaserequisition | tranid | stray_html_tag_in_help -->
