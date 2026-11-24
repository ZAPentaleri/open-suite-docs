# Department (`department`)
<!-- id: record:department -->

| Fact | Value |
|---|---|
| Internal ID | `department` |
| Category | list |
| Client scripts | no |
| Server scripts | yes |
| Tags | `custom_fields_supported` |

## Record facts

- `script_types`: `server`
- `script_functions`: `create`, `update`, `copy`, `delete`, `search`
- `client_supported`: `false`
- `script_types`: `server`
- `purpose`: `categories_to_track_financials_transactions_employees`
- `value_examples`: `department_per_employee_team`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `includechildren` | checkbox | Include Children | no | no |  | `purpose`: `share_department_with_child_subsidiaries` |
| `isinactive` | checkbox | Department is Inactive | no | no |  | `purpose`: `inactivate_department`; `ui_effect`: `hidden_from_department_lists` |
| `name` | text | Name | yes | no |  | `purpose`: `department_name`; `format`: `alphanumeric`; `max_length`: `60` characters; `displayed_on`: `lists` |
| `parent` | select | Subdepartment of | no | no |  | `purpose`: `parent_department` |
| `subsidiary` | select | Subsidiaries | no | no |  | `purpose`: `department_subsidiaries`; `requires_feature`: `OneWorld` [`real_name`] |
| `externalid` | text | ExternalId | no | no |  |  |

## Tabs

| Tab ID | Label |
|---|---|
| `history` | System Notes |
| `translation` | Translation |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `externalid` | select | External ID |  |
| `externalidstring` | text | External ID (Text) |  |
| `formuladate` | date | Formula (Date) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulatext` | text | Formula (Text) |  |
| `internalid` | select | Internal ID |  |
| `internalidnumber` | integer | Internal ID (Number) |  |
| `isinactive` | checkbox | Inactive |  |
| `name` | text | Name |  |
| `subsidiary` | multiselect | Subsidiary |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
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
| `namenohierarchy` | text | Name (no hierarchy) |  |
| `subsidiary` | multiselect | Subsidiary |  |
