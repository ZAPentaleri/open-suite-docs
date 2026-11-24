# Purchase Order (`purchaseorder`)
<!-- id: record:purchaseorder -->

| Fact | Value |
|---|---|
| Internal ID | `purchaseorder` |
| Category | transaction |
| Client scripts | yes |
| Server scripts | yes |
| Searchable | yes |
| Search data via | Transaction |
| Tags | `custom_fields_supported`, `scripting_full` |

## Record facts

- `initialization_default_value`: `entity`, `entity_internal_id` (refs `N/record.create`, `N/record.copy`, `N/record.transform`, `N/record.load`)
- `script_types`: `client`, `server`
- `script_functions`: `create`, `update`, `copy`, `delete`, `search`, `transform`
- `body_only_scriptable_fields`: `estgrossprofit`, `estgrossprofitpercent`, `totalcostestimate`
- `before_load_runs_on_email_operation`: `true`
- `before_load_skipped_when`: `use_popup_for_main_transaction_email_button`
- `purpose`: `authorizes_purchase_tracks_expected_and_received_items`
- `effect`: `no_gl_impact_until_items_received`
- `requires_feature`: `Purchase Orders` [`real_name`]
- `doc_inconsistency`: `fields_table_has_no_rows`
- `before_load_skipped_when`: `view_actions_email_with_popup_preference` [`email_popup_path`]
- `before_load_skipped_when`: `communications_subtab_email_with_popup_preference` [`email_popup_path`]
- `caveat`: `email_popup_actions_are_not_transaction_operations`
- `workaround`: `deploy_before_load_on_message_record` (when `emails_sent_from_popup`)
- `effect`: `save_and_email_always_triggers_before_load`
- `configured_by`: `use_popup_for_main_transaction_email_button_preference`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `approvalstatus` | select | Approval Status | no | no |  | `purpose`: `approval_status`; `applies_when`: `suiteflow_purchase_approvals`; `allowed_values`: `Pending Approval`, `Approved`, `Rejected`; `can`: `override` |
| `availablevendorcredit` | currency | Available Vendor Credit | no | no |  | `purpose`: `available_vendor_credit`; `effect`: `subsidiary_change_converts_currency`; `requires`: `form_customized_to_show_field` |
| `billaddress` | address | Vendor | no | no |  | `purpose`: `vendor_billing_address`; `default_behavior`: `vendor_mailing_address`; `options`: `New`, `Custom`; `option_meanings`: `new:saved_to_vendor`, `custom:transaction_only` |
| `class` | select | Class | no | no |  | `purpose`: `class`; `allowed_values_listed_in`: `class_records`; `ui_effect`: `new_option_creates_record`; `help_text_shared_with`: `purchaseorder.expense.class`, `purchaseorder.item.class` |
| `currency` | select | Currency | yes | no |  | `purpose`: `vendor_currency`; `requires_one_of`: `multiple_currencies`, `multi_currency_vendors` (when `choosing_other_currency`) |
| `customform` | select | Custom Form | yes | no |  | `purpose`: `custom_form`; `ui_effect`: `new_option_creates_record` |
| `department` | select | Department | no | no |  | `purpose`: `department`; `allowed_values_listed_in`: `department_records`; `ui_effect`: `new_option_creates_record`; `help_text_shared_with`: `purchaseorder.expense.department`, `purchaseorder.item.department` |
| `duedate` | date | Receive By | no | no |  | `purpose`: `needed_by_date` |
| `employee` | select | Employee | no | no |  | `purpose`: `requesting_employee`; `requires`: `supervisor_approval_when_supervisor_exists`; `allowed_values_listed_in`: `employee_records`; `ui_effect`: `new_option_creates_record` |
| `entity` | select | Vendor | yes | no |  | `purpose`: `vendor`; `allowed_values_listed_in`: `vendor_records`; `ui_effect`: `new_option_creates_record`; `ui_effect`: `subsidiary_change_resources_items_and_classification` (when `oneworld`) |
| `entitytaxregnum` | select | Tax Reg. Number | no |  |  | `purpose`: `vendor_tax_registration` |
| `exchangerate` | currency2 | Exchange rate | yes | no |  | `purpose`: `exchange_rate`; `can`: `update_currency_record_rate` |
| `fob` | text | FOB | no | no |  | `purpose`: `fob_ownership_location`; `value_examples`: `Shipping point` |
| `incoterm` | select | Incoterm | no | no |  | `purpose`: `incoterm`; `effect`: `defines_where_customer_takes_ownership` |
| `intercostatus` | select | Intercompany Status | no | no |  | `purpose`: `intercompany_status`; `allowed_values`: `Linked`, `Pending`, `Rejected`; `option_meanings`: `linked:paired_with_other_subsidiary_order`, `pending:not_paired`, `rejected:cannot_pair_incorrect_details` |
| `intercotransaction` | select | Paired Intercompany Transaction | no | no |  | `purpose`: `paired_intercompany_order`; `allowed_values`: `sales_order_purchase_order`, `vendor_return_return_authorization` |
| `location` | select | Location | no | no |  | `purpose`: `location`; `allowed_values_listed_in`: `location_records`; `ui_effect`: `new_option_creates_record`; `help_text_shared_with`: `purchaseorder.expense.location` |
| `memo` | text | Memo | no | yes |  | `purpose`: `memo`; `displayed_on`: `registers`; `help_text_shared_with`: `purchaseorder.expense.memo` |
| `message` | textarea | Vendor Message | no | no |  | `purpose`: `vendor_message`; `displayed_on`: `printed_forms` |
| `nextapprover` | select | Next Approver | no | no |  | `purpose`: `next_approver`; `value_source`: `approval_workflow`; `can`: `override`; `caveat`: `changes_during_approval_chain`; `applies_when`: `suiteflow_purchase_approvals` |
| `nexus` | select | Nexus | no | no |  | `purpose`: `transaction_nexus`; `value_source`: `nexus_lookup`; `can`: `override_with_nexus_override` |
| `otherrefnum` | text | Vendor # | no | no |  | `purpose`: `vendor_account_number`; `default_behavior`: `vendor_record_account_number` |
| `purchasecontract` | select | Purchase Contract | no | no |  | `purpose`: `purchase_contract`; `value_source`: `system_displayed` |
| `shipaddress` | address | Ship To | no | no |  | `purpose`: `ship_to_address`; `default_behavior`: `ship_to_customer_default_address`; `default_behavior`: `company_shipping_address` (when `no_ship_to_customer`) |
| `shipdate` | date | Ship Date | no | no |  | `purpose`: `expected_ship_date` |
| `shipmethod` | select | Ship Via | no | no |  | `purpose`: `ship_method` |
| `shipto` | select | Ship To | no | no |  | `purpose`: `drop_ship_customer`; `ui_effect`: `new_option_creates_record` |
| `subsidiary` | select | Subsidiary | no | no |  | `purpose`: `subsidiary`; `value_source`: `system_displayed` |
| `subsidiarytaxregnum` | select | Tax Reg. Number | no |  |  | `purpose`: `subsidiary_tax_registration`; `value_source`: `nexus_lookup`; `can`: `override_with_nexus_override` |
| `supervisorapproval` | checkbox | Supervisor Approval | no | no |  | `purpose`: `supervisor_approval`; `requires_permission`: `approve_purchase_order` |
| `taxdetailsoverride` | checkbox | Tax Details Override | no |  |  | `purpose`: `override_tax_details`; `requires_permission`: `tax_details_tab_edit` |
| `taxpointdate` | date | Tax Point Date | no | no |  | `purpose`: `tax_point_date`; `value_kind`: `date_determining_taxability` |
| `taxpointdateoverride` | checkbox | Override | no | no |  | `purpose`: `override_tax_point_date` |
| `taxregoverride` | checkbox | Tax Registration Override | no |  |  | `purpose`: `override_nexus_and_tax_registration`; `requires_permission`: `tax_details_tab_edit` |
| `terms` | select | Terms | no | no |  | `purpose`: `discount_terms`; `allowed_values_listed_in`: `accounting_list_terms` |
| `tobeemailed` | checkbox | To Be E-mailed | no | no |  | `purpose`: `email_transaction`; `format`: `semicolon_separated_no_spaces` |
| `tobefaxed` | checkbox | To Be Faxed | no | no |  | `purpose`: `fax_transaction`; `requires`: `fax_service_set_up` |
| `tobeprinted` | checkbox | To Be Printed | no | yes |  | `purpose`: `print_later`; `true_means`: `queued_for_printing` |
| `total` | currency | Total | no | no |  | `purpose`: `total`; `value_source`: `sum_of_lines` |
| `trackingnumbers` | text | Additional Tracking # | no | no |  | `purpose`: `tracking_numbers`; `ui_effect`: `ups_fedex_numbers_become_links` |
| `trandate` | date | Date | yes | no |  | `purpose`: `posting_date`; `default_behavior`: `current_date`; `displayed_on`: `registers` |
| `tranid` | text | PO # | no | no |  | `purpose`: `purchase_order_number`; `default_behavior`: `highest_number_plus_one`; `max_length`: `45` characters; `caveat`: `next_number_reverts_to_standard_pattern` |
| `balance` | currency |  | no | no |  |  |
| `billingaddress` | summary |  | no | no |  |  |
| `billisresidential` | text | Shipping address is residential | no | no |  |  |
| `createddate` | datetime |  | no | no |  |  |
| `createdfrom` | select | Created From | no | no |  |  |
| `currencyname` | text | Currency | no | no |  |  |
| `currencysymbol` | text | Currency Symbol | no | no |  |  |
| `email` | emails |  | no | yes |  |  |
| `entitynexus` | select | Nexus | no | no |  |  |
| `externalid` | text | ExternalId | no | no |  |  |
| `isbasecurrency` | checkbox | Base Currency | no | no |  |  |
| `lastmodifieddate` | datetime |  | no | no |  |  |
| `linkedtrackingnumbers` | text | Tracking # | no | no |  |  |
| `orderstatus` | text |  | no | no |  |  |
| `returntrackingnumbers` | text | Return Tracking # | no | no |  |  |
| `shipisresidential` | text | Shipping address is residential | no | no |  |  |
| `shipoverride` | text |  | no | no |  |  |
| `shippingaddress` | summary |  | no | no |  |  |
| `source` | text | Source | no | no |  |  |
| `status` | text |  | no | no |  |  |
| `statusref` | text |  | no | no |  |  |
| `unbilledorders` | currency |  | no | no |  |  |

