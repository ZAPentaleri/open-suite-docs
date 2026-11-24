# Blanket Purchase Order (`blanketpurchaseorder`)
<!-- id: record:blanketpurchaseorder -->

| Fact | Value |
|---|---|
| Internal ID | `blanketpurchaseorder` |
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
- `requires_feature`: `Blanket Purchase Order` [`real_name`]
- `user_events`: `beforeLoad`, `beforeSubmit`, `afterSubmit`
- `purpose`: `fixed_pricing_for_preset_quantity_over_period`
- `configured_by`: `enable_features_transactions_subtab` [`blanket_purchase_order`]
- `ui_location`: `transactions_purchases_enter_blanket_purchase_order`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `billedamount` | currency | Billed Amount | no | no |  | `purpose`: `total_billed_for_orders_from_contract`; `value_source`: `summary_of_created_orders` |
| `billinginstructions` | textarea | Billing Instructions | no | no |  | `purpose`: `vendor_instructions_billing` |
| `carrier` | text | Carrier | no | no |  | `purpose`: `shipping_carrier`; `use_when`: `buyer_has_carrier_account` |
| `class` | select | Class | no | no |  | `purpose`: `class`; `doc_inconsistency`: `help_describes_employee_class`; `allowed_values_listed_in`: `class_records`; `ui_effect`: `new_option_creates_record` |
| `currency` | select | Currency | no | no |  | `purpose`: `currency_of_amounts` |
| `customform` | select | Custom Form | no | no |  | `purpose`: `custom_form`; `doc_inconsistency`: `help_describes_employee_form` |
| `department` | select | Department | no | no |  | `purpose`: `department`; `allowed_values_listed_in`: `department_records`; `ui_effect`: `new_option_creates_record` |
| `employee` | select | Employee | no | no |  | `purpose`: `associated_employee` |
| `enddate` | date | End Date | no | no |  | `purpose`: `last_date_for_orders_in_schedule` |
| `entity` | select | Vendor | yes | no |  | `purpose`: `vendor_or_customer_providing_goods`; `accepts_forms`: `customer`, `vendor`, `employee` |
| `exchangerate` | currency2 | Exchange Rate | yes | no |  | `purpose`: `exchange_rate`; `default_value`: `1`; `recommendation`: `do_not_change`; `caveat`: `reserved_for_future_multi_currency` |
| `incoterm` | select | Incoterm | no | no |  | `purpose`: `incoterm`; `value_kind`: `three_letter_trade_term`; `allowed_values`: `EXW`, `FCA`, `CPT`, `CIP`, `DAT`, `DAP`, `DDP`, `FAS`, `FOB`, `CFR`, `CIF` [`standard_set`]; `effect`: `defines_where_customer_takes_ownership`; `comparable_to`: `fob_terms`; `option_meanings`: `exw:ex_works`, `fca:free_carrier`, `cpt:carriage_paid_to`, `cip:carriage_and_insurance_paid`, `dat:delivered_at_terminal`, `dap:delivered_at_place`, `ddp:delivered_duty_paid`, `fas:free_alongside_ship`, `fob:free_on_board`, `cfr:cost_and_freight`, `cif:cost_insurance_and_freight` |
| `location` | select | Location | no | no |  | `purpose`: `location` |
| `maximumamount` | poscurrency | Maximum Amount | no | no |  | `purpose`: `maximum_amount_for_approval_level`; `applies_when`: `approval_routing` |
| `memo` | text | Memo | no | yes |  | `purpose`: `memo`; `doc_inconsistency`: `help_describes_deletion_reason_comment` |
| `otherrefnum` | text | Vendor # | no | no |  | `purpose`: `entity_reference_number` |
| `packinglistinstructions` | textarea | Packing List Instructions | no | no |  | `purpose`: `vendor_instructions_packing_list` |
| `productlabelinginstructions` | textarea | Product Labeling Instructions | no | no |  | `purpose`: `vendor_instructions_product_labeling` |
| `purchasedamount` | currency | Purchased Amount | no | no |  | `purpose`: `total_purchased_for_orders_from_contract`; `value_source`: `summary_of_created_orders` |
| `purchaseorderinstructions` | textarea | Purchase Order Instructions | no | no |  | `purpose`: `vendor_instructions_purchase_order` |
| `receivedamount` | currency | Received Amount | no | no |  | `purpose`: `total_received_for_orders_from_contract`; `value_source`: `summary_of_created_orders` |
| `startdate` | date | Start Date | yes | no |  | `purpose`: `first_date_for_orders_in_schedule` |
| `subsidiary` | select | Subsidiary | no | no |  | `purpose`: `subsidiary`; `requires_feature`: `OneWorld` [`real_name`]; `default_behavior`: `current_user_subsidiary`; `allowed_values`: `vendor_assigned_subsidiaries` (when `shared_vendor`); `ui_effect`: `item_and_classification_fields_resourced` |
| `terms` | select | Payment Terms | no | no |  | `purpose`: `payment_terms`; `effect`: `copied_to_associated_purchase_orders` |
| `trandate` | date | Date | yes | no |  | `purpose`: `transaction_date`; `default_behavior`: `current_date` |
| `tranid` | text | Blanket Purchase Order # | no | no |  | `purpose`: `transaction_number`; `default_behavior`: `next_number` (when `auto_generated_numbering`) |
| `createddate` | datetime |  | no | no |  |  |
| `createdfrom` | select | Created From | no | no |  |  |
| `currencyname` | text | Currency | no | no |  |  |
| `currencysymbol` | text | Currency Symbol | no | no |  |  |
| `effectivitybasedon` | select | Effectivity Based On | yes | no |  |  |
| `entitynexus` | select | Nexus | no | no |  |  |
| `externalid` | text | ExternalId | no | no |  |  |
| `isbasecurrency` | checkbox | Base Currency | no | no |  |  |
| `lastmodifieddate` | datetime |  | no | no |  |  |
| `nexus` | select | Nexus | no | no |  |  |
| `oldrevenuecommitment` | text |  | no | no |  |  |
| `orderstatus` | text |  | no | no |  |  |
| `source` | text | Source | no | no |  |  |
| `status` | text | Status | no | no |  |  |
| `statusRef` | text |  | no | no |  |  |
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
| `amountordered` | currency | Amount Ordered | no |  |  |
| `category` | select | Category | no |  |  |
| `class` | select | Class | no |  |  |
| `department` | select | Department | no |  |  |
| `line` | text |  | no |  |  |
| `location` | select | Location | no |  |  |
| `memo` | text | Memo | no |  |  |
| `orderschedule` | summary | Schedule | no |  |  |

