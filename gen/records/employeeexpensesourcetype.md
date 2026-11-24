# Employee Expense Source Type (`employeeexpensesourcetype`)
<!-- id: record:employeeexpensesourcetype -->

| Fact | Value |
|---|---|
| Internal ID | `employeeexpensesourcetype` |
| Category | transaction |
| Client scripts | yes |
| Server scripts | yes |
| Searchable | yes |
| Tags | `scripting_full` |

## Record facts

- `script_types`: `client`, `server`
- `script_functions`: `create`, `update`, `copy`, `delete`, `search`, `transform`
- `requires_feature`: `Expense Report` [`real_name`]
- `configured_by`: `enable_features_employees_subtab` [`expense_reports`]

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `customform` | select | Custom Form | no |  |  |  |
| `description` | textarea | Description | no |  |  |  |
| `externalid` | text | External ID | no |  |  |  |
| `id` | integer | Internal ID | no |  |  |  |
| `isinactive` | checkbox | Inactive | no |  |  |  |
| `name` | text | Name | yes |  |  |  |
| `templatestored` | checkbox |  | no |  |  |  |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `description` | textarea | Description |  |
| `externalid` | select | External ID |  |
| `externalidstring` | text | External ID (Text) |  |
| `id` | select | Internal ID |  |
| `idnumber` | integer | Internal ID (Number) |  |
| `isinactive` | checkbox | Inactive |  |
| `name` | text | Name |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `description` | textarea | Description |  |
| `externalid` | select | External ID |  |
| `id` | select | Internal ID |  |
| `isinactive` | checkbox | Inactive |  |
| `name` | text | Name |  |
