# Vendor-Subsidiary Relationship (`vendorsubsidiaryrelationship`)
<!-- id: record:vendorsubsidiaryrelationship -->

| Fact | Value |
|---|---|
| Internal ID | `vendorsubsidiaryrelationship` |
| Category | list |
| Client scripts | yes |
| Server scripts | yes |
| Searchable | yes |
| Tags | `custom_fields_supported`, `scripting_full` |

## Record facts

- `script_types`: `client`, `server`
- `user_events`: `beforeLoad`, `beforeSubmit`, `afterSubmit`
- `requires`: `oneworld`
- `user_events`: `beforeLoad`, `beforeSubmit`, `afterSubmit`
- `script_functions`: `read`, `create`, `update`, `delete`
- `rule`: `shares_external_id_with_customer_relationship_of_same_entity`
- `value_examples`: `company_abc_vendor_and_customer_of_us_sub_share_external_id`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `balance` | currency | Balance | no |  |  | `purpose`: `ap_balance`; `value_source`: `system_displayed` |
| `creditlimit` | currency | Credit Limit | no |  |  | `purpose`: `vendor_credit_limit`; `value_unit`: `vendor_primary_currency`; `default_behavior`: `no_limit` (when `blank`); `special_values`: `0`; `option_meanings`: `0:vendor_on_hold_warning`; `constraint`: `not_negative`; `effect`: `validates_po_and_bill_against_global_limit`; `caveat`: `subsidiary_limits_not_in_global_validation` |
| `subsidiary` | select | Subsidiary | no |  |  | `purpose`: `vendor_subsidiary`; `requires`: `set_before_entering_transactions`; `default_behavior`: `primary_subsidiary_base_currency` [`vendor_currency`]; `cannot`: `change_after_transaction_posted`; `cannot`: `change_primary_subsidiary_after_save` (when `oneworld`) |
| `taxitem` | select | Tax Code | no |  |  | `purpose`: `default_tax_code`; `can`: `change_per_transaction` |
| `unbilledorders` | currency | Unbilled Orders | no |  |  | `purpose`: `unbilled_orders_total`; `effect`: `in_credit_limit` (when `vendor_credit_limit_includes_orders`) |
| `balancebase` | currency | Balance (Base) | no |  |  |  |
| `basecurrency` | text | Base Currency | no |  |  |  |
| `entity` | select | Vendor | no |  |  |  |
| `externalid` | text | External Id | no |  |  |  |
| `id` | integer | Internal ID | no |  |  |  |
| `internalid` | integer | Internal ID | no |  |  |  |
| `isprimarysub` | checkbox | Primary Subsidiary | no |  |  |  |
| `prepaymentbalance` | currency | Prepayment Balance | no |  |  |  |
| `prepaymentbalancebase` | currency | Prepayment Balance (Base) | no |  |  |  |
| `primarycurrency` | text | Primary Currency | no |  |  |  |
| `unbilledordersbase` | currency | Unbilled Orders (Base) | no |  |  |  |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `balance` | currency | Balance |  |
| `balancebase` | currency | Balance (Base) |  |
| `basecurrency` | select | Base Currency |  |
| `creditlimit` | currency | Credit Limit |  |
| `entity` | select | Vendor |  |
| `externalid` | select | External ID |  |
| `externalidstring` | text | External ID (Text) |  |
| `internalid` | select | Internal ID |  |
| `internalidnumber` | integer | Internal ID (Number) |  |
| `isprimarysub` | checkbox | Primary (Y/N) |  |
| `primarycurrency` | select | Primary Currency |  |
| `subsidiary` | select | Subsidiary |  |
| `taxitem` | select | Tax code |  |
| `unbilledorders` | currency | Unbilled Orders |  |
| `unbilledordersbase` | currency | Unbilled Orders (Base) |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `balance` | currency | Balance |  |
| `balancebase` | currency | Balance (Base) |  |
| `basecurrency` | select | Base Currency |  |
| `creditlimit` | currency | Credit limit |  |
| `entity` | select | VENDOR |  |
| `externalid` | select | External ID |  |
| `internalid` | integer | Internal ID |  |
| `isprimarysub` | checkbox | Primary (Y/N) |  |
| `primarycurrency` | select | Primary Currency |  |
| `subsidiary` | select | Subsidiary |  |
| `taxitem` | select | Tax code |  |
| `unbilledorders` | currency | Unbilled Orders |  |
| `unbilledordersbase` | currency | Unbilled Orders (Base) |  |

## Search joins

| Join ID | Label | Joined record |
|---|---|---|
| `subsidiary` | Subsidiary | Subsidiary |
| `vendor` | Vendor | Vendor |
