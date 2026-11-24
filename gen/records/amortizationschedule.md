# Amortization Schedule (`amortizationschedule`)
<!-- id: record:amortizationschedule -->

| Fact | Value |
|---|---|
| Internal ID | `amortizationschedule` |
| Category | list |
| Client scripts | no |
| Server scripts | yes |
| Tags | `custom_fields_supported`, `no_copy`, `no_create`, `no_delete` |

## Record facts

- `script_types`: `server`
- `not_supported`: `copy`, `create`, `delete`, `transform`
- `requires_feature`: `Amortization` [`real_name`]
- `user_events_supported`: `false`
- `created_by`: `system_from_amortization_templates`
- `script_functions`: `read`, `search`, `edit`
- `not_supported`: `copy`, `create`, `delete`, `transform`
- `requires_preference`: `allow_users_to_modify_amortization_schedules` [`edit`]
- `cannot`: `search_by_external_id`
- `configured_by`: `enable_features_accounting_subtab` [`amortization`]
- `ui_location`: `lists_accounting_amortization_schedules`
- `cannot`: `use_external_id_in_search_columns`
- `comparable_to` (refs `record:revrecschedule`) [`reuse_revenue_schedule_integrations`]

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `accountingbook` | select | Accounting Book | no | no |  | `purpose`: `accounting_book`; `allowed_values`: `primary_book`, `accessible_secondary_books` |
| `amortizationtype` | select | Type | no | no |  | `purpose`: `amortization_template_type`; `options`: `Standard`, `Variable`; `option_meanings`: `variable:percent_complete_by_job_completion`; `cannot`: `set_method_term_period_offsets` (when `variable_type`) |
| `enddate` | date | End Date | no | no |  | `purpose`: `schedule_end_date`; `value_source`: `system_displayed` |
| `initialamount` | rate | Initial Amount | no | no |  | `purpose`: `first_period_amount_or_percent`; `effect`: `remainder_recognized_by_amortization_method`; `accepts_forms`: `percentage`, `amount` |
| `isinactive` | checkbox | Inactive | no | no |  | `purpose`: `inactivate_template`; `ui_effect`: `hidden_from_lists`; `cannot`: `create_schedules_from_inactive` |
| `job` | select | Project | no | no |  | `purpose`: `project_of_schedule`; `effect`: `completion_percent_drives_amounts`; `constraint`: `only_variable_schedules` |
| `name` | text | Name | yes | no |  | `purpose`: `template_name`; `displayed_on`: `item_record_template_dropdown` |
| `parentSched` | select | Template | no | no |  | `purpose`: `template_that_created_schedule`; `value_source`: `system_displayed` |
| `periodoffset` | integer | Period Offset | yes | no |  | `purpose`: `periods_to_postpone_start`; `value_kind`: `zero_based_period_offset`; `caveat`: `does_not_change_number_of_periods`; `value_examples`: `2_periods_of_30_days_waits_60_days`; `can`: `same_offset_on_lines_for_several_accounts_in_period` |
| `recogintervalsrc` | select | Term Source | no | no |  | `purpose`: `term_source`; `options`: `Transaction Date`, `Receipt Date` |
| `recurrencetype` | select | Method | no | no |  | `purpose`: `amortization_method`; `options`: `even_periods`, `prorate_first_last`, `exact_days`, `prorate_period_rate`, `Custom`; `option_meanings`: `even_periods:equal_not_prorated_by_days`, `prorate_first_last:prorate_first_and_final_by_days`, `exact_days:each_period_by_its_days`, `prorate_period_rate:proportional_period_amount`, `custom:uneven_periods_amounts_and_accounts` |
| `remainingdeferredbalance` | currency | Remaining Deferred Balance | no | no |  | `purpose`: `unrecognized_amount`; `value_source`: `system_displayed` |
| `residual` | rate | Residual | no | no |  | `purpose`: `residual_amount_not_amortized`; `caveat`: `transaction_residual_overrides_item_residual` |
| `revrecoffset` | integer | Start Offset | yes | no |  | `purpose`: `periods_to_postpone_recognition`; `effect`: `adds_periods_keeps_final_period` |
| `schedulenumber` | text | Number | no | no |  | `purpose`: `schedule_number`; `value_source`: `auto_generated` (when `auto_generated_numbering`); `effect`: `used_as_name_when_name_blank` |
| `sourcetran` | select | Created From | no | no |  | `purpose`: `source_transaction_type`; `value_source`: `system_displayed` |
| `startdate` | date | Start Date | no | no |  | `purpose`: `expense_recognition_start_date`; `value_source`: `bill_recognition_start_date`; `default_behavior`: `bill_posting_date` (when `no_recognition_start_date`) |
| `status` | select | Status | no | no |  | `purpose`: `amortization_status`; `allowed_values`: `Not Started`, `In Progress`, `Complete`; `option_meanings`: `not_started:nothing_recognized`, `in_progress:partly_recognized`, `complete:all_recognized` |
| `totalamortized` | currency | Total Amortized | no | no |  | `purpose`: `amount_recognized_to_date`; `value_source`: `system_displayed` |
| `totalamount` | currency2 | Amount | no | no |  | `purpose`: `total_amortization_amount`; `editable_only_when`: `allow_users_to_modify_amortization_amount`; `effect`: `lines_rescaled_proportionally_on_change`; `constraint`: `only_unrecognized_amounts_change`; `constraint`: `total_equals_purchase_amount` |
| `externalid` | text | ExternalId | no | no |  |  |

