# Revenue Recognition Plan (`revenueplan`)
<!-- id: record:revenueplan -->

| Fact | Value |
|---|---|
| Internal ID | `revenueplan` |
| Tags | `custom_fields_supported` |

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `amount` | currency | Amount | no | no |  | `purpose`: `total_revenue_of_plan` |
| `amountsource` | text | Amount Source | no | no |  | `purpose`: `amount_source`; `value_source`: `revenue_recognition_rule`; `effect`: `with_create_revenue_plans_on_derives_amount` |
| `catchupperiod` | select | Catch Up Period | no | no |  | `purpose`: `catch_up_period`; `default_behavior`: `earliest_open_period` (when `hold_removed`); `allowed_values`: `open_periods` |
| `comments` | text | Comments | no | no |  | `purpose`: `comments` |
| `createdfrom` | select | Created From | no | no |  | `purpose`: `source_revenue_element`; `value_source`: `system_displayed` |
| `enddatechangeimpact` | text | End Date Change Impact | no | no |  | `purpose`: `end_date_change_impact`; `value_source`: `revenue_recognition_rule`; `options`: `Update Remaining Periods Only`, `Update All Periods`; `option_meanings`: `remaining:unrecognized_spread_over_remaining`, `all:respread_with_current_period_catch_up` |
| `exchangerate` | currency2 | Exchange Rate | no | no |  | `purpose`: `plan_exchange_rate`; `caveat`: `may_differ_after_element_merge` |
| `holdrevenuerecognition` | checkbox | Hold Revenue Recognition | no | no |  | `purpose`: `hold_revenue_recognition`; `true_means`: `status_on_hold_no_journal_entries`; `effect`: `held_revenue_recognized_in_catch_up_period` |
| `initialamount` | currency | Initial Amount | no | no |  | `purpose`: `first_period_amount`; `value_source`: `revenue_recognition_rule`; `accepts_forms`: `percentage`, `amount` |
| `item` | select | Item | no | no |  | `purpose`: `item`; `value_source`: `system_displayed` |
| `parentlinecurrency` | text | Parent Line Currency | no | no |  | `purpose`: `source_obligation_currency` |
| `periodoffset` | text | Period Offset | no | no |  | `purpose`: `period_offset`; `value_source`: `revenue_recognition_rule`; `effect`: `whole_recognition_moved_later` |
| `recognitionmethod` | text | Recognition Method | no | no |  | `purpose`: `recognition_method`; `value_source`: `revenue_recognition_rule` |
| `recognitionperiod` | integer | Recognition Period | no | no |  | `purpose`: `recognition_period_count`; `applies_when`: `end_date_source_recognition_period`; `caveat`: `partial_first_period_counted` |
| `recordnumber` | text | Number | no | no |  | `purpose`: `plan_number`; `configured_by`: `auto_generated_numbers_other_subtab` |
| `remainingdeferredbalance` | currency | Remaining Deferred Balance | no | no |  | `purpose`: `remaining_deferred_balance`; `value_source`: `amount_minus_total_recognized` |
| `revenueplancurrency` | text | Revenue Recognition Plan Currency | no | no |  | `purpose`: `plan_currency`; `value_source`: `subsidiary_base_currency`; `value_source`: `book_and_subsidiary_base_currency` (when `multi_book_accounting`) |
| `revenueplantype` | select | Revenue Plan Type | no | no |  | `purpose`: `plan_type`; `allowed_values`: `Forecast`, `Actual`; `option_meanings`: `forecast:forecast_reports_only`, `actual:controls_revenue_posting` |
| `revenuerecognitionrule` | select | Revenue Recognition Rule | no | no |  | `purpose`: `revenue_recognition_rule`; `editable_only_when`: `plan_status_not_started`; `caveat`: `rule_change_keeps_dates_changes_method` |
| `revrecenddate` | date | Rev Rec End Date | yes | no |  | `purpose`: `plan_end_date`; `effect`: `plan_adjusted_per_end_date_change_impact` |
| `revrecenddatesource` | text | Revenue Recognition End Date Source | no | no |  | `purpose`: `end_date_source`; `value_source`: `revenue_recognition_rule` |
| `revrecstartdate` | date | Rev Rec Start Date | yes | no |  | `purpose`: `plan_start_date`; `editable_only_when`: `no_revenue_recognized`; `effect`: `period_amounts_adjusted_end_date_kept` |
| `revrecstartdatesource` | text | Revenue Recognition Start Date Source | no | no |  | `purpose`: `start_date_source`; `value_source`: `revenue_recognition_rule` |
| `startoffset` | text | Start Offset | no | no |  | `purpose`: `start_offset`; `value_source`: `revenue_recognition_rule`; `effect`: `start_moved_end_may_stay` |
| `status` | select | Status | no | no |  | `purpose`: `plan_status`; `allowed_values`: `Not Started`, `In Progress`, `On Hold`, `Complete`; `option_meanings`: `on_hold:no_more_recognition_until_released` |
| `terminmonths` | integer | Revenue Term In Months | no | no |  | `purpose`: `term_in_months`; `applies_when`: `end_date_source_rev_term_in_months`; `caveat`: `mid_month_start_spans_extra_period` |
| `totalrecognized` | currency | Total Recognized | no | no |  | `purpose`: `total_recognized`; `value_source`: `system_displayed` |

## Sublist `plannedrevenue` (Planned Revenue)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `amount` | currency | Amount | yes |  |  |
| `dateexecuted` | date | Date Executed | no |  |  |
| `deferredrevenueaccount` | select | Deferral Account | no |  |  |
| `isrecognized` | checkbox | Is Recognized | no |  |  |
| `journal` | text | Journal | no |  |  |
| `percentrecognizedinperiod` | percent | % Recognized in Period | no |  |  |
| `percenttotalrecognized` | percent | % Total Recognized | no |  |  |
| `plannedperiod` | select | Planned Period | yes |  |  |
| `plannedrevenue` | text | Planned Revenue | no |  |  |
| `postingperiod` | text | Posting Period | no |  |  |
| `recognitionaccount` | select | Recognition Account | no |  |  |
