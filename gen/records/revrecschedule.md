# Revenue Recognition Schedule (`revrecschedule`)
<!-- id: record:revrecschedule -->

| Fact | Value |
|---|---|
| Internal ID | `revrecschedule` |
| Category | list |
| Client scripts | no |
| Server scripts | yes |
| User event scripts | no |
| Tags | `custom_fields_supported`, `no_copy`, `no_create`, `no_delete` |

## Record facts

- `script_types`: `server`
- `not_supported`: `create`, `copy`, `delete`, `transform`
- `script_functions`: `read`, `update`, `search`
- `created_by`: `system`
- `user_events_supported`: `false`
- `purpose`: `periods_and_amounts_of_item_revenue`
- `effect`: `generated_on_save_approve_or_bill_per_settings`
- `requires_feature`: `Revenue Recognition` [`real_name`]

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `amortizationperiod` | integer | Recognition Period | no | no |  | `purpose`: `recognition_periods`; `value_examples`: `12_runs_12_periods`; `caveat`: `transaction_start_and_end_dates_override`; `effect`: `missing_date_filled_from_period_count` |
| `amortizationtype` | select | Type | no | no |  | `purpose`: `amortization_template_type`; `options`: `Standard`, `Variable`; `option_meanings`: `variable:percent_complete_by_job_completion`; `cannot`: `set_method_term_period_offsets` (when `variable_type`) |
| `initialamount` | rate | Initial Amount | no | no |  | `purpose`: `first_period_amount_or_percent`; `effect`: `remainder_recognized_by_amortization_method`; `accepts_forms`: `percentage`, `amount` |
| `isinactive` | checkbox | Inactive | no | no |  | `purpose`: `inactivate_template_or_schedule` |
| `ispublic` | checkbox | Public | no | no |  | `purpose`: `public_template` |
| `job` | select | Project | no | no |  | `purpose`: `project`; `effect`: `completion_percent_drives_revenue`; `constraint`: `only_variable_schedules` |
| `name` | text | Name | yes | no |  | `purpose`: `template_or_schedule_name` |
| `periodoffset` | integer | Period Offset | yes | no |  | `purpose`: `periods_to_postpone_recognition`; `value_examples`: `2_periods_of_30_days_waits_60_days`; `use_when`: `trial_or_probation_services`; `caveat`: `does_not_change_number_of_periods` |
| `recogintervalsrc` | select | Term Source | no | no |  | `purpose`: `recognition_term_source`; `options`: `rev_rec_dates_specified_on_transaction`, `rev_rec_dates_specified_on_sales_order`, `billing_schedule_transaction_date_on_sales_order`, `billing_schedule_rev_rec_date_on_sales_order`; `requires_feature`: `Sales Order` (when `sales_order_options`) [`real_name`] |
| `recurrencetype` | select | Method | no | no |  | `purpose`: `recognition_method`; `options`: `even_periods`, `prorate_first_last`, `exact_days`, `prorate_period_rate`, `Custom` |
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
| `accountingbook` | select | Accounting Book |  |
| `amorstatus` | select | Status |  |
| `amortizedamount` | currency | Total Recognized |  |
| `amortype` | select | Type |  |
| `amount` | currency | Amount |  |
| `currency` | select | Schedule Currency |  |
| `deferredamount` | currency | Remaining Deferred Balance |  |
| `destacct` | select | Destination Account |  |
| `initialamt` | currency | Initial Amount |  |
| `internalid` | select | Internal ID |  |
| `internalidnumber` | integer | Internal ID (Number) |  |
| `isrecognized` | checkbox | Is Recognized? |  |
| `jedoc` | select | Journal |  |
| `name` | text | Schedule Name |  |
| `pctcomplete` | float | % Complete |  |
| `pctrecognition` | float | % Recog. |  |
| `periodoffset` | integer | Period Offset |  |
| `postperiod` | select | Posting Period |  |
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
| `amortemplate` | text | Template Name |  |
| `amortizedamount` | currency | Total Recognized |  |
| `amortype` | text | Type |  |
| `amount` | currency | Amount (Schedule Total) |  |
| `currency` | text | Schedule Currency |  |
| `deferredamount` | currency | Remaining Deferred Balance |  |
| `destacct` | text | Destination Account |  |
| `initialamt` | currency | Initial Amount |  |
| `internalid` | select | Internal ID |  |
| `isrecognized` | checkbox | Is Recognized? |  |
| `jedoc` | text | Journal |  |
| `linesequencenumber` | select | Line Sequence Number |  |
| `name` | text | Schedule Name |  |
| `pctcomplete` | percent | % Complete |  |
| `pctrecognition` | percent | % Recog. |  |
| `periodoffset` | integer | Period Offset |  |
| `recuramount` | currency | Amount |  |
| `recurfxamount` | currency | Amount (Foreign Currency) |  |
| `scheddate` | date | Date |  |
| `schedulenumber` | text | Number |  |
| `sourceacct` | text | Source Account |  |
| `srcdocline` | text | Source Transaction Line |  |
| `srctran` | text | Source Transaction |  |
| `srctranpostperiod` | text | Posting Period |  |
| `startoffset` | integer | Start Offset |  |
| `useforeignamounts` | checkbox | Use Transaction Currency |  |

## Search joins

| Join ID | Label | Joined record |
|---|---|---|
| `appliedtotransaction` | Applied To Transaction | Transaction |
| `customer` | Customer | Customer |
| `item` | Item | Item |
| `user` | User | Employee |
