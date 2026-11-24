# Payment Method (`paymentmethod`)
<!-- id: record:paymentmethod -->

| Fact | Value |
|---|---|
| Internal ID | `paymentmethod` |
| Category | list |
| Client scripts | yes |
| Server scripts | yes |
| Searchable | yes |
| Tags | `custom_fields_supported`, `scripting_full` |

## Record facts

- `script_types`: `client`, `server`
- `script_functions`: `create`, `update`, `copy`, `delete`, `search`
- `purpose`: `options_for_payment_method_field_and_web_store`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `account` | select | Account | no | no |  | `purpose`: `default_account`; `effect`: `fills_deposit_to_for_payment_method`; `purpose`: `landed_cost_clearing_account` (when `landed_cost_category`); `purpose`: `account_debited_for_project_time` (when `project_expense_type`); `shown_only_when`: `landed_cost_category`; `configured_by`: `expand_accounts_preference` (when `landed_cost_category_or_project_expense_type`) [`any_account`]; not `configured_by`: `expand_accounts_preference` (when `payment_method`); `doc_inconsistency`: `help_typo_landeded_for_landed` [`needs_human_review`] |
| `isdebitcard` | checkbox | Debit Card | no | no |  | `purpose`: `is_debit_card` |
| `isinactive` | checkbox | Inactive | no | no |  | `purpose`: `inactivate_record`; `can`: `reactivate_any_time`; `doc_inconsistency`: `says_removes_all_references` |
| `isonline` | checkbox | Display in Web Site | no | no |  | `purpose`: `available_in_web_store` |
| `merchantaccounts` | select | Supporting Merchant Accounts | no | no |  | `purpose`: `supporting_merchant_accounts`; `value_source`: `system_displayed` |
| `name` | text | Payment Method | yes | no |  | `purpose`: `record_name`; `displayed_on`: `lists` |
| `creditcard` | checkbox | Credit Card | no | no |  |  |
| `externalid` | text | ExternalId | no | no |  |  |

## Sublist `visuals` (Payment Visuals)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `flags` | text | Flags | yes |  |  |
| `location` | url | URL | yes |  |  |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `account` | select | Account |  |
| `creditcard` | checkbox | Credit Card |  |
| `externalid` | select | External ID |  |
| `externalidstring` | text | External ID (Text) |  |
| `internalid` | select | Internal ID |  |
| `internalidnumber` | integer | Internal ID (Number) |  |
| `isdebitcard` | checkbox | Is Debit Card |  |
| `isinactive` | checkbox | Inactive |  |
| `name` | text | Name |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `account` | select | Account |  |
| `creditcard` | checkbox | Credit Card |  |
| `externalid` | select | External ID |  |
| `internalid` | select | Internal ID |  |
| `isdebitcard` | checkbox | Is Debit Card |  |
| `isinactive` | checkbox | Inactive |  |
| `name` | text | Name |  |

## Search joins

| Join ID | Label | Joined record |
|---|---|---|
| `user` | User | Employee |

## Suspected source errors

> [!SOURCE-ERROR] `help_typo_landeded_for_landed` (on `account`)
<!-- source-error: record:paymentmethod | account | help_typo_landeded_for_landed -->

> [!SOURCE-ERROR] `says_removes_all_references` (on `isinactive`)
<!-- source-error: record:paymentmethod | isinactive | says_removes_all_references -->
