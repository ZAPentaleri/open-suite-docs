# Imported_employee_expenses (`imported_employee_expenses`)
<!-- id: connect:imported_employee_expenses -->

| Fact | Value |
|---|---|
| Table | `imported_employee_expenses` |
| Record | `importedemployeeexpense` |
| Primary key | `imported_employee_expense_id` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `billed_amount` | NUMBER |  | 30 | 15 |  |  | `purpose`: `billed_amount` |
| `billed_currency_id` | NUMBER |  | 39 | 0 | `currencies`.`currency_id` | foreign_key | `purpose`: `billed_currency_id` |
| `billed_tax_amount` | NUMBER |  | 30 | 15 |  |  | `purpose`: `billed_tax_amount` |
| `card_currency_id` | NUMBER |  | 39 | 0 | `currencies`.`currency_id` | foreign_key | `purpose`: `card_currency_id` |
| `cost_center` | VARCHAR2 | 13 |  |  |  |  | `purpose`: `cost_center` |
| `currency_exchange_rate` | NUMBER |  | 30 | 15 |  |  | `purpose`: `currency_exchange_rate` |
| `date_charge` | TIMESTAMP |  |  |  |  |  | `purpose`: `date_charge` |
| `employee_id` | NUMBER |  | 39 | 0 | `employees`.`employee_id` | foreign_key | `purpose`: `employee_id` |
| `expense_category_id` | NUMBER |  | 39 | 0 | `expense_categories`.`expense_category_id` | foreign_key | `purpose`: `expense_category_id` |
| `format_profile_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `format_profile_id` |
| `imported_empl_expense_extid` | VARCHAR2 | 255 |  |  |  |  | `purpose`: `imported_expense_external_id` |
| `imported_employee_expense_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `imported_employee_expense_id` |
| `is_corporate_card` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `corporate_card_expense` |
| `local_charge_amount` | NUMBER |  | 30 | 15 |  |  | `purpose`: `local_charge_amount` |
| `local_tax_amount` | NUMBER |  | 30 | 15 |  |  | `purpose`: `local_tax_amount` |
| `memo` | VARCHAR2 | 45 |  |  |  |  | `purpose`: `memo` |
| `source_id` | NUMBER |  | 39 | 0 | `employee_expense_sources`.`source_type_id` | foreign_key | `purpose`: `imported_expense_source_type`; `same_as`: `employee_expense_sources.source_type_id` |
| `source_type_id` | VARCHAR2 | 16 |  |  |  |  | `purpose`: `source_type_id` |
| `status_id` | VARCHAR2 | 20 |  |  |  |  | `purpose`: `status_id` |
| `transaction_id` | VARCHAR2 | 50 |  |  |  |  | `purpose`: `transaction_id` |

## Foreign keys

| Key | Columns | References table | Referenced columns |
|---|---|---|---|
| `imported_employee_expenses_currencies_fk` | `billed_currency_id` | `currencies` | `currency_id` |
| `imported_employee_expenses_currencies_fk_2` | `card_currency_id` | `currencies` | `currency_id` |
| `imported_employee_expenses_employee_expense_sources_fk` | `source_id` | `employee_expense_sources` | `source_type_id` |
| `imported_employee_expenses_employees_fk` | `employee_id` | `employees` | `employee_id` |
| `imported_employee_expenses_expense_categories_fk` | `expense_category_id` | `expense_categories` | `expense_category_id` |
