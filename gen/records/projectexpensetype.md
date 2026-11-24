# Project Expense Type (`projectexpensetype`)
<!-- id: record:projectexpensetype -->

| Fact | Value |
|---|---|
| Internal ID | `projectexpensetype` |
| Category | list |
| Client scripts | no |
| Server scripts | yes |
| Searchable | yes |
| Tags | `custom_fields_supported`, `scripting_full` |

## Record facts

- `script_types`: `server`
- `requires_feature`: `Job Costing and Project Budgeting` [`real_name`]
- `sublist_requires_feature`: `language:multi_language`
- `rule`: `language_sublist_update_only`
- `purpose`: `expense_classification_for_projects`
- `configured_by`: `enable_features_company_subtab` [`job_costing_and_project_budgeting`]
- `caveat`: `feature_may_need_account_representative`
- `ui_location`: `setup_accounting_setup_tasks_project_expense_types_new`
- `configured_by`: `enable_features_company_subtab` [`multi_language`, `language_sublist`]
- `cannot`: `add_language_sublist_lines_by_script`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `account` | select | Account | yes | no |  | `purpose`: `default_account`; `effect`: `fills_deposit_to_for_payment_method`; `purpose`: `landed_cost_clearing_account` (when `landed_cost_category`); `purpose`: `account_debited_for_project_time` (when `project_expense_type`); `shown_only_when`: `landed_cost_category`; `configured_by`: `expand_accounts_preference` [`any_account`] |
| `description` | textarea | Description | no | no |  | `purpose`: `list_item_description` |
| `isinactive` | checkbox | Inactive | no | no |  | `purpose`: `inactivate_record`; `can`: `reactivate_any_time`; `doc_inconsistency`: `says_removes_all_references` |
| `name` | text | Name | yes | no |  | `purpose`: `record_name`; `displayed_on`: `lists` |
| `externalid` | text | ExternalId | no | no |  |  |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `account` | select | Account |  |
| `description` | text | Description |  |
| `formuladate` | date | Formula (Date) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulatext` | text | Formula (Text) |  |
| `internalid` | select | Internal ID |  |
| `isinactive` | checkbox | Inactive |  |
| `name` | text | Name |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `account` | text | Account |  |
| `description` | text | Description |  |
| `formulacurrency` | currency | Formula (Currency) |  |
| `formuladate` | date | Formula (Date) |  |
| `formuladatetime` | datetime | Formula (Date/Time) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulapercent` | percent | Formula (Percent) |  |
| `formulatext` | text | Formula (Text) |  |
| `internalid` | select | Internal ID |  |
| `isinactive` | text | Inactive |  |
| `name` | text | Name |  |

## Search joins

| Join ID | Label | Joined record |
|---|---|---|
| `user` | User | Employee |

## Suspected source errors

> [!SOURCE-ERROR] `says_removes_all_references` (on `isinactive`)
<!-- source-error: record:projectexpensetype | isinactive | says_removes_all_references -->
