# Cost Category (`costcategory`)
<!-- id: record:costcategory -->

| Fact | Value |
|---|---|
| Internal ID | `costcategory` |
| Category | list |
| Client scripts | yes |
| Server scripts | yes |
| Searchable | yes |
| Tags | `custom_fields_supported`, `scripting_full` |

## Record facts

- `script_types`: `client`, `server`
- `script_functions`: `create`, `update`, `copy`, `delete`, `search`
- `requires_feature`: `Standard Costing`, `Landed Cost` [`either`, `real_name`]
- `purpose`: `classify_item_costs_and_variances`
- `option_meanings`: `material_or_service:standard_costs`, `landed:purchase_expense_kinds`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `account` | select | Expense Account | yes | no |  | `purpose`: `default_account`; `effect`: `fills_deposit_to_for_payment_method`; `purpose`: `landed_cost_clearing_account` (when `landed_cost_category`); `purpose`: `account_debited_for_project_time` (when `project_expense_type`); `shown_only_when`: `landed_cost_category`; `configured_by`: `expand_accounts_preference` [`any_account`] |
| `isinactive` | checkbox | Inactive | no | no |  | `purpose`: `inactivate_record`; `can`: `reactivate_any_time`; `doc_inconsistency`: `says_removes_all_references` |
| `itemcosttype` | select | Cost Type | yes | no |  | `purpose`: `cost_type`; `options`: `Landed`, `Material`, `Service`; `requires_feature`: `Landed Cost` (when `landed_option`) [`real_name`]; `effect`: `landed_option_hidden_without_feature` |
| `name` | text | Name | yes | no |  | `purpose`: `record_name`; `displayed_on`: `lists` |
| `externalid` | text | ExternalId | no | no |  |  |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `account` | select | Account |  |
| `externalid` | select | External ID |  |
| `externalidstring` | text | External ID (Text) |  |
| `internalid` | select | Internal ID |  |
| `internalidnumber` | integer | Internal ID (Number) |  |
| `isinactive` | checkbox | Inactive |  |
| `itemcosttype` | select | Cost Type |  |
| `name` | text | Name |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `account` | select | Account |  |
| `externalid` | select | External ID |  |
| `internalid` | select | Internal ID |  |
| `isinactive` | checkbox | Inactive |  |
| `itemcosttype` | select | Cost Type |  |
| `name` | text | Name |  |

## Search joins

| Join ID | Label | Joined record |
|---|---|---|
| `account` | Account | Account |
| `transaction` | Transaction | Transaction |

## Suspected source errors

> [!SOURCE-ERROR] `says_removes_all_references` (on `isinactive`)
<!-- source-error: record:costcategory | isinactive | says_removes_all_references -->
