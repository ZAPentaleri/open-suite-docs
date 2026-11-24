# Check (`check`)
<!-- id: record:check -->

| Fact | Value |
|---|---|
| Internal ID | `check` |
| Category | transaction |
| Client scripts | yes |
| Server scripts | yes |
| Searchable | yes |
| Search data via | Transaction |
| Tags | `custom_fields_supported`, `scripting_full` |

## Record facts

- `initialization_default_value`: `entity`, `entity_internal_id` (refs `N/record.create`, `N/record.copy`, `N/record.transform`, `N/record.load`)
- `script_types`: `client`, `server`
- `script_functions`: `create`, `update`, `copy`, `delete`, `search`
- `field_id_pattern`: `landedcostamount<n>`, `landedcostsource<n>`
- `address_subrecord`: `payee_address`
- `purpose`: `check_cash_expense_or_non_check_debit`
- `effect`: `debits_expense_account_credits_bank_account`
- `not_supported_in`: `search`, `lookup` [`amount_field_on_any_transaction`]
- `ui_effect`: `payee_address_shown_as_sublist`
- `risk`: `scripts_using_old_payee_address_text_may_break`
- `caveat`: `gross_profit_fields_not_scriptable_at_line_level`

## Body fields

| Field ID | Type | Label | Required | submitField | Read-only | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `account` | select | Account | no | no |  |  | `purpose`: `bank_account`; `editable_only_when`: `edit_mode`; `allowed_values_listed_in`: `bank_accounts`; `ui_effect`: `new_option_creates_record` |
| `balance` | currency | Balance | no | no | yes |  | `purpose`: `bank_account_balance` |
| `billpay` | checkbox | Bill Pay | no | no |  |  | `purpose`: `send_check_online`; `requires_feature`: `Online Bill Pay` [`real_name`]; `caveat`: `limited_transactions_included` |
| `class` | select | Class | no | no |  |  | `purpose`: `class`; `editable_only_when`: `edit_mode`; `allowed_values_listed_in`: `class_records`; `ui_effect`: `new_option_creates_record` |
| `currency` | select | Currency | yes | no |  |  | `purpose`: `check_currency`; `value_source`: `payee_currency` |
| `customform` | select | Custom Form | yes | no |  |  | `purpose`: `check_form`; `allowed_values_listed_in`: `standard_and_custom_check_forms` |
| `department` | select | Department | no | no |  |  | `purpose`: `department`; `editable_only_when`: `edit_mode`; `allowed_values_listed_in`: `department_records`; `ui_effect`: `new_option_creates_record` |
| `entity` | select | Payee | yes | no |  |  | `purpose`: `payee`; `editable_only_when`: `edit_mode`; `allowed_values_listed_in`: `entity_payees`; `ui_effect`: `new_option_creates_record` |
| `exchangerate` | currency2 | Exchange Rate | yes | no |  |  | `purpose`: `exchange_rate`; `can`: `update_currency_record_rate` |
| `landedcostperline` | checkbox | Landed Cost per Line | no | no |  |  | `purpose`: `landed_cost_per_line`; `true_means`: `landed_cost_entered_per_line` |
| `location` | select | Location | yes | no |  |  | `purpose`: `location`; `editable_only_when`: `edit_mode`; `allowed_values_listed_in`: `location_records`; `ui_effect`: `new_option_creates_record` |
| `memo` | text | Memo | no | yes |  |  | `purpose`: `memo`; `displayed_on`: `check_memo_line`; `help_text_shared_with`: `check.expense.memo` |
| `payeeaddresslist` | select | Address To Select | no | no |  |  | `purpose`: `payee_address_label`; `default_behavior`: `default_billing_address_label`; `value_source`: `payee_address_labels`; `ui_effect`: `new_option_creates_record` |
| `postingperiod` | select | Posting Period | yes | no |  |  | `purpose`: `posting_period`; `applies_when`: `accounting_periods_used`; `cannot`: `post_to_closed_period` |
| `subsidiary` | select | Subsidiary | no | no |  |  | `purpose`: `subsidiary`; `default_behavior`: `payee_primary_subsidiary` (when `oneworld`); `can`: `choose_secondary_subsidiary` (when `shared_vendor`); `ui_effect`: `resets_bank_account_currency_and_clears_lines` |
| `tobeprinted` | checkbox | To Be Printed | no | yes |  |  | `purpose`: `print_later`; `true_means`: `queued_for_printing` |
| `trandate` | date | Date | yes | no |  |  | `purpose`: `posting_date`; `default_behavior`: `current_date`; `displayed_on`: `registers` |
| `tranid` | rtext | Check # | no | no |  |  | `purpose`: `check_number`; `value_source`: `system_filled`; `default_value`: `To Print` (when `tobeprinted`); `default_behavior`: `highest_number_plus_one` (when `printing_now`); `accepts_forms`: `ach_atm_or_other_reference` |
| `transactionnumber` | text | Transaction Number | no | no |  |  | `purpose`: `transaction_number`; `default_value`: `To Be Generated` (when `auto_transaction_numbering`); `value_source`: `gapless_number_generated_on_save`; `cannot`: `overwrite` (when `auto_transaction_numbering`) |
| `usertotal` | currency | Amount | no | no |  |  | `purpose`: `check_amount`; `effect`: `updated_from_lines` |
| `cleared` | checkbox | Cleared | no |  |  |  |  |
| `cleareddate` | date | Date Cleared | no |  |  |  |  |
| `createddate` | datetime |  | no | no |  |  |  |
| `currencyname` | text | Currency | no | no |  |  |  |
| `currencysymbol` | text | Currency Symbol | no | no |  |  |  |
| `entitynexus` | select | Nexus | no | no |  |  |  |
| `externalid` | text | ExternalId | no | no |  |  |  |
| `isbasecurrency` | checkbox | Base Currency | no | no |  |  |  |
| `lastmodifieddate` | datetime |  | no | no |  |  |  |
| `nexus` | select | Nexus | no | no |  |  |  |
| `payeeaddress` | summary |  | no | no |  |  |  |
| `status` | text | Status | no | no |  |  |  |
| `statusref` | text |  | no | no |  |  |  |
| `total` | currency | Amount | no | no |  |  |  |

