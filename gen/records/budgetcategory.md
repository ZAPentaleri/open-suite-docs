# Budget Category (`budgetcategory`)
<!-- id: record:budgetcategory -->

| Fact | Value |
|---|---|
| Internal ID | `budgetcategory` |
| Category | list |
| Client scripts | yes |
| Server scripts | yes |
| Searchable | yes |
| Tags | `supports_create`, `supports_delete`, `supports_read`, `supports_search`, `supports_update` |

## Record facts

- `script_types`: `client`, `server`
- `script_functions`: `create`, `read`, `update`, `delete`, `search`
- `requires_feature`: `Multiple Budgets` [`real_name`]
- `field_alias`: `global:budgetType`
- `not_updatable_fields`: `budgetType`
- `purpose`: `categorize_budgets_with_otherwise_identical_criteria`
- `option_meanings`: `local:subsidiary_currency_budgeting`, `global:root_subsidiary_currency_budgeting` [`budget_category_type`]
- `required_when`: `multiple_budgets` [`category_on_every_budget`]
- `configured_by`: `enable_features_accounting_subtab_advanced_features` [`multiple_budgets`]
- `requires_permission`: `administrator` (when `enabling_multiple_budgets`)
- `true_means`: `global_budget_in_root_parent_base_currency` [`budgettype_field`]
- `cannot`: `change_global_after_save_in_ui_or_script`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `budgettype` | checkbox | Global | yes |  |  | `purpose`: `budget_category_type`; `requires_feature`: `Multiple Budgets` [`real_name`]; `allowed_values`: `local`, `global`; `option_meanings`: `local:subsidiary_level_local_currency`, `global:root_subsidiary_currency` (when `oneworld`) |
| `isinactive` | checkbox | Inactive | no |  |  | `purpose`: `inactivate_record`; `can`: `reactivate_any_time`; `doc_inconsistency`: `says_removes_all_references` |
| `name` | text | Name | yes |  |  | `purpose`: `record_name`; `displayed_on`: `lists` |
| `externalid` | text | External ID | no |  |  |  |
| `id` | integer | Internal ID | no |  |  |  |

## Suspected source errors

> [!SOURCE-ERROR] `says_removes_all_references` (on `isinactive`)
<!-- source-error: record:budgetcategory | isinactive | says_removes_all_references -->
