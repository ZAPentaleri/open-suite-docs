# Vendor Prepayment Application (`vendorprepaymentapplication`)
<!-- id: record:vendorprepaymentapplication -->

| Fact | Value |
|---|---|
| Internal ID | `vendorprepaymentapplication` |
| Category | transaction |
| Client scripts | yes |
| Server scripts | yes |
| Searchable | yes |
| Search data via | Transaction |
| Tags | `supports_delete`, `supports_read`, `supports_search`, `supports_transform`, `supports_update` |

## Record facts

- `script_types`: `client`, `server`
- `requires_permission`: `vendor_prepayment_application`
- `create_via`: `transform_from_vendorprepayment` (refs `N/record.transform`)
- `purpose`: `apply_prepayment_to_vendor_bills`
- `effect`: `credits_prepayment_offsets_ap`
- `configured_by`: `auto_apply_accounting_preference`
- `value_examples`: `apply_prepayment_718_to_first_bill`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `account` | select | A/P Account | yes |  |  | `purpose`: `payables_account`; `constraint`: `active_account` |
| `class` | select | Class | no |  |  | `purpose`: `class`; `constraint`: `only_values_of_selected_subsidiary` |
| `currency` | select | Currency | no |  |  | `purpose`: `currency`; `value_source`: `system_displayed` |
| `department` | select | Department | no |  |  | `purpose`: `department`; `constraint`: `only_values_of_selected_subsidiary` |
| `entity` | select | Payee | no |  |  | `purpose`: `payee` |
| `exchangerate` | currency2 | Exchange Rate | no |  |  | `purpose`: `exchange_rate`; `value_source`: `payee_currency`; `cannot`: `change` |
| `location` | select | Location | no |  |  | `purpose`: `location`; `constraint`: `only_values_of_selected_subsidiary`; `doc_inconsistency`: `location_help_says_departments` |
| `memo` | text | Memo | no |  |  | `purpose`: `memo`; `displayed_on`: `applications_list`, `reports`; `supported_in`: `workbooks` |
| `postingperiod` | select | Posting Period | no |  |  | `purpose`: `posting_period`; `cannot`: `post_to_closed_period`; `doc_inconsistency`: `stray_html_tag_in_help` |
| `prepaymentdate` | date | Prepayment Date | no |  |  | `purpose`: `prepayment_date`; `value_source`: `system_displayed` |
| `subsidiary` | select | Subsidiary | no |  |  | `purpose`: `subsidiary`; `can`: `change_from_default` |
| `trandate` | date | Date | yes |  |  | `purpose`: `posting_date`; `default_behavior`: `current_date` |
| `tranid` | text | Document Number | no |  |  | `purpose`: `document_number`; `default_value`: `To be generated` (when `new_transaction`); `value_source`: `system_generated_on_save` |
| `transactionnumber` | text | Transaction Number | no |  |  | `purpose`: `transaction_number`; `default_value`: `To be generated` (when `new_transaction`); `value_source`: `system_generated_on_save` |
| `vendorprepayment` | select | Application Of | no |  |  | `purpose`: `vendor_prepayment_link`; `value_source`: `system_displayed` |
| `createddate` | datetime | Date Created | no |  |  |  |
| `customform` | select | Custom Form | no |  |  |  |
| `externalid` | text | External ID | no |  |  |  |
| `isbasecurrency` | checkbox | Base Currency | no |  |  |  |
| `lastmodifieddate` | datetime | Date Last Modified | no |  |  |  |
| `prepaymentaccount` | select | Prepayment Account | no |  |  |  |

## Sublist `bill` (Bill)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `amount` | currency | Payment | no |  |  |
| `apply` | checkbox | Apply | no |  |  |
| `applydate` | date | Date | no |  |  |
| `currency` | text | Currency | no |  |  |
| `doc` | select |  | yes |  |  |
| `due` | currency | Amt. Due | no |  |  |
| `duedate` | date |  | no |  |  |
| `line` | integer |  | no |  |  |
| `total` | currency | Orig. Amt. | no |  |  |

## Suspected source errors

> [!SOURCE-ERROR] `location_help_says_departments` (on `location`)
<!-- source-error: record:vendorprepaymentapplication | location | location_help_says_departments -->

> [!SOURCE-ERROR] `stray_html_tag_in_help` (on `postingperiod`)
<!-- source-error: record:vendorprepaymentapplication | postingperiod | stray_html_tag_in_help -->
