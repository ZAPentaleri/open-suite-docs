# Revenue Recognition Template (`revrectemplate`)
<!-- id: record:revrectemplate -->

| Fact | Value |
|---|---|
| Internal ID | `revrectemplate` |
| Category | list |
| Client scripts | no |
| Server scripts | yes |
| Searchable | yes |
| User event scripts | no |
| Tags | `custom_fields_supported`, `scripting_full` |

## Record facts

- `script_types`: `server`
- `user_events_supported`: `false`
- `purpose`: `how_item_revenue_posts`
- `contains`: `terms`, `recognition_period`, `start_offset`, `initial_amount`
- `requires_feature`: `Revenue Recognition` [`real_name`]
- `script_functions`: `create`, `update`, `copy`, `delete`, `search`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `amortizationperiod` | integer | Recognition Period | no | no |  | `purpose`: `recognition_periods`; `value_examples`: `12_runs_12_periods`; `caveat`: `transaction_start_and_end_dates_override`; `effect`: `missing_date_filled_from_period_count` |
| `amortizationtype` | select | Type | no | no |  | `purpose`: `amortization_template_type`; `options`: `Standard`, `Variable`; `cannot`: `set_method_term_period_offsets` (when `variable_type`); `option_meanings`: `standard:method_and_term_source_entered`, `variable:percent_complete_by_job_completion`; `rule`: `variable_templates_selected_on_item_records` |
| `initialamount` | rate | Initial Amount | no | no |  | `purpose`: `first_period_amount_or_percent`; `effect`: `remainder_recognized_by_amortization_method`; `accepts_forms`: `percentage`, `amount` |
| `isinactive` | checkbox | Inactive | no | no |  | `purpose`: `inactivate_template_or_schedule` |
| `ispublic` | checkbox | Public | no | no |  | `purpose`: `public_template` |
| `job` | select | Project | no | no |  | `purpose`: `project`; `effect`: `completion_percent_drives_revenue`; `constraint`: `only_variable_schedules` |
| `name` | text | Name | yes | no |  | `purpose`: `template_or_schedule_name` |
| `periodoffset` | integer | Period Offset | yes | no |  | `purpose`: `periods_to_postpone_recognition`; `value_examples`: `2_periods_of_30_days_waits_60_days`; `use_when`: `trial_or_probation_services`; `caveat`: `does_not_change_number_of_periods` |
| `recogintervalsrc` | select | Term Source | no | no |  | `purpose`: `recognition_term_source`; `options`: `rev_rec_dates_specified_on_transaction`, `rev_rec_dates_specified_on_sales_order`, `billing_schedule_transaction_date_on_sales_order`, `billing_schedule_rev_rec_date_on_sales_order`; `requires_feature`: `Sales Order` (when `sales_order_options`) [`real_name`]; `option_meanings`: `on_transaction:invoice_or_cash_sale_rev_rec_dates`, `on_sales_order:sales_order_rev_rec_dates`, `billing_schedule_tran_date:sales_order_transaction_date`, `billing_schedule_rev_rec_date:sales_order_rev_rec_dates` |
| `recurrencetype` | select | Method | no | no |  | `purpose`: `recognition_method`; `options`: `even_periods`, `prorate_first_last`, `exact_days`, `prorate_period_rate`, `Custom`; `option_meanings`: `even_periods:equal_per_period_no_day_proration`, `prorate_first_last:middle_equal_first_last_prorated_by_days`, `exact_days:each_period_by_its_day_count`, `prorate_period_rate:full_period_count_proportional_amounts`, `Custom:uneven_periods_amounts_multiple_income_accounts` |
| `revrecoffset` | integer | Start Offset | yes | no |  | `purpose`: `start_offset`; `effect`: `adds_periods_keeps_final_period` |
| `totalamount` | currency2 | Amount | no | no |  | `purpose`: `total_recognition_amount`; `editable_only_when`: `allow_users_to_modify_revrec_schedule`; `effect`: `lines_rescaled_proportionally_on_change`; `constraint`: `only_unrecognized_amounts_change`; `constraint`: `total_equals_line_sales_or_vsoe_amount` |
| `externalid` | text | ExternalId | no | no |  |  |

## Sublist `recurrence` (Custom Recurrence Pattern)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `incomeaccount` | select | Account | no |  |  |
| `periodoffset` | integer | Period Offset | yes |  |  |
| `postingperiod` | text |  | no |  |  |
| `recamount` | rate | Amount | yes |  |  |

## Tabs

| Tab ID | Label |
|---|---|
| `history` | System Notes |
| `meta` | Recurrence |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `amormethod` | select | Method |  |
| `amorperiod` | integer | Amortization Period |  |
| `amorstartoffset` | integer | Start Offset |  |
| `amortermsrc` | select | Term Source |  |
| `amortype` | select | Type |  |
| `contraaccount` | select | Contra Account |  |
| `deferralaccount` | select | Deferral Account |  |
| `externalid` | select | External ID |  |
| `externalidstring` | text | External ID (Text) |  |
| `formuladate` | date | Formula (Date) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulatext` | text | Formula (Text) |  |
| `internalid` | select | Internal ID |  |
| `internalidnumber` | integer | Internal ID (Number) |  |
| `isinactive` | checkbox | Inactive |  |
| `name` | text | Name |  |
| `periodoffset` | integer | Period Offset |  |
| `targetaccount` | select | Target Account |  |
| `useforeignamounts` | checkbox | Use Transaction Currency |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `amorinactive` | text | Inactive |  |
| `amormethod` | text | Method |  |
| `amorperiod` | text | Amortization Period |  |
| `amorstartoffset` | text | Start Offset |  |
| `amortermsrc` | text | Term Source |  |
| `amortype` | text | Type |  |
| `contraaccount` | text | Contra Account |  |
| `deferralaccount` | text | Deferral Account |  |
| `externalid` | select | External ID |  |
| `formulacurrency` | currency | Formula (Currency) |  |
| `formuladate` | date | Formula (Date) |  |
| `formuladatetime` | datetime | Formula (Date/Time) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulapercent` | percent | Formula (Percent) |  |
| `formulatext` | text | Formula (Text) |  |
| `internalid` | select | Internal ID |  |
| `name` | text | Name |  |
| `periodoffset` | text | Period Offset |  |
| `targetaccount` | text | Target Account |  |
| `useforeignamounts` | checkbox | Use Transaction Currency |  |
