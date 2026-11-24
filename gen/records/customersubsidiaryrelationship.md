# Customer-Subsidiary Relationship (`customersubsidiaryrelationship`)
<!-- id: record:customersubsidiaryrelationship -->

| Fact | Value |
|---|---|
| Internal ID | `customersubsidiaryrelationship` |
| Category | list |
| Client scripts | yes |
| Server scripts | yes |
| Searchable | yes |
| Tags | `custom_fields_supported`, `scripting_full` |

## Record facts

- `script_types`: `client`, `server`
- `user_events`: `beforeLoad`, `beforeSubmit`, `afterSubmit`
- `requires_feature`: `Multi-Subsidiary Customer` [`real_name`]
- `requires`: `oneworld`
- `user_events`: `beforeLoad`, `beforeSubmit`, `afterSubmit`
- `script_functions`: `read`, `create`, `update`, `delete`
- `rule`: `shares_external_id_with_vendor_relationship_of_same_entity`
- `purpose`: `share_customer_with_multiple_subsidiaries`
- `configured_by`: `enable_features_company_subtab` [`multi_subsidiary_customer`]
- `value_examples`: `company_abc_vendor_and_customer_of_us_sub_share_external_id`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `balance` | currency | Balance | no |  |  | `purpose`: `ar_balance`; `value_unit`: `customer_currency` (when `multiple_currencies`) |
| `depositbalance` | currency | Deposit Balance | no |  |  | `purpose`: `unapplied_deposit_balance`; `caveat`: `deposits_liability_not_in_ar_balance`; `value_unit`: `customer_currency` (when `multiple_currencies`) |
| `id` | integer | Internal ID | no |  |  | `purpose`: `internal_id`; `configured_by`: `show_internal_ids_preference` [`display`]; `value_source`: `system_assigned` |
| `subsidiary` | select | Subsidiary | no |  |  | `purpose`: `entity_subsidiary`; `requires`: `set_before_entering_transactions`; `effect`: `transactions_associated_with_subsidiary`; `effect`: `entity_access_limited_to_subsidiary`; `cannot`: `change_after_transaction_posted` |
| `unbilledorders` | currency | Unbilled Orders | no |  |  | `purpose`: `unbilled_orders_total`; `effect`: `in_credit_limit` (when `credit_limit_includes_orders`); `value_unit`: `customer_currency` (when `multiple_currencies`); `configured_by`: `customer_credit_limit_includes_orders` |
| `entity` | select | Customer | no |  |  |  |
| `externalid` | text | External Id | no |  |  |  |
| `internalid` | integer | Internal ID | no |  |  |  |
| `isprimarysub` | checkbox | Primary Subsidiary | no |  |  |  |
| `primarycurrency` | text | Primary Currency | no |  |  |  |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `balance` | currency | Balance |  |
| `depositbalance` | currency | Deposit Balance |  |
| `entity` | select | Customer |  |
| `externalid` | select | External ID |  |
| `externalidstring` | text | External ID (Text) |  |
| `internalid` | select | Internal ID |  |
| `internalidnumber` | integer | Internal ID (Number) |  |
| `isprimarysub` | checkbox | Primary (Y/N) |  |
| `primarycurrency` | select | Primary Currency |  |
| `subsidiary` | select | Subsidiary |  |
| `unbilledorders` | currency | Unbilled Orders |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `balance` | currency | Balance |  |
| `depositbalance` | currency | Deposit Balance |  |
| `entity` | select | Customer |  |
| `externalid` | select | External ID |  |
| `internalid` | integer | Internal ID |  |
| `isprimarysub` | checkbox | Primary (Y/N) |  |
| `primarycurrency` | select | Primary Currency |  |
| `subsidiary` | select | Subsidiary |  |
| `unbilledorders` | currency | Unbilled Orders |  |

## Search joins

| Join ID | Label | Joined record |
|---|---|---|
| `customer` | Customer | Customer |
| `subsidiary` | Subsidiary | Subsidiary |