## Sublist `expense` (Expenses)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `class` | select | Class | no |  | `purpose`: `class`; `allowed_values_listed_in`: `class_records`; `ui_effect`: `new_option_creates_record`; `help_text_shared_with`: `purchaseorder.class` |
| `department` | select | Department | no |  | `purpose`: `department`; `allowed_values_listed_in`: `department_records`; `ui_effect`: `new_option_creates_record`; `help_text_shared_with`: `estimate.department`, `purchaseorder.department` |
| `location` | select | Location | no |  | `purpose`: `location`; `allowed_values_listed_in`: `location_records`; `ui_effect`: `new_option_creates_record`; `help_text_shared_with`: `purchaseorder.location` |
| `memo` | text | Memo | no |  | `purpose`: `memo`; `displayed_on`: `registers`; `help_text_shared_with`: `purchaseorder.memo` |
| `account` | select | Account | yes |  |  |
| `amount` | currency | Amount | yes |  |  |
| `category` | select | Category | no |  |  |
| `createdfrom` | select | Created From | no |  |  |
| `customer` | select | Customer | no |  |  |
| `isbillable` | checkbox | Billable | no |  |  |
| `isclosed` | checkbox | Closed | no |  |  |
| `line` | text |  | no |  |  |
| `linkedorder` | select | Linked Order | no |  |  |

