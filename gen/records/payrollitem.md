# Payroll Item (`payrollitem`)
<!-- id: record:payrollitem -->

| Fact | Value |
|---|---|
| Internal ID | `payrollitem` |
| Category | list |
| Client scripts | yes |
| Server scripts | yes |
| Searchable | yes |
| Tags | `custom_fields_supported`, `scripting_full` |

## Record facts

- `script_types`: `client`, `server`
- `purpose`: `global_payroll_solutions`
- `related` (refs `record:paycheckjournal`)
- `purpose`: `values_for_payroll_transaction_lines`
- `caveat`: `payroll_transactions_managed_by_netsuite_not_integrations`
- `script_functions`: `create`, `update`, `copy`, `delete`, `search`
- `value_examples`: `deduction_items`, `earning_addition_items`
- `use_cases`: `external_payroll_system_integrations`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `accountcategory` | select | Account Category | no | no |  | `purpose`: `payroll_account_category`; `value_examples`: `Liability` |
| `allowbulkentry` | checkbox | Allow entry from Create Payroll | no | no |  | `purpose`: `allow_entry_from_create_payroll`; `applies_to`: `Earning:Wage`, `Earning:Addition` |
| `applylimit` | checkbox | Apply Limit to All Employees | no | no |  | `purpose`: `apply_limit_to_all_employees`; `false_means`: `limit_only_for_selected_employees` |
| `applyrate` | checkbox | Apply Rate to All Employees | no | no |  | `purpose`: `apply_rate_to_all_employees`; `false_means`: `rate_only_for_selected_employees` |
| `basedonquantity` | checkbox | Based on quantity | no | no |  | `purpose`: `calculated_from_quantity`; `value_examples`: `hours`, `miles` |
| `defaultrate` | rate | Default Rate | no | no |  | `purpose`: `default_rate`; `accepts_forms`: `amount`, `percentage`; `value_examples`: `2.00`, `2%`; `effect`: `inserted_for_new_employees`; `can`: `override_per_employee` |
| `employeepaid` | checkbox | Employee Paid | no | no |  | `purpose`: `employee_paid_tax`; `true_means`: `employee_tax`; `false_means`: `company_tax` |
| `expenseaccount` | select | Expense Account | no | no |  | `purpose`: `expense_account` |
| `inactive` | checkbox | Inactive | no | no |  | `purpose`: `inactivate_payroll_item`; `ui_effect`: `hidden_from_list_unless_show_inactives` |
| `itemtype` | select | Item Type | yes | no |  | `purpose`: `payroll_item_type` |
| `liabilityaccount` | select | Liability Account | no | no |  | `purpose`: `liability_account` |
| `limit` | currency | Limit | no | no |  | `purpose`: `annual_limit`; `value_unit`: `dollars`; `effect`: `item_stops_applying_at_limit_even_mid_run`; `default_behavior`: `no_limit` (when `blank`) |
| `limitiswithholding` | radio | Wage | no | no |  | `purpose`: `limit_basis`; `options`: `Wage`, `Withholding` |
| `limittype` | text | Limit Type | no | no |  | `purpose`: `limit_type`; `options`: `wages`, `withholding` |
| `masterpaycode` | select | Pay Code | no | no |  | `purpose`: `master_pay_code` |
| `name` | text | Item Name | yes | no |  | `purpose`: `payroll_item_name` |
| `reportingsection` | select | Report Section | no | no |  | `purpose`: `payroll_report_section` |
| `showinemployeecenter` | checkbox | View in Employee Center | no | no |  | `purpose`: `show_in_employee_center` |
| `subsidiary` | select | Subsidiary | yes | no |  | `purpose`: `payroll_issuing_subsidiary`; `requires`: `valid_us_address_and_unique_ein` |
| `usedefaults` | checkbox | Use System Defaults | no | no |  | `purpose`: `uses_item_type_defaults`; `value_source`: `system_displayed` |
| `vendor` | select | Agency | no | no |  | `purpose`: `deduction_agency` |
| `externalid` | text | ExternalId | no | no |  |  |

## Sublist `employeepaycodelist` (Employees)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `apply` | checkbox | Apply | no |  |  |
| `employeeid` | integer | Name | no |  |  |
| `empprevstate` | text | State | no |  |  |
| `inactive` | checkbox | Inactive | no |  |  |

## Tabs

| Tab ID | Label |
|---|---|
| `employee` | Employee |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `expenseaccount` | select | Expense Account |  |
| `itemtypenohierarchy` | select | Item Type (no hierarchy) |  |
| `liabilityaccount` | select | Liability Account |  |
| `masterpaycode` | select | Master Pay Code |  |
| `name` | text | Name |  |
| `subsidiary` | select | Subsidiary |  |
| `vendor` | select | Vendor |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `expenseaccount` | text | Expense Account |  |
| `isinactive` | checkbox | Inactive |  |
| `itemtypenohierarchy` | text | Item Type (no hierarchy) |  |
| `liabilityaccount` | text | Liability Account |  |
| `masterpaycode` | text | Master Pay Code |  |
| `name` | text | Name |  |
| `vendor` | text | Vendor |  |
