# Bonus Type (`bonustype`)
<!-- id: record:bonustype -->

| Fact | Value |
|---|---|
| Internal ID | `bonustype` |
| Category | entity |
| Client scripts | yes |
| Server scripts | yes |
| Searchable | yes |
| Tags | `supports_copy`, `supports_create`, `supports_delete`, `supports_read`, `supports_search`, `supports_update` |

## Record facts

- `script_types`: `client`, `server`
- `script_functions`: `read`, `create`, `update`, `copy`, `delete`, `search`
- `requires_feature`: `Compensation Tracking` [`real_name`]
- `purpose`: `bonus_types_in_compensation_plans`
- `value_examples`: `holiday`, `sign_on`, `merit`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `isinactive` | checkbox | Inactive | no |  |  | `purpose`: `inactivate_bonus_type`; `ui_effect`: `hidden_from_lists_and_searches` |
| `name` | text | Name | yes |  |  | `purpose`: `bonus_type_name`; `recommendation`: `unique_name` |
| `payrollitem` | select | Payroll Item | no |  |  | `purpose`: `payroll_item_of_bonus_type`; `cannot`: `change_after_save`; `applies_when`: `employees_in_suitepeople_us_payroll` |
| `subsidiary` | select | Subsidiary | yes |  |  | `purpose`: `subsidiary_of_bonus_type`; `cannot`: `change_after_save` |
| `externalid` | text | External ID | no |  |  |  |
| `id` | integer | Internal ID | no |  |  |  |
| `internalid` | integer | Internal ID | no |  |  |  |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `externalid` | select | External ID |  |
| `externalidstring` | text | External ID (Text) |  |
| `internalid` | select | Internal ID |  |
| `internalidnumber` | integer | Internal ID (Number) |  |
| `isinactive` | checkbox | Inactive |  |
| `name` | text | Name |  |
| `payrollitem` | select | Payroll Item |  |
| `subsidiary` | select | Subsidiary |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `externalid` | select | External ID |  |
| `internalid` | select | Internal ID |  |
| `isinactive` | checkbox | Inactive |  |
| `name` | text | Name |  |
| `payrollitem` | select | Payroll Item |  |
| `subsidiary` | select | Subsidiary |  |