## Sublist `recurrence` (Custom Recurrence Pattern)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `defrevaccount` | select | Account | no |  |  |
| `id` | integer |  | no |  |  |
| `incomeaccount` | text |  | no |  |  |
| `isrecognized` | checkbox | Is Recognized | no |  |  |
| `jdate` | date | Date Executed | no |  |  |
| `journal` | text | Journal | no |  |  |
| `periodoffset` | integer | Period Offset | yes |  |  |
| `postingperiod` | text |  | no |  |  |
| `recamount` | rate | Amount | yes |  |  |
| `recurdate` | date | Date | no |  |  |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `accountingbook` | select | Accounting Book |  |
| `amorstatus` | select | Status |  |
| `amortizedamount` | currency | Total Amortized |  |
| `amortype` | select | Type |  |
| `amount` | currency | Amount |  |
| `currencyname` | select | Schedule Currency |  |
| `deferredamount` | currency | Remaining Deferred Balance |  |
| `destacct` | select | Destination Account |  |
| `formuladate` | date | Formula (Date) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulatext` | text | Formula (Text) |  |
| `initialamt` | currency | Initial Amount |  |
| `internalid` | select | Internal ID |  |
| `internalidnumber` | integer | Internal ID (Number) |  |
| `isrecognized` | checkbox | Is Recognized? |  |
| `name` | text | Schedule Name |  |
| `pctcomplete` | float | % Complete |  |
| `pctrecognition` | float | % Recog. |  |
| `periodoffset` | integer | Period Offset |  |
| `postperiod` | select | Posting Period |  |
| `residual` | currency | Residual |  |
| `scheddate` | date | Date |  |
| `schedulenumber` | integer | Number |  |
| `schedulenumbertext` | text | Number/ID |  |
| `sourceacct` | select | Source Account |  |
| `srctranpostperiod` | select | Source Transaction Posting Period |  |
| `srctrantype` | select | Transaction Type |  |
| `startoffset` | integer | Start Offset |  |
| `templatename` | text | Template Name |  |
| `useforeignamounts` | checkbox | Use Transaction Currency |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `accountingbook` | select | Accounting Book |  |
| `amorstatus` | text | Status |  |
| `amortemplate` | select | Template Name |  |
| `amortizedamount` | currency | Total Amortized |  |
| `amortype` | text | Type |  |
| `amount` | currency | Amount (Schedule Total) |  |
| `currency` | select | Schedule Currency |  |
| `deferredamount` | currency | Remaining Deferred Balance |  |
| `destacct` | select | Destination Account |  |
| `formulacurrency` | currency | Formula (Currency) |  |
| `formuladate` | date | Formula (Date) |  |
| `formuladatetime` | datetime | Formula (Date/Time) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulapercent` | percent | Formula (Percent) |  |
| `formulatext` | text | Formula (Text) |  |
| `initialamt` | currency | Initial Amount |  |
| `internalid` | select | Internal ID |  |
| `isrecognized` | checkbox | Is Recognized? |  |
| `name` | text | Schedule Name |  |
| `pctcomplete` | percent | % Complete |  |
| `pctrecognition` | percent | % Recog. |  |
| `periodoffset` | integer | Period Offset |  |
| `recuramount` | currency | Amount |  |
| `recurfxamount` | currency | Amount (Foreign Currency) |  |
| `residual` | currency | Residual |  |
| `scheddate` | date | Date |  |
| `schedulenumber` | text | Number |  |
| `sourceacct` | select | Source Account |  |
| `srctran` | select | Source Transaction |  |
| `srctranpostperiod` | select | Posting Period |  |
| `startoffset` | integer | Start Offset |  |
| `useforeignamounts` | checkbox | Use Transaction Currency |  |

## Search joins

| Join ID | Label | Joined record |
|---|---|---|
| `user` | User | Employee |
| `vendor` | Vendor | Vendor |
