# Percent Complete Project Revenue Rule (`pctcompleteprojectrevenuerule`)
<!-- id: record:pctcompleteprojectrevenuerule -->

| Fact | Value |
|---|---|
| Internal ID | `pctcompleteprojectrevenuerule` |
| Category | list |
| Client scripts | yes |
| Server scripts | yes |
| Tags | `custom_fields_supported`, `no_copy`, `no_transform` |

## Record facts

- `requires_feature`: `Project Management`, `Charge-Based Billing`, `Advanced Revenue Management (Essentials)` [`real_name`]
- `script_types`: `client`, `server`
- `script_functions`: `create`, `read`, `update`, `delete`, `search`
- `not_supported`: `copy`, `transform`
- `standalone`: `false`
- `required_fields`: `company:project_id`
- `purpose`: `project_revenue_from_charge_rules`
- `displayed_on`: `charge_based_project_financial_subtab`
- `option_meanings`: `labor_based:time_tracked`, `fixed_amount:date_task_or_milestone`, `percent_complete:project_progress` [`rule_types`]
- `not_supported`: `expense_based_charge_rules`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `description` | textarea | Description | no | no |  | `purpose`: `rule_description` |
| `isinactive` | checkbox | Inactive | no | no |  | `purpose`: `inactivate_rule` |
| `name` | text | Name | yes | no |  | `purpose`: `rule_name` |
| `project` | select | Project | no | no |  | `purpose`: `rule_project`; `value_source`: `system_displayed` |
| `serviceitem` | select | Service Item | yes | no |  | `purpose`: `rule_service_item`; `effect`: `determines_revenue_element_accounts`; `caveat`: `one_labor_rule_per_service_item` |
| `customform` | select | Custom Form | yes | no |  |  |
| `externalid` | text | ExternalId | no | no |  |  |
| `revenuereconciled` | checkbox | Revenue Reconciled | no | no |  |  |

## Sublist `chargerule` (Charge Rules)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `actualamount` | currency | Amount | no |  |  |
| `billingitem` | select | Billing Item | no |  |  |
| `chargerule` | select | Name | yes |  |  |
| `description` | text | Description | no |  |  |
| `forecastamount` | currency | Amount | no |  |  |
| `ruletype` | select | Type | no |  |  |

## Tabs

| Tab ID | Label |
|---|---|
| `chargerules` | Charge Rules |
| `systeminfo` | System Information |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `chargerule` | select | Charge Rule |  |
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
| `project` | select | Project |  |
| `revenuereconciled` | checkbox | Revenue Reconciled |  |
| `ruletype` | select | Rule Type |  |
| `serviceitem` | select | Service Item |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `chargerule` | select | Charge Rule |  |
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
| `project` | text | Project |  |
| `revenuereconciled` | checkbox | Revenue Reconciled |  |
| `ruletype` | text | Rule Type |  |
| `serviceitem` | select | Service Item |  |
| `totalamounttorecognize` | currency | Total Amount to Recognize |  |

## Search joins

| Join ID | Label | Joined record |
|---|---|---|
| `project` | Project | Job |
| `serviceitem` | Service Item | Item |
