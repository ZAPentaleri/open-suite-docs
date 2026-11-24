# Paycheck Journal (`paycheckjournal`)
<!-- id: record:paycheckjournal -->

| Fact | Value |
|---|---|
| Internal ID | `paycheckjournal` |
| Category | transaction |
| Client scripts | yes |
| Server scripts | yes |
| Searchable | yes |
| Search data via | Transaction |
| Tags | `custom_fields_supported`, `scripting_full` |

## Record facts

- `requires_feature`: `Paycheck Journal` [`real_name`]
- `purpose`: `global_payroll_integrations`
- `related` (refs `record:payrollitem`)
- `value_examples`: `create_and_update_paycheck_journal`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `account` | select | Account | yes | no |  | `purpose`: `payment_account`; `doc_inconsistency`: `help_describes_receivables_bill_payment` |
| `class` | select | Class | no | no |  | `purpose`: `class`; `doc_inconsistency`: `help_describes_employee_class`; `allowed_values_listed_in`: `class_records`; `ui_effect`: `new_option_creates_record` |
| `currency` | select | Currency | yes | no |  | `purpose`: `currency_of_amounts` |
| `customform` | select | Custom Form | no | no |  | `purpose`: `custom_form`; `doc_inconsistency`: `help_describes_employee_form` |
| `department` | select | Department | no | no |  | `purpose`: `department`; `allowed_values_listed_in`: `department_records`; `ui_effect`: `new_option_creates_record` |
| `employee` | select | Employee | yes | no |  | `purpose`: `associated_employee` |
| `exchangerate` | float | Exchange Rate | no | no |  | `purpose`: `exchange_rate`; `default_value`: `1`; `recommendation`: `do_not_change`; `caveat`: `reserved_for_future_multi_currency` |
| `location` | select | Location | no | no |  | `purpose`: `location` |
| `postingperiod` | select | Posting Period | no | no |  | `purpose`: `posting_period`; `cannot`: `post_to_closed_period` |
| `subsidiary` | select | Subsidiary | yes | no |  | `purpose`: `subsidiary`; `requires_feature`: `OneWorld` [`real_name`]; `default_behavior`: `current_user_subsidiary`; `allowed_values`: `vendor_assigned_subsidiaries` (when `shared_vendor`); `ui_effect`: `item_and_classification_fields_resourced` |
| `trandate` | date | Date | yes | no |  | `purpose`: `transaction_date`; `default_behavior`: `current_date` |
| `createddate` | datetime |  | no | no |  |  |
| `externalid` | text | ExternalId | no | no |  |  |
| `lastmodifieddate` | datetime |  | no | no |  |  |

## Sublist `companycontribution` (Company Contributions)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `amount` | currency2 | Amount | yes |  |  |
| `class` | select | Class | no |  |  |
| `department` | select | Department | no |  |  |
| `id` | integer | Line Id | no |  |  |
| `location` | select | Location | no |  |  |
| `payrollitem` | select | Payroll Item | yes |  |  |

## Sublist `companytax` (Company Taxes)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `amount` | currency2 | Amount | yes |  |  |
| `class` | select | Class | no |  |  |
| `department` | select | Department | no |  |  |
| `id` | integer | Line Id | no |  |  |
| `location` | select | Location | no |  |  |
| `payrollitem` | select | Payroll Item | yes |  |  |

## Sublist `deduction` (Deductions)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `amount` | currency2 | Amount | yes |  |  |
| `class` | select | Class | no |  |  |
| `department` | select | Department | no |  |  |
| `id` | integer | Line Id | no |  |  |
| `location` | select | Location | no |  |  |
| `payrollitem` | select | Payroll Item | yes |  |  |

## Sublist `earning` (Earnings)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `amount` | currency2 | Amount | yes |  |  |
| `class` | select | Class | no |  |  |
| `department` | select | Department | no |  |  |
| `hours` | float | Hours | no |  |  |
| `id` | integer | Line Id | no |  |  |
| `location` | select | Location | no |  |  |
| `payrollitem` | select | Payroll Item | yes |  |  |

## Sublist `employeetax` (Employee Taxes)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `amount` | currency2 | Amount | yes |  |  |
| `class` | select | Class | no |  |  |
| `department` | select | Department | no |  |  |
| `id` | integer | Line Id | no |  |  |
| `location` | select | Location | no |  |  |
| `payrollitem` | select | Payroll Item | yes |  |  |

## Tabs

| Tab ID | Label |
|---|---|
| `custom` | Custom |
| `paychecklines` | Paycheck Lines |

## Suspected source errors

> [!SOURCE-ERROR] `help_describes_receivables_bill_payment` (on `account`)
<!-- source-error: record:paycheckjournal | account | help_describes_receivables_bill_payment -->

> [!SOURCE-ERROR] `help_describes_employee_class` (on `class`)
<!-- source-error: record:paycheckjournal | class | help_describes_employee_class -->

> [!SOURCE-ERROR] `help_describes_employee_form` (on `customform`)
<!-- source-error: record:paycheckjournal | customform | help_describes_employee_form -->