## Sublist `item` (Item)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `amount` | currency | Amount | no |  |  |
| `billvariancestatus` | text |  | no |  |  |
| `catchupperiod` | select |  | no |  |  |
| `class` | select | Class | no |  |  |
| `deferrevrec` | checkbox |  | no |  |  |
| `department` | select | Department | no |  |  |
| `description` | textarea | Description | no |  |  |
| `id` | text |  | no |  |  |
| `isvsoebundle` | text |  | no |  |  |
| `item` | select | Item | yes |  |  |
| `itemsubtype` | text |  | no |  |  |
| `itemtype` | text |  | no |  |  |
| `line` | text |  | no |  |  |
| `linenumber` | integer |  | no |  |  |
| `location` | select | Location | no |  |  |
| `matrixtype` | text |  | no |  |  |
| `options` | text |  | no |  |  |
| `orderschedule` | summary | Schedule | no |  |  |
| `quantity` | float | Quantity | no |  |  |
| `quantityordered` | float | Quantity Ordered | no |  |  |
| `rate` | rate | Rate | no |  |  |
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

## Suspected source errors

> [!SOURCE-ERROR] `help_describes_employee_class` (on `class`)
<!-- source-error: record:blanketpurchaseorder | class | help_describes_employee_class -->

> [!SOURCE-ERROR] `help_describes_employee_form` (on `customform`)
<!-- source-error: record:blanketpurchaseorder | customform | help_describes_employee_form -->

> [!SOURCE-ERROR] `help_describes_deletion_reason_comment` (on `memo`)
<!-- source-error: record:blanketpurchaseorder | memo | help_describes_deletion_reason_comment -->
