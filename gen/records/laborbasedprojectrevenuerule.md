# Labor Based Project Revenue Rule (`laborbasedprojectrevenuerule`)
<!-- id: record:laborbasedprojectrevenuerule -->

| Fact | Value |
|---|---|
| Internal ID | `laborbasedprojectrevenuerule` |
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
- `not_exposed`: `filters_sublist`
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
| `ratemultiplier` | posfloat | Rate Multiplier | no | no |  | `purpose`: `rate_multiplier`; `effect`: `recognized_amount_is_rate_times_multiplier` |
| `rateroundingtype` | select | Time Rounding | no | no |  | `purpose`: `time_rounding_method` |
| `ratesourcetype` | select | Rate Basis | yes | no |  | `purpose`: `revenue_rate_source`; `options`: `Billing Classes`, `Resources`, `Service Items`; `requires_feature`: `Per-Employee Billing Rates` (when `billing_classes`) [`real_name`]; `caveat`: `task_rates_not_used` |
| `serviceitem` | select | Service Item | yes | no |  | `purpose`: `rule_service_item`; `effect`: `determines_revenue_element_accounts`; `caveat`: `one_labor_rule_per_service_item` |
| `customform` | select | Custom Form | yes | no |  |  |
| `externalid` | text | ExternalId | no | no |  |  |
| `revenuereconciled` | checkbox | Revenue Reconciled | no | no |  |  |
| `savedsearch` | integer |  | no | no |  |  |

## Sublist `chargerule` (Charge Rules)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `actualamount` | currency | Amount | no |  |  |
| `billingitem` | select | Billing Item | no |  |  |
| `chargerule` | select | Name | yes |  |  |
| `description` | text | Description | no |  |  |
| `forecastamount` | currency | Amount | no |  |  |
| `ruletype` | select | Type | no |  |  |

## Sublist `resourcerateoverride` (Resources)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `rate` | poscurrency | Rate | yes |  |  |
| `rsrc` | select | Name | yes |  |  |

## Tabs

| Tab ID | Label |
|---|---|
| `chargerules` | Charge Rules |
| `filterstab` | Filters |
| `rates` | Rates |
| `systeminfo` | System Information |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `billingratecard` | select | Billing Rate Card |  |
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
| `ratemultiplier` | posfloat | Rule Multiplier |  |
| `rateroundingtype` | select | Time Rounding |  |
| `ratesourcetype` | select | Rate Source |  |
| `revenuereconciled` | checkbox | Revenue Reconciled |  |
| `ruletype` | select | Rule Type |  |
| `serviceitem` | select | Service Item |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `billingratecard` | text | Billing Rate Card |  |
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
| `ratemultiplier` | posfloat | Rule Multiplier |  |
| `rateroundingtype` | text | Time Rounding |  |
| `ratesourcetype` | text | Rate Source |  |
| `revenuereconciled` | checkbox | Revenue Reconciled |  |
| `ruletype` | text | Rule Type |  |
| `serviceitem` | select | Service Item |  |
| `totalamounttorecognize` | currency | Total Amount to Recognize |  |

## Search joins

| Join ID | Label | Joined record |
|---|---|---|
| `billingratecard` | Billing Rate Card | Billing Rate Card |
| `project` | Project | Job |
| `serviceitem` | Service Item | Item |
