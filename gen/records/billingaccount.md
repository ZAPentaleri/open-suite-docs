# Billing Account (`billingaccount`)
<!-- id: record:billingaccount -->

| Fact | Value |
|---|---|
| Internal ID | `billingaccount` |
| Category | list |
| Client scripts | yes |
| Server scripts | yes |
| Searchable | yes |
| Tags | `custom_fields_supported`, `scripting_full` |

## Record facts

- `script_types`: `client`, `server`
- `script_functions`: `create`, `update`, `copy`, `delete`, `search`
- `doc_inconsistency`: `record_list_spells_id_billlingaccount` [`needs_human_review`]

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `billingschedule` | select | Billing Schedule | yes | no |  | `purpose`: `billing_schedule` |
| `cashsaleform` | select | Cash Sale Form | no | no |  | `purpose`: `cash_sale_form` |
| `currency` | select |  | yes | no |  | `purpose`: `customer_currency`; `cannot`: `change_after_customer_transactions_saved`; `effect`: `all_customer_transactions_in_this_currency`; `requires_feature`: `Multi-Currency Customers` (when `primary_currency`) [`real_name`]; `effect`: `default_transaction_and_balance_currency` (when `multi_currency_customers`); `value_source`: `parent_customer_currency` (when `subcustomer`); `constraint`: `changeable_only_without_other_name_records`; `requires`: `re_enter_credit_limit_after_change`; `doc_inconsistency`: `says_cannot_change_then_can_change`; `effect`: `converted_to_base_currency_in_registers_and_reports`; `can`: `add_currencies_on_currencies_subtab` (when `multi_currency_customers`); `allowed_values_listed_in`: `currency_records` |
| `customer` | select | Customer | yes | no |  | `purpose`: `customer_of_billing_account` |
| `customerdefault` | checkbox | Customer Default | no | no |  | `purpose`: `default_billing_account_flag`; `true_means`: `default_billing_account_of_customer` |
| `customform` | select | Custom Form | no | no |  | `purpose`: `entry_form`; `special_values`: `Standard Customer Form`; `allowed_values_listed_in`: `standard_and_custom_entry_forms`; `ui_effect`: `new_option_creates_record` |
| `frequency` | select | Frequency | no | no |  | `purpose`: `billing_frequency`; `value_source`: `billing_schedule` |
| `idnumber` | text | Account Number | no | no |  | `purpose`: `billing_account_number`; `value_source`: `auto_generated_on_save` |
| `inactive` | checkbox | Inactive | no | no |  | `purpose`: `inactivate_billing_account` |
| `invoiceform` | select | Invoice Form | no | no |  | `purpose`: `invoice_form` |
| `memo` | textarea | Account Description | no | no |  | `purpose`: `billing_account_description` |
| `name` | text | Name | yes | no |  | `purpose`: `billing_account_name` |
| `startdate` | date | Start Date | yes | no |  | `purpose`: `relationship_or_contract_start_date`; `effect`: `updated_by_estimate_or_opportunity_date` |
| `subsidiary` | select |  | no | no |  | `purpose`: `entity_subsidiary`; `requires`: `set_before_entering_transactions`; `effect`: `transactions_associated_with_subsidiary`; `effect`: `entity_access_limited_to_subsidiary`; `cannot`: `change_after_transaction_posted` |
| `class` | select | Class | no | no |  |  |
| `createdby` | text | Created By | no | no |  |  |
| `createddate` | datetime | Created | no | no |  |  |
| `department` | select | Department | no | no |  |  |
| `externalid` | text | ExternalId | no | no |  |  |
| `idnumberexternal` | text | External Id | no | no |  |  |
| `lastbillcycledate` | date | Last Bill Cycle Date | no | no |  |  |
| `lastbilldate` | date | Last Bill Date | no | no |  |  |
| `nextbillcycledate` | date | Next Bill Cycle Date | no | no |  |  |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `billingschedule` | select | Billing Schedule |  |
| `cashsaleform` | select | Cash Sale Form |  |
| `currency` | select | Currency |  |
| `customer` | select | Customer |  |
| `customerdefault` | checkbox | Customer Default |  |
| `datecreated` | datetime | Date Created |  |
| `formuladate` | date | Formula (Date) |  |
| `frequency` | select | Frequency |  |
| `idnumber` | text | Account Number |  |
| `idnumberexternal` | text | External Id |  |
| `inactive` | checkbox | Inactive |  |
| `internalid` | select | Internal ID |  |
| `internalidnumber` | integer | Internal ID (Number) |  |
| `invoiceform` | select | Invoice Form |  |
| `lastbillcycledate` | date | Last Bill Cycle Date |  |
| `lastbilldate` | date | Last Bill Date |  |
| `memo` | text | Account Description |  |
| `name` | text | Name |  |
| `nextbillcycledate` | date | Next Bill Cycle Date |  |
| `startdate` | date | Start Date |  |
| `subsidiary` | select | Subsidiary |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `billingschedule` | select | Billing Schedule |  |
| `cashsaleform` | select | Cash Sale Form |  |
| `currency` | select | Currency |  |
| `customer` | select | Customer |  |
| `customerdefault` | checkbox | Customer Default |  |
| `datecreated` | datetime | Date Created |  |
| `frequency` | select | Frequency |  |
| `idnumber` | text | Account Number |  |
| `idnumberexternal` | text | External Id |  |
| `inactive` | checkbox | Inactive |  |
| `internalid` | select | Internal ID |  |
| `invoiceform` | select | Invoice Form |  |
| `lastbillcycledate` | date | Last Bill Cycle Date |  |
| `lastbilldate` | date | Last Bill Date |  |
| `memo` | text | Account Description |  |
| `name` | text | Name |  |
| `nextbillcycledate` | date | Next Bill Cycle Date |  |
| `startdate` | date | Start Date |  |
| `subsidiary` | select | Subsidiary |  |

## Suspected source errors

> [!SOURCE-ERROR] `record_list_spells_id_billlingaccount`
<!-- source-error: record:billingaccount | - | record_list_spells_id_billlingaccount -->

> [!SOURCE-ERROR] `says_cannot_change_then_can_change` (on `currency`)
<!-- source-error: record:billingaccount | currency | says_cannot_change_then_can_change -->
