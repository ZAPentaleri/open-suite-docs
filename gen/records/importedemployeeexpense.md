# Imported Employee Expense (`importedemployeeexpense`)
<!-- id: record:importedemployeeexpense -->

| Fact | Value |
|---|---|
| Internal ID | `importedemployeeexpense` |
| Category | transaction |
| Client scripts | yes |
| Server scripts | yes |
| Searchable | yes |
| Tags | `scripting_full` |

## Record facts

- `script_types`: `client`, `server`
- `script_functions`: `create`, `update`, `copy`, `delete`, `search`, `transform`
- `requires_feature`: `Expense Report` [`real_name`]
- `subrecords`: `importedemplexpensedetail`
- `configured_by`: `enable_features_employees_subtab` [`expense_reports`]

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `billedamount` | currency | Amount | yes |  |  | `purpose`: `billed_amount`; `value_source`: `card_company_import` |
| `billedcurrency` | select | Currency | yes |  |  | `purpose`: `billed_currency`; `value_source`: `card_company_import` |
| `billedtaxamount` | currency | Tax Amount | no |  |  | `purpose`: `billed_tax_amount`; `value_unit`: `base_currency`; `not_displayed_on`: `expense_reports` |
| `cardcurrency` | select | Card Currency | yes |  |  | `purpose`: `card_currency` |
| `cardmemberembossedname` | text | Card Member Embossed Name | no |  |  | `purpose`: `embossed_cardholder_name`; `value_source`: `employee_corporate_cards_cardholder_name` |
| `corporatecard` | checkbox | Corporate Card | no |  |  | `purpose`: `imported_from_corporate_card`; `default_value`: `true` (when `automatic_import`) |
| `costcenter` | text | Cost Center | no |  |  | `purpose`: `card_company_cost_center` |
| `currencyexchangerate` | currency2 | Exchange Rate | no |  |  | `purpose`: `exchange_rate_at_transaction` |
| `id` | integer | Internal ID | no |  |  | `purpose`: `expense_id` |
| `localchargeamount` | currency | Foreign Amount | no |  |  | `purpose`: `local_charge_amount`; `value_source`: `card_company_import` |
| `localtaxamount` | currency | Local Tax Amount | no |  |  | `purpose`: `local_tax_amount`; `not_displayed_on`: `expense_reports` |
| `memo` | text | Memo | no |  |  | `purpose`: `card_company_description`; `value_source`: `card_company_import` |
| `status` | select | Status | yes |  |  | `purpose`: `import_status`; `allowed_values`: `Failed`, `Ignored`, `Pending`, `Processed`, `Ready`; `option_meanings`: `pending:no_employee_assigned`, `ready:addable_to_reports`, `ignored:hidden_when_adding_to_reports`; `option_meanings`: `failed:not_imported_properly`, `processed:added_to_report`; `can`: `assign_employee_manually` (when `pending`) |
| `transactionid` | text | Transaction ID | yes |  |  | `purpose`: `card_company_transaction_id`; `value_source`: `card_company_import` |
| `category` | select | Expense Category | no |  |  |  |
| `chargedate` | date | Date | yes |  |  |  |
| `customform` | select | Custom Form | no |  |  |  |
| `employee` | select | Employee | no |  |  |  |
| `externalid` | text | External ID | no |  |  |  |
| `formatprofile` | select | Import Type | no |  |  |  |
| `internalid` | integer | Internal ID | no |  |  |  |
| `sourcetype` | select | Source Type | yes |  |  |  |

## Sublist `impemplexpensedetail` (Imported Employee Expense Detail)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `fieldname` | text | Field Name | no |  |  |
| `fieldvalue` | text | Value | no |  |  |
| `id` | integer | Internal ID | no |  |  |
| `importedemployeeexpense` | select | Imported Employee Expense | no |  |  |
