# Expense Amortization Plan (`expenseplan`)
<!-- id: record:expenseplan -->

| Fact | Value |
|---|---|
| Internal ID | `expenseplan` |

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `catchupperiod` | select | Catch Up Period | no | no |  | `purpose`: `catch_up_period`; `default_behavior`: `earliest_open_period` (when `hold_removed`); `allowed_values`: `open_periods`; `caveat`: `ap_locked_periods_generally_excluded` |
| `holdrevenuerecognition` | checkbox | Hold Expense Amortization | no | no |  | `purpose`: `hold_amortization`; `true_means`: `status_on_hold_no_journal_entries`; `effect`: `held_amounts_recognized_in_catch_up_period` |
| `residual` | currency2 | Residual | no | no |  | `purpose`: `residual_amount_not_amortized`; `value_kind`: `amount_or_percentage`; `value_examples`: `fixed_asset_salvage_value` |
| `amount` | currency | Amount | no | no |  |  |
| `amountsource` | text | Amount Source | no | no |  |  |
| `comments` | text | Comments | no | no |  |  |
| `createdfrom` | select | Created From | no | no |  |  |
| `creationtriggeredby` | select | Creation Triggered By | no | no |  |  |
| `customform` | select | Custom Form | no | no |  |  |
| `enddatechangeimpact` | text | End Date Change Impact | no | no |  |  |
| `externalid` | text | ExternalId | no | no |  |  |
| `initialamount` | currency | Initial Amount | no | no |  |  |
| `iseliminate` | checkbox | Eliminate | no | no |  |  |
| `item` | select | Item | no | no |  |  |
| `periodoffset` | text | Period Offset | no | no |  |  |
| `recalcadjustperiodoffset` | integer | Recalculation Adjustment Period Offset | no | no |  |  |
| `recognitionmethod` | text | Recognition Method | no | no |  |  |
| `recognitionperiod` | integer | Recognition Period | no | no |  |  |
| `recordnumber` | text | Number | no | no |  |  |
| `reforecastmethod` | select | Reforecast Method | no | no |  |  |
| `relatedrevenuearrangement` | select | Related Revenue Arrangement | no | no |  |  |
| `relatedrevenueelement` | select | Related Revenue Element | no | no |  |  |
| `remainingdeferredbalance` | currency | Remaining Deferred Balance | no | no |  |  |
| `revenueplancurrency` | text | Expense Plan Currency | no | no |  |  |
| `revenueplantype` | select | Expense Plan Type | no | no |  |  |
| `revenuerecognitionrule` | select | Expense Amortization Rule | no | no |  |  |
| `revenueschedulekey` | text | None | no | no |  |  |
| `revrecenddate` | date | Amortization End Date | yes | no |  |  |
| `revrecenddatesource` | text | Amortization End Date Source | no | no |  |  |
| `revrecstartdate` | date | Amortization Start Date | yes | no |  |  |
| `revrecstartdatesource` | text | Amortization Start Date Source | no | no |  |  |
| `startoffset` | text | Start Offset | no | no |  |  |
| `status` | select | Status | no | no |  |  |
| `termindays` | integer | Expense Term In Days | no | no |  |  |
| `terminmonths` | integer | Expense Term In Months | no | no |  |  |
| `totalrecognized` | currency | Total Amortized | no | no |  |  |
| `tranlineuniquekey` | text | None | no | no |  |  |
| `transactionbook` | text | None | no | no |  |  |
| `transactiondoc` | text | None | no | no |  |  |
| `transactionline` | text | None | no | no |  |  |

