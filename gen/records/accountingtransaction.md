# Accounting Transaction (`accountingtransaction`)
<!-- id: record:accountingtransaction -->

| Fact | Value |
|---|---|
| Internal ID | `accountingtransaction` |
| Category | transaction |
| Client scripts | yes |
| Server scripts | yes |
| Searchable | yes |
| Tags | `deleted_record_search_supported`, `search_only`, `supports_search` |

## Record facts

- `script_types`: `client`, `server`
- `script_functions`: `search`
- `not_supported`: `create`, `copy`
- `requires_feature`: `Multi-Book Accounting` [`real_name`]
- `purpose`: `search_by_accounting_book`
- `ui_location`: `reports_new_search_multi_book_accounting_transaction`
- `value_examples`: `transaction_with_accounting_book_and_fx_rate_columns`

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `account` | select | Account |  |
| `accountingbook` | select | Accounting Book |  |
| `accounttype` | select | Account Type |  |
| `altsalesamount` | currency | Amount (Alt.Sales) |  |
| `altsalesnetamount` | currency | Amount (Alt.Sales Net) |  |
| `amount` | currency | Amount |  |
| `bookspecifictransaction` | checkbox | Book Specific Transaction |  |
| `catchupperiod` | select | Catch Up Period |  |
| `creditamount` | currency | Amount (Credit) |  |
| `customgl` | checkbox | Custom GL |  |
| `customscript` | select | Custom Script |  |
| `debitamount` | currency | Amount (Debit) |  |
| `deferrevrec` | checkbox | Hold Revenue Recognition |  |
| `exchangerate` | currency | Exchange Rate |  |
| `formuladate` | date | Formula (Date) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulatext` | text | Formula (Text) |  |
| `grossamount` | currency | Amount (Gross) |  |
| `internalid` | select | Internal ID |  |
| `internalidnumber` | integer | Internal ID (Number) |  |
| `multisubsidiary` | checkbox | Intercompany |  |
| `netamount` | currency | Amount (Net) |  |
| `posting` | checkbox | Posting |  |
| `quantityrevcommitted` | float | Quantity Rev. Committed |  |
| `recognizedrevenue` | currency | Amount (Recognized) |  |
| `revcommitstatus` | select | Rev. Commit. Status |  |
| `revenuestatus` | select | Revenue Status |  |
| `revrecenddate` | date | Rev. Rec. End Date |  |
| `revreconrevcommitment` | checkbox | Rev. Rec. On Rev. Committment |  |
| `revrecstartdate` | date | Rev. Rec. Start Date |  |
| `subsidiary` | select | Subsidiary |  |
| `tranisvsoebundle` | checkbox | Transaction Is VSOE Bundle |  |
| `type` | select | Type |  |
| `vsoeallocation` | currency | VSOE Allocation |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `account` | select | Account |  |
| `accountingbook` | select | Accounting Book |  |
| `accounttype` | text | Account Type |  |
| `altsalesamount` | currency | Amount (Alt.Sales) |  |
| `altsalesnetamount` | currency | Amount (Alt.Sales Net) |  |
| `amount` | currency | Amount |  |
| `basecurrency` | text | Base Currency |  |
| `catchupperiod` | text | Catch Up Period |  |
| `creditamount` | currency | Amount (Credit) |  |
| `customgl` | checkbox | Custom GL |  |
| `customscript` | text | Custom Script |  |
| `datecreated` | datetime | Date Created |  |
| `debitamount` | currency | Amount (Debit) |  |
| `deferrevrec` | checkbox | Hold Revenue Recognition |  |
| `exchangerate` | currency2 | Exchange Rate |  |
| `formulacurrency` | currency | Formula (Currency) |  |
| `formuladate` | date | Formula (Date) |  |
| `formuladatetime` | datetime | Formula (Date/Time) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulapercent` | percent | Formula (Percent) |  |
| `formulatext` | text | Formula (Text) |  |
| `fxamount` | currency | Amount (Foreign Currency) |  |
| `grossamount` | currency | Amount (Gross) |  |
| `internalid` | select | Internal ID |  |
| `lastmodifieddate` | datetime | Last Modified |  |
| `lastviewed` | datetime | Last Viewed |  |
| `multisubsidiary` | checkbox | Intercompany |  |
| `netamount` | currency | Amount (Net) |  |
| `posting` | checkbox | Posting |  |
| `quantityrevcommitted` | float | Quantity Rev. Committed |  |
| `recognizedrevenue` | currency | Amount (Recognized) |  |
| `revcommitstatus` | text | Rev. Commit. Status |  |
| `revenuestatus` | text | Revenue Status |  |
| `revrecenddate` | date | Rev. Rec. End Date |  |
| `revreconrevcommitment` | checkbox | Rev. Rec. On Rev. Committment |  |
| `revrecstartdate` | date | Rev. Rec. Start Date |  |
| `subsidiary` | select | Subsidiary |  |
| `tranisvsoebundle` | checkbox | Transaction Is VSOE Bundle |  |
| `vsoeallocation` | currency | VSOE Allocation |  |

## Search joins

| Join ID | Label | Joined record |
|---|---|---|
| `account` | Account | Account |
| `amortizationschedule` | Amortization Schedule | Amortization Schedules |
| `transaction` | Transaction | Transaction |
