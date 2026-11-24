# Employee Type (`employeetype`)
<!-- id: record:employeetype -->

| Fact | Value |
|---|---|
| Internal ID | `employeetype` |
| Category | entity |
| Client scripts | yes |
| Server scripts | yes |
| Tags | `no_transform` |

## Record facts

- `script_types`: `client`, `server`
- `script_functions`: `read`, `create`, `update`, `copy`, `delete`, `search`
- `doc_inconsistency`: `script_type_sentence_names_employee_record`
- `purpose`: `categorize_employees_for_reporting`
- `value_examples`: `contractor`, `exempt`, `officer`, `owner`, `regular_employee`, `statutory_employee`
- `displayed_on`: `employee_human_resources_subtab`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `category` | select | Category | no |  |  | `purpose`: `employee_list_category`; `cannot`: `change_after_save` |
| `exempt` | checkbox | Exempt | no |  |  | `purpose`: `exempt_employee_type`; `true_means`: `not_covered_by_overtime_wage_rules` |
| `isinactive` | checkbox | Inactive | no |  |  | `purpose`: `inactivate_list_value` |
| `name` | text | Employee Type | yes |  |  | `purpose`: `list_value_name`; `displayed_on`: `dropdowns` |
| `externalid` | text | External ID | no |  |  |  |
| `id` | integer | Internal ID | no |  |  |  |
| `includeinkpi` | checkbox | Include in KPI | no |  |  |  |
| `internalid` | integer | Internal ID | no |  |  |  |
| `statutory` | checkbox | Statutory Employee | no |  |  |  |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `category` | select | Category |  |
| `exempt` | checkbox | Exempt |  |
| `externalid` | select | External ID |  |
| `externalidstring` | text | External ID (Text) |  |
| `includeinkpi` | checkbox | Include In KPI |  |
| `isinactive` | checkbox | Inactive |  |
| `name` | text | Name |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `category` | select | Category |  |
| `exempt` | checkbox | Exempt |  |
| `externalid` | select | External ID |  |
| `includeinkpi` | checkbox | Include in KPI |  |
| `isinactive` | checkbox | Inactive |  |
| `name` | text | Type |  |

## Suspected source errors

> [!SOURCE-ERROR] `script_type_sentence_names_employee_record`
<!-- source-error: record:employeetype | - | script_type_sentence_names_employee_record -->
