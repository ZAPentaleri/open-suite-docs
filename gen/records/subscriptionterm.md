# Subscription Term (`subscriptionterm`)
<!-- id: record:subscriptionterm -->

| Fact | Value |
|---|---|
| Internal ID | `subscriptionterm` |
| Category | item |
| Client scripts | yes |
| Server scripts | yes |
| Searchable | yes |
| Tags | `supports_copy`, `supports_create`, `supports_delete`, `supports_read`, `supports_search`, `supports_update` |

## Record facts

- `script_types`: `client`, `server`
- `requires_feature`: `SuiteBilling` [`real_name`]
- `script_functions`: `create`, `read`, `update`, `delete`, `search`, `copy`
- `not_supported`: `transform`
- `purpose`: `length_of_subscription`
- `effect`: `term_available_to_future_plans_and_subscriptions`
- `configured_by`: `enable_features_transactions_subtab` [`suitebilling`]
- `value_examples`: `six_month_term`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `isinactive` | checkbox | Inactive | no |  |  | `purpose`: `inactivate_record`; `can`: `reactivate_any_time`; `doc_inconsistency`: `says_removes_all_references` |
| `externalid` | text | External ID | no |  |  |  |
| `id` | integer | Internal ID | no |  |  |  |
| `name` | text | Name | yes |  |  |  |
| `subscriptiontermduration` | posinteger | Duration | yes |  |  |  |
| `subscriptiontermtype` | select | Type | yes |  |  |  |
| `subscriptiontermunit` | select | Units | yes |  |  |  |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `externalid` | select | External ID |  |
| `externalidstring` | text | External ID (Text) |  |
| `internalid` | select | Internal ID |  |
| `internalidnumber` | integer | Internal ID (Number) |  |
| `name` | text | Name |  |
| `subscriptiontermduration` | integer | Duration |  |
| `subscriptiontermtype` | select | Type |  |
| `subscriptiontermunit` | select | Units |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `externalid` | select | External ID |  |
| `internalid` | select | Internal ID |  |
| `name` | text | Name |  |
| `subscriptiontermduration` | integer | Duration |  |
| `subscriptiontermtype` | select | Type |  |
| `subscriptiontermunit` | select | Units |  |

## Suspected source errors

> [!SOURCE-ERROR] `says_removes_all_references` (on `isinactive`)
<!-- source-error: record:subscriptionterm | isinactive | says_removes_all_references -->
