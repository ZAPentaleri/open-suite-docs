# Usage (`usage`)
<!-- id: record:usage -->

| Fact | Value |
|---|---|
| Internal ID | `usage` |
| Category | transaction |
| Client scripts | no |
| Server scripts | no |
| Searchable | yes |
| Tags | `custom_fields_supported`, `scripting_full` |

## Record facts

- `script_functions`: `create`, `update`, `copy`, `delete`, `search`
- `requires_feature`: `SuiteBilling` [`real_name`]
- `doc_inconsistency`: `says_no_client_or_server_script_support`
- `purpose`: `subscription_usage_for_billing`
- `configured_by`: `enable_features_transactions_subtab` [`suitebilling`]
- `ui_location`: `transactions_subscriptions_create_usages`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `customer` | select | Customer | no | no |  | `purpose`: `rate_card_customer`; `effect`: `rate_card_limited_to_customer_projects` |
| `customform` | select | Custom Form | no | no |  |  |
| `item` | select | Item | no | no |  |  |
| `memo` | textarea | Memo | no | no |  |  |
| `status` | select | Status | yes |  |  |  |
| `subscriptionplan` | select | Subscription Plan | no | no |  |  |
| `usagedate` | date | Date | yes | no |  |  |
| `usagequantity` | posfloat | Quantity | yes | no |  |  |
| `usagesubscription` | select | Subscription | yes | no |  |  |
| `usagesubscriptionline` | select | Subscription Line | yes | no |  |  |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `customer` | select | Customer |  |
| `date` | date | Date |  |
| `externalid` | select | External ID |  |
| `externalidstring` | text | External ID (Text) |  |
| `internalid` | select | Internal ID |  |
| `internalidnumber` | integer | Internal ID (Number) |  |
| `item` | select | Item |  |
| `memo` | text | Memo |  |
| `quantity` | float | Quantity |  |
| `status` | select | Status |  |
| `subscription` | select | Subscription |  |
| `subscriptionline` | select | Subscription Line |  |
| `subscriptionplan` | select | Subscription Plan |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `customer` | select | Customer |  |
| `date` | date | Date |  |
| `externalid` | text | External ID |  |
| `internalid` | integer | Internal ID |  |
| `item` | select | Item |  |
| `memo` | text | Memo |  |
| `quantity` | float | Quantity |  |
| `status` | select | Status |  |
| `subscription` | select | Subscription |  |
| `subscriptionline` | select | Subscription Line |  |
| `subscriptionplan` | select | Subscription Plan |  |

## Suspected source errors

> [!SOURCE-ERROR] `says_no_client_or_server_script_support`
<!-- source-error: record:usage | - | says_no_client_or_server_script_support -->
