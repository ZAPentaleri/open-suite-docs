# Amortization Template (`amortizationtemplate`)
<!-- id: record:amortizationtemplate -->

| Fact | Value |
|---|---|
| Internal ID | `amortizationtemplate` |
| Category | list |
| Client scripts | no |
| Server scripts | yes |
| Searchable | yes |
| Tags | `custom_fields_supported`, `scripting_full` |

## Record facts

- `script_functions`: `create`, `update`, `copy`, `delete`, `search`
- `requires_feature`: `Amortization` [`real_name`]
- `script_types`: `server`
- `user_events_supported`: `false`
- `rule`: `custom_method_requires_recurrence_line_and_blocks_delete`
- `cannot`: `search_by_external_id`
- `purpose`: `terms_for_system_created_amortization_schedules`
- `effect`: `schedule_created_on_save_of_line_with_template`
- `applies_to`: `journal_entry`, `vendor_bill`, `vendor_credit`
- `configured_by`: `enable_features_accounting_subtab` [`amortization`]
- `ui_location`: `lists_accounting_amortization_templates_new`
- `cannot`: `use_external_id_in_search_columns`
- `comparable_to` (refs `record:revrectemplate`) [`reuse_revenue_template_integrations`]

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `acctcontra` | select | Contra Account | no | no |  | `purpose`: `accumulated_amortization_account`; `use_when`: `depreciation_schedules` |
| `acctdeferral` | select | Deferral Account | no | no |  | `purpose`: `deferred_expense_account`; `special_values`: `Default`; `option_meanings`: `default:line_deferred_expense_account`, `gl_account:overrides_line_account` |
| `accttarget` | select | Target Account | no | no |  | `purpose`: `amortization_target_account`; `special_values`: `Default`; `option_meanings`: `default:bill_or_item_expense_account`, `gl_account:overrides_line_account` |
| `amortizationperiod` | integer | Amortization Period | no | no |  | `purpose`: `number_of_amortization_periods`; `value_source`: `bill_amortization_start_date` [`start`]; `default_behavior`: `bill_posting_date_starts` (when `no_start_date`); `not_required_when`: `bill_has_amortization_end_date`; `value_examples`: `60_amortizes_over_60_periods` |
| `amortizationtype` | select | Type | no | no |  | `purpose`: `amortization_template_type`; `options`: `Standard`, `Variable`; `option_meanings`: `variable:percent_complete_by_job_completion`; `cannot`: `set_method_term_period_offsets` (when `variable_type`) |
| `initialamount` | rate | Initial Amount | no | no |  | `purpose`: `first_period_amount_or_percent`; `effect`: `remainder_recognized_by_amortization_method`; `accepts_forms`: `percentage`, `amount` |
| `isinactive` | checkbox | Inactive | no | no |  | `purpose`: `inactivate_template`; `ui_effect`: `hidden_from_lists`; `cannot`: `create_schedules_from_inactive` |
| `name` | text | Name | yes | no |  | `purpose`: `template_name`; `displayed_on`: `item_record_template_dropdown` |
| `periodoffset` | integer | Period Offset | yes | no |  | `purpose`: `periods_to_postpone_start`; `value_kind`: `zero_based_period_offset`; `caveat`: `does_not_change_number_of_periods`; `value_examples`: `2_periods_of_30_days_waits_60_days`; `can`: `same_offset_on_lines_for_several_accounts_in_period` |
| `recogintervalsrc` | select | Term Source | no | no |  | `purpose`: `term_source`; `options`: `Transaction Date`, `Receipt Date` |
| `recurrencetype` | select | Method | no | no |  | `purpose`: `amortization_method`; `options`: `even_periods`, `prorate_first_last`, `exact_days`, `prorate_period_rate`, `Custom`; `option_meanings`: `even_periods:equal_not_prorated_by_days`, `prorate_first_last:prorate_first_and_final_by_days`, `exact_days:each_period_by_its_days`, `prorate_period_rate:proportional_period_amount`, `custom:uneven_periods_amounts_and_accounts` |
| `residual` | rate | Residual | no | no |  | `purpose`: `residual_amount_not_amortized`; `caveat`: `transaction_residual_overrides_item_residual` |
| `revrecoffset` | integer | Start Offset | yes | no |  | `purpose`: `periods_to_postpone_recognition`; `effect`: `adds_periods_keeps_final_period` |
| `totalamount` | currency2 | Amount | no | no |  | `purpose`: `total_amortization_amount`; `editable_only_when`: `allow_users_to_modify_amortization_amount`; `effect`: `lines_rescaled_proportionally_on_change`; `constraint`: `only_unrecognized_amounts_change`; `constraint`: `total_equals_purchase_amount` |
| `externalid` | text | ExternalId | no | no |  |  |

## Sublist `recurrence` (Custom Recurrence Pattern)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `defrevaccount` | select | Account | no |  |  |
| `incomeaccount` | text |  | no |  |  |
| `periodoffset` | integer | Period Offset | yes |  |  |
| `postingperiod` | text |  | no |  |  |
| `recamount` | rate | Amount | yes |  |  |

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
| `formuladate` | date | Formula (Date) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulatext` | text | Formula (Text) |  |
| `internalid` | select | Internal ID |  |
| `internalidnumber` | integer | Internal ID (Number) |  |
| `isinactive` | checkbox | Inactive |  |
| `name` | select | Name |  |
| `periodoffset` | integer | Period Offset |  |
| `targetaccount` | select | Target Account |  |
| `useforeignamounts` | checkbox | Use Transaction Currency |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `amormethod` | text | Method |  |
| `amorperiod` | text | Amortization Period |  |
| `amorstartoffset` | text | Start Offset |  |
| `amortermsrc` | text | Term Source |  |
| `amortype` | text | Type |  |
| `contraaccount` | text | Contra Account |  |
| `deferralaccount` | text | Deferral Account |  |
| `formulacurrency` | currency | Formula (Currency) |  |
| `formuladate` | date | Formula (Date) |  |
| `formuladatetime` | datetime | Formula (Date/Time) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulapercent` | percent | Formula (Percent) |  |
| `formulatext` | text | Formula (Text) |  |
| `internalid` | select | Internal ID |  |
| `isinactive` | text | Inactive |  |
| `name` | text | Name |  |
| `periodoffset` | text | Period Offset |  |
| `targetaccount` | text | Target Account |  |
| `useforeignamounts` | checkbox | Use Transaction Currency |  |

## Search joins

| Join ID | Label | Joined record |
|---|---|---|
| `user` | User | Employee |
