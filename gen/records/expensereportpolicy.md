# Expense Report Policy (`expensereportpolicy`)
<!-- id: record:expensereportpolicy -->

| Fact | Value |
|---|---|
| Internal ID | `expensereportpolicy` |
| Category | transaction |
| Client scripts | yes |
| Server scripts | yes |
| Searchable | yes |
| Tags | `supports_copy`, `supports_create`, `supports_delete`, `supports_read`, `supports_search`, `supports_update` |

## Record facts

- `script_types`: `client`, `server`
- `script_functions`: `read`, `create`, `update`, `copy`, `delete`, `search`
- `requires_feature`: `Expense Reports` [`real_name`]
- `subrecords`: `erpolicynotification`
- `sublists`: `notification`
- `purpose`: `limit_employee_expense_entries`
- `constraint`: `report_must_satisfy_all_filters_any_value_per_filter`
- `value_examples`: `airfare_or_meals_for_parent_subsidiary_employees`
- `configured_by`: `enable_features_employees_subtab` [`expense_reports`]

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `action` | select | Action | yes |  |  | `purpose`: `policy_violation_action`; `options`: `allow_submission_with_overage`, `require_adjustment` |
| `customer` | multiselect | Customer | no |  |  | `purpose`: `policy_customers_filter`; `value_kind`: `multi_select`; `effect`: `policy_applies_to_lines_with_selected_customers` |
| `department` | multiselect | Department | no |  |  | `purpose`: `policy_departments_filter`; `value_kind`: `multi_select`; `effect`: `policy_applies_to_lines_with_selected_departments` |
| `description` | textarea | Description | no |  |  | `purpose`: `policy_description`; `displayed_on`: `policy_list` |
| `enddate` | date | End Date | no |  |  | `purpose`: `policy_end_date`; `default_behavior`: `applies_indefinitely` (when `blank`) |
| `expensecategory` | multiselect | Expense Category | yes |  |  | `purpose`: `policy_expense_categories_filter`; `value_kind`: `multi_select`; `effect`: `policy_applies_to_lines_with_selected_expense_categories` |
| `isinactive` | checkbox | Inactive | no |  |  | `purpose`: `inactivate_policy`; `true_means`: `policy_not_applied` |
| `limittype` | select | Limit Type | yes |  |  | `purpose`: `policy_limit_type`; `options`: `Per day`, `Per individual expense report` |
| `location` | multiselect | Location | no |  |  | `purpose`: `policy_locations_filter`; `value_kind`: `multi_select`; `effect`: `policy_applies_to_lines_with_selected_locations` |
| `maxamount` | currency2 | Maximum Amount | yes |  |  | `purpose`: `policy_maximum_amount`; `value_scope`: `per_report_or_per_day_by_limittype` |
| `name` | text | Name | yes |  |  | `purpose`: `policy_name`; `displayed_on`: `show_policies_list` |
| `requiresmemo` | checkbox | Requires Memo | no |  |  | `purpose`: `require_memo_on_affected_lines` |
| `requiresreceipt` | checkbox | Requires Receipt | no |  |  | `purpose`: `require_receipt_on_affected_lines` |
| `startdate` | date | Start Date | yes |  |  | `purpose`: `policy_start_date`; `caveat`: `past_date_not_applied_to_existing_reports` |
| `externalid` | text | External ID | no |  |  |  |
| `id` | integer | Internal ID | no |  |  |  |
| `isbillable` | select | Billable | no |  |  |  |
| `nowarningreqfieldincluded` | checkbox | Do not warn if required fields are included | no |  |  |  |
| `subsidiary` | multiselect | Subsidiary | no |  |  |  |

## Sublist `notification` (Overage Notification)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `entity` | select | Entity | no |  |  |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `action` | select | Action |  |
| `customer` | multiselect | Customer |  |
| `department` | multiselect | Department |  |
| `description` | textarea | Description |  |
| `enddate` | date | End Date |  |
| `expensecategory` | multiselect | Expense Category |  |
| `externalid` | select | External ID |  |
| `externalidstring` | text | External ID (Text) |  |
| `id` | select | Internal ID |  |
| `idnumber` | integer | Internal ID (Number) |  |
| `isbillable` | select | Billable |  |
| `isinactive` | checkbox | Inactive |  |
| `limittype` | select | Limit Type |  |
| `location` | multiselect | Location |  |
| `maxamount` | currency | Maximum Amount |  |
| `name` | text | Name |  |
| `nowarningreqfieldincluded` | checkbox | Do not warn if required fields are included |  |
| `requiresmemo` | checkbox | Requires Memo |  |
| `requiresreceipt` | checkbox | Requires Receipt |  |
| `startdate` | date | Start Date |  |
| `status` | select | Status |  |
| `subsidiary` | multiselect | Subsidiary |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `action` | select | Action |  |
| `customer` | multiselect | Customer |  |
| `department` | multiselect | Department |  |
| `description` | textarea | Description |  |
| `enddate` | date | End Date |  |
| `expensecategory` | multiselect | Expense Category |  |
| `externalid` | select | External ID |  |
| `id` | select | Internal ID |  |
| `isbillable` | select | Billable |  |
| `isinactive` | checkbox | Inactive |  |
| `limittype` | select | Limit Type |  |
| `location` | multiselect | Location |  |
| `maxamount` | currency | Maximum Amount |  |
| `name` | text | Name |  |
| `nowarningreqfieldincluded` | checkbox | Do not warn if required fields are included |  |
| `requiresmemo` | checkbox | Requires Memo |  |
| `requiresreceipt` | checkbox | Requires Receipt |  |
| `startdate` | date | Start Date |  |
| `status` | select | Status |  |
| `subsidiary` | multiselect | Subsidiary |  |

## Search joins

| Join ID | Label | Joined record |
|---|---|---|
| `department` | Department | Department |
| `expensecategory` | Expense Category | Expense Category |
| `location` | Location | Location |
| `subsidiary` | Subsidiary | Subsidiary |