## Sublist `expense` (Expenses)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `account` | select | Account | yes |  | `purpose`: `expense_account`; `doc_inconsistency`: `expense_line_help_says_bank_account`; `ui_effect`: `new_option_creates_record` |
| `class` | select | Class | no |  | `purpose`: `line_class`; `allowed_values_listed_in`: `class_records`; `ui_effect`: `new_option_creates_record`; `help_text_shared_with`: `vendorbill.class` |
| `department` | select | Department | no |  | `purpose`: `line_department`; `allowed_values_listed_in`: `department_records`; `ui_effect`: `new_option_creates_record`; `help_text_shared_with`: `journalentry.department` |
| `location` | select | Location | no |  | `purpose`: `line_location`; `allowed_values_listed_in`: `location_records`; `ui_effect`: `new_option_creates_record`; `help_text_shared_with`: `creditcardrefund.location` |
| `memo` | text | Memo | no |  | `purpose`: `memo`; `displayed_on`: `check_memo_line`; `help_text_shared_with`: `check.memo` |
| `amount` | currency | Amount | yes |  |  |
| `category` | text |  | no |  |  |
| `customer` | select | Customer | no |  |  |
| `isbillable` | checkbox | Billable | no |  |  |
| `line` | text |  | no |  |  |

## Sublist `item` (Items)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `class` | select | Class | no |  | `purpose`: `line_class`; `allowed_values_listed_in`: `class_records`; `ui_effect`: `new_option_creates_record`; `help_text_shared_with`: `vendorbill.class` |
| `department` | select | Department | no |  | `purpose`: `line_department`; `allowed_values_listed_in`: `department_records`; `ui_effect`: `new_option_creates_record`; `help_text_shared_with`: `journalentry.department` |
| `location` | select | Location | yes |  | `purpose`: `line_location`; `allowed_values_listed_in`: `location_records`; `ui_effect`: `new_option_creates_record`; `help_text_shared_with`: `creditcardrefund.location` |
| `amount` | currency | Amount | no |  |  |
| `billvariancestatus` | text |  | no |  |  |
| `catchupperiod` | select |  | no |  |  |
| `customer` | select | Customer | no |  |  |
| `deferrevrec` | checkbox |  | no |  |  |
| `description` | textarea | Description | no |  |  |
| `id` | text |  | no |  |  |
| `inventorydetail` | summary | Inventory Detail | no |  |  |
| `isbillable` | checkbox | Billable | no |  |  |
| `isvsoebundle` | text |  | no |  |  |
| `item` | select | Item | yes |  |  |
| `itemsubtype` | text |  | no |  |  |
| `itemtype` | text |  | no |  |  |
| `line` | text |  | no |  |  |
| `linenumber` | integer |  | no |  |  |
| `matrixtype` | text |  | no |  |  |
| `options` | namevaluelist | Options | no |  |  |
| `quantity` | float | Quantity | no |  |  |
| `rate` | rate | Rate | no |  |  |
| `rateschedule` | text |  | no |  |  |
| `taxcode` | select | Tax Code | yes |  |  |
| `taxrate1` | percent | Tax Rate | no |  |  |
| `units` | select | Units | no |  |  |
| `vendorname` | text | Vendor Name | no |  |  |

## Sublist `accountingbookdetail` (Accounting Books)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `accountingbook` | select | Secondary Book | no |  |  |
| `exchangerate` | currency2 | Exchange Rate | yes |  |  |

## Tabs

| Tab ID | Label |
|---|---|
| `custom` | Custom |
| `history` | History |
| `items` | Items |
| `landedcost` | Landed Cost |
| `workflow` | Workflow |

## Transform targets

| Target record | Default fields |
|---|---|
| `journalentry` | `customform`, `void` |

## Suspected source errors

> [!SOURCE-ERROR] `expense_line_help_says_bank_account` (on `expense.account`)
<!-- source-error: record:check | expense.account | expense_line_help_says_bank_account -->
