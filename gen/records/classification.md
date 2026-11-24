# Class (`classification`)
<!-- id: record:classification -->

| Fact | Value |
|---|---|
| Internal ID | `classification` |
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
- `value_examples`: `household_vs_commercial_janitorial_accounts`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `includechildren` | checkbox | Include Children | no | no |  | `purpose`: `share_class_with_child_subsidiaries` |
| `isinactive` | checkbox | Class is Inactive | no | no |  | `purpose`: `inactivate_class`; `ui_effect`: `hidden_from_class_lists` |
| `name` | text | Name | yes | no |  | `purpose`: `class_name`; `format`: `alphanumeric`; `max_length`: `60` characters; `displayed_on`: `lists` |
| `parent` | select | Subclass of | no | no |  | `purpose`: `parent_class` |
| `subsidiary` | select | Subsidiaries | no | no |  | `purpose`: `class_subsidiaries`; `requires_feature`: `OneWorld` [`real_name`] |
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
