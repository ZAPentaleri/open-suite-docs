# Employee Status (`employeestatus`)
<!-- id: record:employeestatus -->

| Fact | Value |
|---|---|
| Internal ID | `employeestatus` |
| Category | entity |
| Client scripts | yes |
| Server scripts | yes |
| Tags | `no_transform` |

## Record facts

- `script_types`: `client`, `server`
- `script_functions`: `read`, `create`, `update`, `copy`, `delete`, `search`
- `purpose`: `categorize_employee_work_status`
- `value_examples`: `part_time`, `full_time`, `temporary`, `regular`
- `displayed_on`: `employee_human_resources_subtab`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `category` | select | Category | no |  |  | `purpose`: `employee_list_category`; `cannot`: `change_after_save` |
| `isinactive` | checkbox | Inactive | no |  |  | `purpose`: `inactivate_list_value` |
| `name` | text | Employee Status Name | yes |  |  | `purpose`: `list_value_name`; `displayed_on`: `dropdowns` |
| `externalid` | text | External ID | no |  |  |  |
| `id` | integer | Internal ID | no |  |  |  |
| `internalid` | integer | Internal ID | no |  |  |  |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `category` | select | Category |  |
| `externalid` | select | External ID |  |
| `externalidstring` | text | External ID (Text) |  |
| `isinactive` | checkbox | Inactive |  |
| `name` | text | Name |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `category` | select | Category |  |
| `externalid` | select | External ID |  |
| `isinactive` | checkbox | Inactive |  |
| `name` | text | Status |  |
