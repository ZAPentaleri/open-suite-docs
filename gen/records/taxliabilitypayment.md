# Tax Liability Payment (`taxliabilitypayment`)
<!-- id: record:taxliabilitypayment -->

| Fact | Value |
|---|---|
| Internal ID | `taxliabilitypayment` |
| Search data via | Transaction |

## Body fields

| Field ID | Type | Label | Required | submitField | Read-only | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `account` | select | Bank Account | yes |  |  |  | `purpose`: `payment_bank_account`; `allowed_values`: `bank_accounts_in_subsidiary_currency` |
| `adjaccount` | select | Adjustment Account | no |  |  |  | `purpose`: `adjustment_account` |
| `adjamount` | currency | Adjustment Amount | no |  |  |  | `purpose`: `adjustment_amount`; `option_meanings`: `positive:increases_payment`, `negative:decreases_payment` |
| `asofdate` | date | As of | no |  |  |  | `purpose`: `liabilities_as_of_date`; `default_behavior`: `current_date`; `effect`: `recalculates_tax_due` |
| `class` | select | Class | no |  |  |  | `purpose`: `class` |
| `department` | select | Department | no |  |  |  | `purpose`: `department` |
| `location` | select | Location | no |  |  |  | `purpose`: `location` |
| `memo` | text | Memo | no |  |  |  | `purpose`: `memo` |
| `nexus` | select | Nexus | yes |  |  |  | `purpose`: `nexus` |
| `paymentamount` | currency | Payment Amount | no |  |  |  | `purpose`: `payment_amount`; `value_source`: `tax_due_plus_adjustments` |
| `postingperiod` | select | Posting Period | no |  |  |  | `purpose`: `posting_period` |
| `subsidiary` | select | Subsidiary | yes |  |  |  | `purpose`: `subsidiary` |
| `taxaccount` | select | Tax Account | yes |  |  |  | `purpose`: `tax_control_account` |
| `taxagency` | select | Tax Agency | yes |  | yes |  | `purpose`: `tax_agency`; `value_source`: `subsidiary_and_nexus` |
| `taxdue` | currency | Tax Due | no |  |  |  | `purpose`: `tax_due`; `value_source`: `system_displayed` |
| `trandate` | date | Date | yes |  |  |  | `purpose`: `payment_date` |
| `tranid` | text | Check # | no |  |  |  | `purpose`: `check_number`; `value_source`: `system_assigned`; `can`: `override` |
| `transactionnumber` | text | Transaction Number | no |  |  |  | `purpose`: `transaction_number`; `value_source`: `system_generated` |
| `createddate` | datetime | Date Created | no |  |  |  |  |
| `externalid` | text | External ID | no |  |  |  |  |
| `lastmodifieddate` | datetime | Date Last Modified | no |  |  |  |  |
| `status` | select | Status | no |  |  |  |  |
| `subsidiarytaxreg` | select | Subsidiary Tax Reg. Number | no |  |  |  |  |
| `voided` | checkbox | Voided | no |  |  |  |  |
| `voidjournal` | select | Voided Journal | no |  |  |  |  |
