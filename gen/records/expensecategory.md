# Expense Category (`expensecategory`)
<!-- id: record:expensecategory -->

| Fact | Value |
|---|---|
| Internal ID | `expensecategory` |
| Category | list |
| Client scripts | no |
| Server scripts | yes |
| Searchable | yes |
| Tags | `custom_fields_supported`, `scripting_full` |

## Record facts

- `script_types`: `server`
- `script_functions`: `create`, `update`, `copy`, `delete`, `search`
- `rule`: `available_to_subsidiaries_of_linked_account`
- `purpose`: `group_expenses_and_post_to_linked_account`
- `value_examples`: `transportation`, `lodging`, `mileage`, `entertainment`
- `cannot`: `create_category_while_entering_expense_report`
- `requires`: `category_available_to_employee_and_customer_subsidiaries` (when `intercompany_expense_lines`)
- `ui_location`: `setup_accounting_expense_categories_new`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `defaultrate` | posfloat | Default Rate | no | no |  | `purpose`: `default_rate`; `can`: `override_per_line` |
| `description` | textarea | Description | no | no |  | `purpose`: `category_description`; `caveat`: `optional_field`; `purpose`: `tracking_url_description` (when `google_checkout_integration`) |
| `expenseacct` | select | Expense Account | yes | no |  | `purpose`: `expense_account` |
| `isinactive` | checkbox | Inactive | no | no |  | `purpose`: `inactivate_category` |
| `name` | text | Name | yes | no |  | `purpose`: `category_name`; `doc_inconsistency`: `help_describes_tracking_url_vendor` |
| `personalcorporatecardexpense` | checkbox | Personal Corporate Card Expense | no | no |  | `purpose`: `personal_corporate_card_category`; `constraint`: `other_asset_accounts_only`; `read_only_when`: `expanded_account_lists_off`; `value_source`: `set_for_other_asset_account_categories` |
| `raterequired` | checkbox | Rate is Required | no | no |  | `purpose`: `rate_required`; `effect`: `amount_from_quantity_times_rate` |
| `subsidiary` | select | Subsidiaries | no | no |  | `purpose`: `category_subsidiaries`; `allowed_values`: `subsidiaries_of_expense_account` |
| `externalid` | text | ExternalId | no | no |  |  |

## Tabs

| Tab ID | Label |
|---|---|
| `cattrans` | Translation |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `account` | select | Account |  |
| `description` | text | Description |  |
| `externalid` | select | External ID |  |
| `externalidstring` | text | External ID (Text) |  |
| `formuladate` | date | Formula (Date) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulatext` | text | Formula (Text) |  |
| `internalid` | select | Internal ID |  |
| `internalidnumber` | integer | Internal ID (Number) |  |
| `isinactive` | checkbox | Inactive |  |
| `name` | text | Name |  |
| `personalcorporatecardexpense` | checkbox | Personal Corporate Card Expense |  |
| `subsidiary` | select | Subsidiary |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `account` | select | Account |  |
| `description` | text | Description |  |
| `externalid` | select | External ID |  |
| `formulacurrency` | currency | Formula (Currency) |  |
| `formuladate` | date | Formula (Date) |  |
| `formuladatetime` | datetime | Formula (Date/Time) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulapercent` | percent | Formula (Percent) |  |
| `formulatext` | text | Formula (Text) |  |
| `internalid` | select | Internal ID |  |
| `isinactive` | checkbox | Inactive |  |
| `name` | text | Name |  |
| `personalcorporatecardexpense` | checkbox | Personal Corporate Card Expense |  |
| `subsidiary` | select | Subsidiary |  |

## Suspected source errors

> [!SOURCE-ERROR] `help_describes_tracking_url_vendor` (on `name`)
<!-- source-error: record:expensecategory | name | help_describes_tracking_url_vendor -->
