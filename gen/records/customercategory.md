# Customer Category (`customercategory`)
<!-- id: record:customercategory -->

| Fact | Value |
|---|---|
| Internal ID | `customercategory` |
| Category | list |
| Client scripts | yes |
| Server scripts | yes |
| Searchable | no |
| Tags | `no_search` |

## Record facts

- `script_types`: `client`, `server`
- `custom_fields_supported`: `false`
- `script_functions`: `create`, `update`, `copy`, `delete`
- `not_supported`: `search`
- `doc_inconsistency`: `functions_sentence_names_cost_category`
- `purpose`: `customer_type_values`
- `ui_location`: `setup_accounting_accounting_lists_type_customer_category_new`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `isinactive` | checkbox | Inactive | no | no |  | `purpose`: `inactivate_record`; `can`: `reactivate_any_time`; `doc_inconsistency`: `says_removes_all_references` |
| `name` | text | Customer | yes | no |  | `purpose`: `record_name`; `displayed_on`: `lists` |
| `externalid` | text | ExternalId | no | no |  |  |

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

## Suspected source errors

> [!SOURCE-ERROR] `functions_sentence_names_cost_category`
<!-- source-error: record:customercategory | - | functions_sentence_names_cost_category -->

> [!SOURCE-ERROR] `says_removes_all_references` (on `isinactive`)
<!-- source-error: record:customercategory | isinactive | says_removes_all_references -->