## Sublist `plannedexpense` (Planned Expense)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `amount` | currency | Amount | yes |  |  |
| `dateexecuted` | date | Date Executed | no |  |  |
| `deferredrevenueaccount` | select | Source Account | no |  |  |
| `isrecognized` | checkbox | Is Amortized | no |  |  |
| `journal` | text | Journal | no |  |  |
| `percentrecognizedinperiod` | percent | % Amortized in Period | no |  |  |
| `percenttotalrecognized` | percent | % Total Amortized | no |  |  |
| `periodcomments` | text | Period Comments | no |  |  |
| `plannedperiod` | select | Planned Period | yes |  |  |
| `plannedrevenue` | text | Planned Expense | no |  |  |
| `plannedrevenuetype` | text | Planned Expense Type | no |  |  |
| `postingperiod` | text | Posting Period | no |  |  |
| `recognitionaccount` | select | Expense Account | no |  |  |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `accountingbook` | select | Accounting Book |  |
| `amount` | currency | Amount |  |
| `amountsource` | select | Amortization Amount Source Type |  |
| `catchupperiod` | select | Catch Up Period |  |
| `comments` | text | Comments |  |
| `createdfrom` | select | Created From |  |
| `creationtriggeredby` | select | Creation Triggered By |  |
| `datecreated` | datetime | Date Created |  |
| `dateexecuted` | date | Date Executed |  |
| `deferralaccount` | select | Deferred Expense Account |  |
| `enddatechangeimpact` | select | Amortization End Date Change Impact |  |
| `exchangerate` | currency | Exchange Rate |  |
| `externalid` | select | External ID |  |
| `externalidstring` | text | External ID (Text) |  |
| `holdrevenuerecognition` | checkbox | Hold Expense Amortization |  |
| `initialamount` | currency | Initial Amount |  |
| `internalid` | select | Internal ID |  |
| `internalidnumber` | integer | Internal ID (Number) |  |
| `iseliminate` | checkbox | Eliminate |  |
| `isrecognized` | checkbox | Is Recognized |  |
| `item` | select | Item |  |
| `journal` | select | Journal |  |
| `lastmodifieddate` | datetime | Date Last Modified |  |
| `lineamount` | currency | Amount (Line Level) |  |
| `linetotalrecognized` | currency | Total Recognized (Line Level) |  |
| `percentrecognizedinperiod` | percent | % Recognized in Period |  |
| `percenttotalrecognized` | percent | % Total Recognized |  |
| `periodcomments` | text | Period Comments |  |
| `periodoffset` | integer | Period Offset |  |
| `plannedperiod` | select | Planned Period |  |
| `postingperiod` | select | Posting Period |  |
| `recalcadjustperiodoffset` | integer | Recalculation Adjustment Period Offset |  |
| `recognitionaccount` | select | Expense Account |  |
| `recognitionmethod` | select | Recognition Method |  |
| `recognitionperiod` | integer | Recognition Period |  |
| `recordnumber` | text | Number |  |
| `reforecastmethod` | select | Reforecast Method |  |
| `relatedrevenuearrangement` | select | Related Revenue Arrangement |  |
| `relatedrevenueelement` | select | Related Revenue Element |  |
| `remainingdeferredbalance` | currency | Remaining Deferred Balance |  |
| `residual` | currency | Residual |  |
| `revenueplancurrency` | text | Expense Plan Currency |  |
| `revenueplantype` | select | Expense Plan Type |  |
| `revenuerecognitionrule` | select | Expense Amortization Rule |  |
| `revrecenddate` | date | Amortization End Date |  |
| `revrecenddatesource` | select | Amortization End Date Source Type |  |
| `revrecstartdate` | date | Amortization Start Date |  |
| `revrecstartdatesource` | select | Amortization Start Date Source Type |  |
| `startoffset` | integer | Start Offset |  |
| `status` | select | Status |  |
| `termindays` | integer | Expense Term In Days |  |
| `terminmonths` | integer | Expense Term In Months |  |
| `totalrecognized` | currency | Total Recognized |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `accountingbook` | select | Accounting Book |  |
| `amount` | currency | Amount |  |
| `amountsource` | text | Amortization Amount Source Type |  |
| `catchupperiod` | select | Catch Up Period |  |
| `comments` | text | Comments |  |
| `createdfrom` | text | Created From |  |
| `creationtriggeredby` | text | Creation Triggered By |  |
| `datecreated` | datetime | Date Created |  |
| `dateexecuted` | date | Date Executed |  |
| `deferralaccount` | text | Deferred Expense Account |  |
| `enddatechangeimpact` | text | Amortization End Date Change Impact |  |
| `exchangerate` | currency | Exchange Rate |  |
| `externalid` | select | External ID |  |
| `holdrevenuerecognition` | checkbox | Hold Expense Amortization |  |
| `initialamount` | currency | Initial Amount |  |
| `internalid` | select | Internal ID |  |
| `iseliminate` | checkbox | Eliminate |  |
| `isrecognized` | checkbox | Is Recognized |  |
| `item` | select | Item |  |
| `journal` | text | Journal |  |
| `lastmodifieddate` | datetime | Last Modified |  |
| `lineamount` | currency | Amount (Line Level) |  |
| `linetotalrecognized` | currency | Total Recognized (Line Level) |  |
| `percentrecognizedinperiod` | percent | % Recognized in Period |  |
| `percenttotalrecognized` | percent | % Total Recognized |  |
| `periodcomments` | text | Period Comments |  |
| `periodoffset` | integer | Period Offset |  |
| `plannedperiod` | text | Planned Period |  |
| `postingperiod` | text | Posting Period |  |
| `recalcadjustperiodoffset` | integer | Recalculation Adjustment Period Offset |  |
| `recognitionaccount` | text | Expense Account |  |
| `recognitionmethod` | text | Recognition Method |  |
| `recognitionperiod` | integer | Recognition Period |  |
| `recordnumber` | text | Number |  |
| `reforecastmethod` | select | Reforecast Method |  |
| `relatedrevenuearrangement` | select | Related Revenue Arrangement |  |
| `relatedrevenueelement` | select | Related Revenue Element |  |
| `remainingdeferredbalance` | currency | Remaining Deferred Balance |  |
| `residual` | currency | Residual |  |
| `revenueplancurrency` | text | Expense Plan Currency |  |
| `revenueplantype` | select | Expense Plan Type |  |
| `revenuerecognitionrule` | select | Expense Amortization Rule |  |
| `revrecenddate` | date | Amortization End Date |  |
| `revrecenddatesource` | text | Amortization End Date Source Type |  |
| `revrecstartdate` | date | Amortization Start Date |  |
| `revrecstartdatesource` | text | Amortization Start Date Source Type |  |
| `startoffset` | integer | Start Offset |  |
| `status` | text | Status |  |
| `termindays` | integer | Expense Term In Days |  |
| `terminmonths` | integer | Expense Term In Months |  |
| `totalrecognized` | currency | Total Recognized |  |

## Search joins

| Join ID | Label | Joined record |
|---|---|---|
| `customer` | Customer | Customer |
| `expenseamortizationrule` | Expense Amortization Rule | Expense Amortization Rule |
| `transaction` | Transaction | Transaction |
| `user` | User | Employee |
