# Accounting Book (`accountingbook`)
<!-- id: record:accountingbook -->

| Fact | Value |
|---|---|
| Internal ID | `accountingbook` |
| Category | list |
| Client scripts | no |
| Server scripts | yes |
| Tags | `custom_fields_supported`, `no_copy` |

## Record facts

- `user_events`: `beforeLoad`, `beforeSubmit`, `afterSubmit`
- `requires_feature`: `Multi-Book Accounting` [`real_name`]
- `script_types`: `server`
- `user_events`: `beforeLoad`, `beforeSubmit`, `afterSubmit`
- `purpose`: `secondary_accounting_books`
- `ui_location`: `setup_accounting_accounting_books_new`
- `script_functions`: `create`, `update`, `delete`, `search`

## Body fields

| Field ID | Type | Label | Required | submitField | Read-only | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `effectiveperiod` | select | Effective Period | yes | no |  |  | `purpose`: `historical_base_period_of_secondary_book`; `allowed_values`: `primary_book_periods`; `default_behavior`: `first_open_period_listed` (when `no_closed_periods`); `editable_only_when`: `secondary_book_status_pending`; `cannot`: `change` (when `pending_book_has_transactions`); `cannot`: `change` (when `book_deleting_or_processing_history`) |
| `includechildren` | checkbox | Include Children | no | no | yes |  | `purpose`: `child_subsidiaries_included`; `effect`: `child_subsidiaries_associated_automatically`; `cannot`: `select_parent_with_only_one_child` |
| `isadjustmentonly` | checkbox | Adjustment Only | no | no |  |  | `purpose`: `create_as_adjustment_only_book`; `true_means`: `book_stores_adjustments_not_copies`; `can`: `budget_and_report_combined_with_primary`; `caveat`: `not_counted_against_accounting_book_limit` |
| `isconsolidated` | checkbox | Enable Consolidation | no | no |  |  | `purpose`: `enable_secondary_book_for_consolidation`; `requires_feature`: `OneWorld` [`real_name`]; `default_value`: `true` (when `primary_book`) |
| `isprimary` | checkbox | Is Primary | no | no | yes |  | `purpose`: `marks_primary_book`; `cannot`: `check_for_secondary_book`; `cannot`: `clear_for_primary_book` |
| `name` | text | Name | yes | no |  |  | `purpose`: `accounting_book_name`; `constraint`: `unique` |
| `status` | select | Status | yes | no |  |  | `purpose`: `accounting_book_status`; `allowed_values`: `Pending`, `Active`, `Inactive`; `default_value`: `Pending` (when `new_book`); `requires`: `book_currency_set_in_each_subsidiary` (when `currency_management`); `cannot`: `reactivate_after_deactivation` |
| `subsidiary` | select | Subsidiaries | no | no |  |  | `purpose`: `subsidiaries_of_accounting_book`; `effect`: `child_subsidiaries_associated_automatically` |
| `basebook` | select | Base Book | no | no |  |  |  |
| `customform` | select | Custom Form | no | no |  |  |  |
| `externalid` | text | ExternalId | no | no |  |  |  |

## Sublist `accountingbookcurrencies` (Currencies)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `currency` | select | Base Currency | no |  |  |
| `effectivedate` | date | Effective Date | no |  |  |
| `exchangerate` | float | Translation exchange rate | no |  |  |
| `primarybookcurrency` | select | Primary Book Base Currency | no |  |  |
| `subsidiary` | text | Subsidiary | no |  |  |
| `subsidiarykey` | text | key | no |  |  |
| `subsidiarystatus` | text | Status | no |  |  |

## Sublist `accountingbooksubsidiaries` (Subsidiaries)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `currency` | select | Base Currency | no |  |  |
| `effectivedate` | date | Effective Date | no |  |  |
| `exchangerate` | float | Translation exchange rate | no |  |  |
| `primarybookcurrency` | select | Primary Book Base Currency | no |  |  |
| `status` | select | Status | no |  |  |
| `subsidiary` | select | Subsidiary | no |  |  |

## Tabs

| Tab ID | Label |
|---|---|
| `workflow` | Workflow |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `basebook` | select | Base Book |  |
| `currency` | select | Currency |  |
| `externalid` | select | External ID |  |
| `externalidstring` | text | External ID (Text) |  |
| `formuladate` | date | Formula (Date) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulatext` | text | Formula (Text) |  |
| `includechildren` | checkbox | Include Children |  |
| `internalid` | select | Internal ID |  |
| `internalidnumber` | integer | Internal ID (Number) |  |
| `isadjustmentonly` | checkbox | Is Adjustment Only |  |
| `isprimary` | checkbox | Primary |  |
| `name` | text | Name |  |
| `status` | select | Status |  |
| `subsidiary` | select | Subsidiary |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `basebook` | select | Base Book |  |
| `currency` | select | Currency |  |
| `externalid` | select | External ID |  |
| `formulacurrency` | currency | Formula (Currency) |  |
| `formuladate` | date | Formula (Date) |  |
| `formuladatetime` | datetime | Formula (Date/Time) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulapercent` | percent | Formula (Percent) |  |
| `formulatext` | text | Formula (Text) |  |
| `includechildren` | checkbox | Include Children |  |
| `internalid` | select | Internal ID |  |
| `isadjustmentonly` | checkbox | Is Adjustment Only |  |
| `isprimary` | checkbox | Primary |  |
| `name` | text | Name |  |
| `status` | select | Status |  |
| `subsidiary` | select | Subsidiary |  |