## Sublist `item` (Items)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `class` | select | Class | no |  | `purpose`: `class`; `allowed_values_listed_in`: `class_records`; `ui_effect`: `new_option_creates_record`; `help_text_shared_with`: `purchaseorder.class` |
| `department` | select | Department | no |  | `purpose`: `department`; `allowed_values_listed_in`: `department_records`; `ui_effect`: `new_option_creates_record`; `help_text_shared_with`: `estimate.department`, `purchaseorder.department` |
| `id` | text |  | no |  | `purpose`: `line_internal_id` |
| `itemsubtype` | text |  | no |  | `purpose`: `item_subtype`; `value_examples`: `For Purchase`, `For Resale`, `For Sale` |
| `itemtype` | text |  | no |  | `purpose`: `item_type`; `value_examples`: `Inventory Item`, `Service Item`, `Download Item` |
| `line` | text |  | no |  | `purpose`: `line_id`; `caveat`: `gaps_remain_after_line_removal`; `value_examples`: `1, 3, 4 after removing line 2` |
| `linenumber` | integer |  | no |  | `purpose`: `line_number`; `value_kind`: `always_sequential_without_gaps`; `comparable_to`: `line`; `value_examples`: `1, 2, 3 after removing line 2` |
| `amount` | currency | Amount | no |  |  |
| `assembly` | select | Assembly | yes |  |  |
| `assemblyunits` | select | Assembly Units | no |  |  |
| `billofmaterials` | select | Bill of Materials | yes |  |  |
| `billofmaterialsrevision` | select | Bill of Materials Revision | yes |  |  |
| `billvariancestatus` | text |  | no |  |  |
| `catchupperiod` | select |  | no |  |  |
| `createdfrom` | select | Created From | no |  |  |
| `createoutsourcedwo` | checkbox | Create WO | no |  |  |
| `customer` | select | Customer | no |  |  |
| `deferrevrec` | checkbox |  | no |  |  |
| `description` | textarea | Description | no |  |  |
| `excludefrompredictiverisk` | checkbox | Exclude From Predictive Risk | no |  |  |
| `expectedreceiptdate` | date | Expected Receipt Date | no |  |  |
| `inventorydetail` | summary | Inventory Detail | no |  |  |
| `isbillable` | checkbox | Billable | no |  |  |
| `isclosed` | checkbox | Closed | no |  |  |
| `isvsoebundle` | text |  | no |  |  |
| `item` | select | Item | yes |  |  |
| `landedcostcategory` | select | Landed Cost Category | no |  |  |
| `leadtime` | integer |  | no |  |  |
| `linkedorder` | select | Linked Order | no |  |  |
| `matchbilltoreceipt` | checkbox | Match Bill To Receipt | no |  |  |
| `matrixtype` | text |  | no |  |  |
| `options` | namevaluelist | Options | no |  |  |
| `printitems` | text |  | no |  |  |
| `productionenddate` | date | Production End Date | no |  |  |
| `productionstartdate` | date | Production Start Date | no |  |  |
| `purchasecontract` | select | Purchase Contract | no |  |  |
| `quantity` | float | Quantity | no |  |  |
| `quantitybilled` | float | Billed | no |  |  |
| `quantityonshipments` | float | Quantity On Shipments | no |  |  |
| `quantityreceived` | float | Received | no |  |  |
| `rate` | rate | Rate | no |  |  |
| `rateschedule` | text |  | no |  |  |
| `units` | select | Units | no |  |  |
| `vendorname` | text | Vendor Name | no |  |  |

## Sublist `accountingbookdetail` (Accounting Books)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `accountingbook` | select | Secondary Book | no |  |  |
| `exchangerate` | currency2 | Exchange Rate | yes |  |  |

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
| `address` | Address |
| `custom` | Custom |
| `history` | History |
| `items` | Items |
| `output` | Messages |
| `shipping` | Shipping |
| `workflow` | Workflow |

## Transform targets

| Target record | Default fields |
|---|---|
| `custompurchase` |  |
| `itemreceipt` | `customform` |
| `vendorbill` | `customform`, `entity`, `poids` |

## Suspected source errors

> [!SOURCE-ERROR] `fields_table_has_no_rows`
<!-- source-error: record:purchaseorder | - | fields_table_has_no_rows -->
