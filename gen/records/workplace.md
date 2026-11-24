# Workplace (`workplace`)
<!-- id: record:workplace -->

| Fact | Value |
|---|---|
| Internal ID | `workplace` |
| Category | list |
| Client scripts | yes |
| Server scripts | yes |
| Searchable | yes |
| Tags | `custom_fields_supported`, `scripting_full` |

## Record facts

- `script_types`: `client`, `server`
- `script_functions`: `create`, `update`, `copy`, `delete`, `search`
- `requires_feature`: `Payroll` [`real_name`]
- `required_for`: `paid_employees_per_tax_location`
- `purpose`: `defines_employer_withholding_taxes_per_tax_location`
- `requires`: `every_payroll_employee_has_workplace`
- `configured_by`: `enable_features_employees_subtab` [`payroll`]
- `ui_location`: `lists_employees_workplaces`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `isinactive` | checkbox | Workplace is Inactive | no | no |  | `purpose`: `inactivate_workplace`; `ui_effect`: `hidden_from_lists_fields_and_reports` |
| `name` | text | Name | yes | no |  | `purpose`: `workplace_name`; `displayed_on`: `workplace_lists`, `reports` |
| `parent` | select | Subworkplace of | no | no |  | `purpose`: `parent_workplace` |
| `subsidiary` | select | Subsidiary | yes | no |  | `purpose`: `payroll_issuing_subsidiary`; `requires`: `valid_us_address_and_unique_ein` |
| `externalid` | text | ExternalId | no | no |  |  |
| `jurisdictioncounty` | select | County: | no | no |  |  |
| `jurisdictionfederal` | select | Federal: | no | no |  |  |
| `jurisdictionlocal` | select | Local: | no | no |  |  |
| `jurisdictionschool` | select | School District: | no | no |  |  |
| `jurisdictionstate` | select | State: | no | no |  |  |
| `mainaddress` | summary |  | no | no |  |  |

## Sublist `jurisdictionhist` (History)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `effectivedate` | date | Effective Date | no |  |  |
| `expirationdate` | date | Expiration Date | no |  |  |
| `name` | text | Jurisdiction | no |  |  |
| `type` | text | Level | no |  |  |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `address` | textarea | Address |  |
| `city` | text | City |  |
| `country` | select | Country |  |
| `county` | text | County/State |  |
| `internalid` | select | Internal ID |  |
| `internalidnumber` | integer | Internal ID (Number) |  |
| `isinactive` | checkbox | Inactive |  |
| `name` | text | Name |  |
| `phone` | phone | Phone |  |
| `state` | select | State/Province |  |
| `zip` | text | Zip |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `city` | text | City |  |
| `country` | text | Country |  |
| `internalid` | select | Internal ID |  |
| `isinactive` | checkbox | Inactive |  |
| `name` | text | Name |  |
| `namenohierarchy` | text | Name (no hierarchy) |  |
| `phone` | phone | Phone |  |
| `state` | text | State/Province |  |
| `zip` | text | Zip |  |

## Search joins

| Join ID | Label | Joined record |
|---|---|---|
| `address` | Address | Address |
| `time` | Time | Time |
| `timeentry` | Time Entry | Time Entry |
| `user` | User | Employee |
