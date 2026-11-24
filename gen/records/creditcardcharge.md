# Credit Card Charge (`creditcardcharge`)
<!-- id: record:creditcardcharge -->

| Fact | Value |
|---|---|
| Internal ID | `creditcardcharge` |
| Category | transaction |
| Client scripts | no |
| Server scripts | yes |
| Searchable | yes |
| Search data via | Transaction |
| Tags | `custom_fields_supported`, `scripting_full` |

## Record facts

- `script_types`: `server`
- `script_functions`: `create`, `update`, `copy`, `delete`, `search`, `transform`
- `requires`: `credit_card_account`
- `requires_permission`: `credit_card_full`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `account` | select | Account | no | no |  | `purpose`: `credit_card_account`; `editable_only_when`: `edit_mode`; `allowed_values_listed_in`: `credit_card_accounts`; `ui_effect`: `new_option_creates_record` |
| `balance` | currency | Balance | no | no |  | `purpose`: `account_balance`; `value_source`: `system_displayed` |
| `class` | select | Class | no | no |  | `purpose`: `class`; `editable_only_when`: `edit_mode`; `allowed_values_listed_in`: `class_records`; `ui_effect`: `new_option_creates_record` |
| `currency` | select | Currency | no | no |  | `purpose`: `currency`; `default_behavior`: `vendor_currency` |
| `customform` | select | Custom Form | yes | no |  | `purpose`: `entry_form`; `allowed_values_listed_in`: `standard_and_custom_entry_forms` |
| `department` | select | Department | no | no |  | `purpose`: `department`; `editable_only_when`: `edit_mode`; `allowed_values_listed_in`: `department_records`; `ui_effect`: `new_option_creates_record` |
| `entity` | select | Vendor | yes | no |  | `purpose`: `vendor`; `editable_only_when`: `edit_mode`; `allowed_values_listed_in`: `vendor_records`; `ui_effect`: `new_option_creates_record` |
| `exchangerate` | currency2 | Exchange Rate | yes | no |  | `purpose`: `exchange_rate`; `can`: `update_currency_record_rate` |
| `location` | select | Location | yes | no |  | `purpose`: `location`; `editable_only_when`: `edit_mode`; `allowed_values_listed_in`: `location_records`; `ui_effect`: `new_option_creates_record` |
| `memo` | text | Memo | no | yes |  | `purpose`: `memo`; `displayed_on`: `reports` |
| `postingperiod` | select | Posting Period | yes | no |  | `purpose`: `posting_period`; `applies_when`: `accounting_periods_used`; `cannot`: `post_to_closed_period` |
| `subsidiary` | select | Subsidiary | no | no |  | `purpose`: `subsidiary`; `default_behavior`: `vendor_primary_subsidiary`; `allowed_values`: `vendor_secondary_subsidiaries` (when `oneworld_shared_vendor`); `ui_effect`: `change_clears_line_items` |
| `trandate` | date | Date | yes | no |  | `purpose`: `posting_date`; `default_behavior`: `current_date`; `displayed_on`: `registers` |
| `tranid` | text | Reference # | no | no |  | `purpose`: `card_transaction_number`; `max_length`: `45` characters |
| `transactionnumber` | text | Transaction Number | no | no |  | `purpose`: `transaction_number`; `default_value`: `To Be Generated` (when `auto_transaction_numbering`); `value_source`: `gapless_number_generated_on_save`; `cannot`: `overwrite` (when `auto_transaction_numbering`) |
| `trantype` | radio | Charge | no | no |  | `purpose`: `charge_or_credit`; `allowed_values`: `Charge`, `Credit`; `cannot`: `change_after_save` |
| `usertotal` | currency | Amount | no | no |  | `purpose`: `transaction_amount`; `effect`: `updated_from_lines`; `caveat`: `credits_shown_positive` |
| `cleared` | checkbox | Cleared | no |  |  |  |
| `cleareddate` | date | Date Cleared | no |  |  |  |
| `createddate` | datetime |  | no | no |  |  |
| `currencyname` | text | Currency | no | no |  |  |
| `currencysymbol` | text | Currency Symbol | no | no |  |  |
| `entity_nexus_country` | text | Country | no | no |  |  |
| `entitynexus` | select | Nexus | no | no |  |  |
| `externalid` | text | ExternalId | no | no |  |  |
| `isbasecurrency` | checkbox | Base Currency | no | no |  |  |
| `lastmodifieddate` | datetime |  | no | no |  |  |
| `nexus` | select | Nexus | no | no |  |  |
| `nexus_country` | text | Country | no | no |  |  |
| `taxperiod` | text |  | no | no |  |  |
| `taxpointdate` | date | Tax Point Date | no | no |  |  |
| `taxpointdateoverride` | checkbox | Override | no | no |  |  |
| `total` | currency | Amount | no | no |  |  |
| `updatecurrency` | text | Update Currency | no | no |  |  |

## Sublist `expense` (Expenses)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `account` | select | Account | yes |  |  |
| `amount` | currency | Amount | yes |  |  |
| `category` | text |  | no |  |  |
| `class` | select | Class | no |  |  |
| `customer` | select | Customer | no |  |  |
| `department` | select | Department | no |  |  |
| `expenseitem` | integer | Expense Item | no |  |  |
| `isbillable` | checkbox | Billable | no |  |  |
| `line` | integer | Line | no |  |  |
| `location` | select | Location | no |  |  |
| `memo` | text | Memo | no |  |  |
| `numrules` | integer | Expense-Based Rules | no |  |  |

## Sublist `item` (Items)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `amount` | currency | Amount | no |  |  |
| `billvariancestatus` | text |  | no |  |  |
| `catchupperiod` | select |  | no |  |  |
| `class` | select | Class | no |  |  |
| `customer` | select | Customer | no |  |  |
| `deferrevrec` | checkbox |  | no |  |  |
| `department` | select | Department | no |  |  |
| `description` | textarea | Description | no |  |  |
| `id` | text |  | no |  |  |
| `isbillable` | checkbox | Billable | no |  |  |
| `isvsoebundle` | text |  | no |  |  |
| `item` | select | Item | yes |  |  |
| `itemsubtype` | text | Item subtype | no |  |  |
| `itemtype` | text | Item Type | no |  |  |
| `line` | text |  | no |  |  |
| `linenumber` | integer | Line Number | no |  |  |
| `linked` | text | Linked | no |  |  |
| `location` | select | Location | yes |  |  |
| `matrixtype` | text | Matrix Type | no |  |  |
| `options` | namevaluelist | Options | no |  |  |
| `printitems` | text | Print Items | no |  |  |
| `quantity` | float | Quantity | no |  |  |
| `rate` | rate | Rate | no |  |  |
| `rateschedule` | text |  | no |  |  |
| `vendorname` | text | Vendor Name | no |  |  |

## Sublist `reimbursements` (Reimbursements)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `id` | text |  | no |  |  |
