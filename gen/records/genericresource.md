# Generic Resource (`genericresource`)
<!-- id: record:genericresource -->

| Fact | Value |
|---|---|
| Internal ID | `genericresource` |
| Category | entity |
| Client scripts | yes |
| Server scripts | yes |
| Tags | `custom_fields_supported`, `no_copy` |

## Record facts

- `script_functions`: `read`, `create`, `edit`, `delete`, `search`
- `script_types`: `client`, `server`
- `not_supported`: `copy`
- `purpose`: `placeholder_resource_for_planning`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `billingclass` | select | Billing Class | no | no |  | `purpose`: `billing_class`; `effect`: `sets_default_project_budget_pricing` |
| `entityid` | text | Name | yes | no |  | `purpose`: `generic_resource_name`; `value_examples`: `Project Manager` |
| `isinactive` | checkbox | Inactive | no | no |  | `purpose`: `inactivate_generic_resource`; `ui_effect`: `hidden_from_all_lists_and_choices` |
| `laborcost` | poscurrency | Labor Cost | no | no |  | `purpose`: `estimated_hourly_labor_cost`; `effect`: `used_for_estimated_task_cost` |
| `laborprice` | poscurrency | Price | no | no |  | `purpose`: `labor_price` |
| `subsidiary` | select | Subsidiary | yes | no |  | `purpose`: `subsidiary` |
| `workcalendar` | select | Work Calendar | yes | no |  | `purpose`: `work_calendar` |
| `customform` | select | Custom Form | yes | no |  |  |
| `externalid` | text | ExternalId | no | no |  |  |

## Tabs

| Tab ID | Label |
|---|---|
| `custom` | Custom |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `billingclass` | select | Billing Class |  |
| `entityid` | text | Name |  |
| `externalid` | select | External ID |  |
| `externalidstring` | text | External ID (Text) |  |
| `formuladate` | date | Formula (Date) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulatext` | text | Formula (Text) |  |
| `internalid` | select | Internal ID |  |
| `internalidnumber` | integer | Internal ID (Number) |  |
| `isinactive` | checkbox | Inactive |  |
| `laborcost` | currency | Cost |  |
| `laborprice` | currency | Price |  |
| `subsidiary` | select | Subsidiary |  |
| `workcalendar` | select | Work Calendar |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `billingclass` | select | Billing Class |  |
| `entityid` | text | Name |  |
| `externalid` | select | External ID |  |
| `formulacurrency` | currency | Formula (Currency) |  |
| `formuladate` | date | Formula (Date) |  |
| `formuladatetime` | datetime | Formula (Date/Time) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulapercent` | percent | Formula (Percent) |  |
| `formulatext` | text | Formula (Text) |  |
| `id` | integer | ID |  |
| `internalid` | select | Internal ID |  |
| `isinactive` | checkbox | Inactive |  |
| `laborcost` | currency | Cost |  |
| `laborprice` | currency | Price |  |
| `subsidiary` | select | Subsidiary |  |
| `workcalendar` | select | Work Calendar |  |
