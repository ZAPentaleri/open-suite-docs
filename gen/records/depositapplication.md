# Deposit Application (`depositapplication`)
<!-- id: record:depositapplication -->

| Fact | Value |
|---|---|
| Internal ID | `depositapplication` |
| Category | transaction |
| Client scripts | yes |
| Server scripts | yes |
| Search data via | Transaction |
| Tags | `custom_fields_supported`, `no_create` |

## Record facts

- `script_types`: `client`, `server`
- `script_functions`: `copy`, `update`, `delete`, `search`
- `script_functions`: `copy`, `update`, `delete`, `search`
- `not_supported`: `create`
- `created_by`: `applying_customer_deposit_on_customer_payment`
- `purpose`: `apply_customer_deposit_to_invoice`
- `effect`: `backend_creates_application_for_applied_amount`
- `can`: `read_deposit_or_invoice_id_from_apply_doc_field`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `aracct` | select | A/R Account | no | no |  | `purpose`: `receivables_account`; `allowed_values_listed_in`: `accounts_receivable_accounts`; `ui_effect`: `new_option_creates_record` |
| `class` | select | Class | no | no |  | `purpose`: `transaction_class`; `allowed_values_listed_in`: `class_records`; `ui_effect`: `new_option_creates_record` |
| `currency` | select | Currency | no | no |  | `purpose`: `currency`; `value_source`: `system_displayed` |
| `customer` | select | Customer:Project | no | no |  | `purpose`: `customer_or_project`; `value_source`: `system_displayed` |
| `department` | select | Department | no | no |  | `purpose`: `transaction_department`; `allowed_values_listed_in`: `department_records`; `ui_effect`: `new_option_creates_record` |
| `deposit` | select | Application Of | no | no |  | `purpose`: `applied_customer_deposit`; `value_source`: `system_displayed` |
| `depositdate` | date | Deposit Date | no | no |  | `purpose`: `original_deposit_date`; `value_source`: `system_displayed` |
| `exchangerate` | currency2 | Exchange Rate | no | no |  | `purpose`: `exchange_rate`; `value_source`: `system_displayed` |
| `location` | select | Location | no | no |  | `purpose`: `transaction_location`; `allowed_values_listed_in`: `location_records`; `ui_effect`: `new_option_creates_record` |
| `memo` | text | Memo | no | yes |  | `purpose`: `memo` |
| `postingperiod` | select | Posting Period | no | no |  | `purpose`: `posting_period`; `cannot`: `post_to_closed_period` |
| `subsidiary` | select | Subsidiary | no | no |  | `purpose`: `subsidiary`; `value_source`: `system_displayed` |
| `trandate` | date | Date | yes | no |  | `purpose`: `transaction_date`; `default_behavior`: `current_date` |
| `applied` | currency | Applied | no | no |  |  |
| `createddate` | datetime |  | no | no |  |  |
| `externalid` | text | ExternalId | no | no |  |  |
| `isbasecurrency` | checkbox | Base Currency | no | no |  |  |
| `lastmodifieddate` | datetime |  | no | no |  |  |
| `status` | text | Status | no | no |  |  |
| `statusRef` | text |  | no | no |  |  |
| `total` | currency | To Apply | no | no |  |  |
| `unapplied` | currency | Unapplied | no | no |  |  |

## Sublist `accountingbookdetail` (Accounting Books)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `accountingbook` | select | Secondary Book | no |  |  |
| `exchangerate` | currency2 | Exchange Rate | yes |  |  |

## Sublist `apply` (Invoices)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `amount` | currency | Payment | no |  |  |
| `apply` | checkbox | Apply | no |  |  |
| `applydate` | date | Date | no |  |  |
| `createdfrom` | text |  | no |  |  |
| `doc` | text |  | no |  |  |
| `due` | currency | Amt. Due | no |  |  |
| `duedate` | date |  | no |  |  |
| `internalid` | text | Hidden | no |  |  |
| `job` | integer |  | no |  |  |
| `line` | integer |  | no |  |  |
| `refnum` | text | Ref No. | no |  |  |
| `total` | currency | Orig. Amt. | no |  |  |
| `url` | text |  | no |  |  |
