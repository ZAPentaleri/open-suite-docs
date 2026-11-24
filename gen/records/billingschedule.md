# Billing Schedule (`billingschedule`)
<!-- id: record:billingschedule -->

| Fact | Value |
|---|---|
| Internal ID | `billingschedule` |
| Category | list |
| Client scripts | no |
| Server scripts | yes |
| Searchable | yes |
| Tags | `custom_fields_supported`, `scripting_full` |

## Record facts

- `script_types`: `server`
- `script_functions`: `create`, `update`, `copy`, `delete`, `search`
- `requires_feature`: `Advanced Billing` [`real_name`]
- `user_events`: `beforeSubmit`, `afterSubmit`
- `schedule_types`: `charge_based`, `fixed_bid_interval`, `fixed_bid_milestone`, `standard`, `time_and_materials`
- `rule`: `schedule_type_set_via_initialize_values`
- `rule`: `fixed_bid_milestone_needs_project_and_project_update`
- `applies_to`: `sales_orders`, `sales_order_lines`, `projects`
- `configured_by`: `enable_features_transactions_subtab` [`advanced_billing`]
- `ui_location`: `lists_accounting_billing_schedules_new`
- `displayed_on`: `project_financial_subtab` (when `fixed_bid_milestone`)
- `available_only_when`: `standard_type_with_custom_frequency` [`recurrence_sublist`]
- `available_only_when`: `fixed_bid_milestone_type` [`milestone_sublist`]
- `caveat`: `project_link_not_created_automatically`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `applytosubtotal` | checkbox | Apply To Subtotal | no | no |  | `purpose`: `apply_to_pretax_prediscount_subtotal`; `requires`: `type_standard_and_frequency_custom` |
| `billforactuals` | checkbox | Invoice Actual Time Only | no | no |  | `purpose`: `bill_actual_time_only`; `true_means`: `invoice_only_tracked_time`; `false_means`: `invoice_planned_and_actual_time`; `cannot`: `enter_initial_amount` (when `checked`) |
| `frequency` | select | Recurrence Frequency | yes | no |  | `purpose`: `billing_frequency`; `options`: `Daily`, `Weekly`, `Monthly`, `Quarterly`, `Twice a Year`, `Annually`, `One Time`, `End of Period`, `Start of Period`, `Custom`; `comparable_to`: `monthly_with_repeat_every_3_or_6` |
| `inarrears` | checkbox | In Arrears | no | no |  | `purpose`: `bill_in_arrears`; `true_means`: `bill_at_period_end`; `false_means`: `first_invoice_initial_plus_first_recurring`; `value_examples`: `monthly_no_initial_first_invoice_one_month_after_order` |
| `initialamount` | ratehighprecision | Initial Amount | yes | no |  | `purpose`: `initial_bill_amount`; `accepts_forms`: `percentage`, `amount`; `constraint`: `percentage_only_for_transaction_level_schedules` |
| `initialterms` | select | Initial Payment Terms | no | no |  | `purpose`: `initial_payment_terms` |
| `isinactive` | checkbox | Inactive | no | no |  | `purpose`: `inactivate_billing_schedule`; `cannot`: `apply_inactive_schedule_to_orders` |
| `ispublic` | checkbox | Public | no | no |  | `purpose`: `public_billing_schedule`; `true_means`: `selectable_on_any_estimate_order_item`; `false_means`: `private_to_originating_transaction`; `shown_only_when`: `schedule_created_on_the_fly`; `default_value`: `true` (when `regular_schedule`); `cannot`: `change` (when `regular_schedule`) |
| `name` | text | Name | yes | no |  | `purpose`: `billing_schedule_name` |
| `numberremaining` | integer | Recurrence Count | yes | no |  | `purpose`: `number_of_subsequent_payments` |
| `recurrencepattern` | select | Recurrence Pattern | no | no |  | `purpose`: `recurrence_pattern`; `options`: `fixed_date`, `anniversary_date`; `value_examples`: `created_on_3rd_billed_on_15th_and_30th_is_fixed_date` |
| `recurrenceterms` | select | Recurrence Payment Terms | no | no |  | `purpose`: `terms_for_subsequent_invoices`; `allowed_values_listed_in`: `accounting_list_terms` |
| `repeatevery` | select | Repeat Every | no | no |  | `purpose`: `recurring_invoice_interval` |
| `scheduletype` | select | Type | no | no |  | `purpose`: `billing_schedule_type`; `options`: `Charge-Based`, `Fixed Bid, Interval`, `Standard`, `Time and Materials`; `requires_feature`: `Project Management` (when `fixed_bid_interval_type`) [`real_name`]; `caveat`: `available_types_depend_on_features`; `option_meanings`: `charge_based:bill_charges_from_charge_rules`, `fixed_bid_interval:bill_completed_work_at_intervals`, `standard:initial_amount_frequency_and_count`, `time_and_materials:bill_actual_project_expenses` |
| `transaction` | select | Transaction | no | no |  | `purpose`: `associated_transaction`; `value_source`: `system_displayed` |
| `externalid` | text | ExternalId | no | no |  |  |
| `project` | select | Project | no | no |  |  |
| `seriesstartdate` | date | Start Date | no | no |  |  |
| `weekfriday` | checkbox | Friday | no | no |  |  |
| `weekmonday` | checkbox | Monday | no | no |  |  |
| `weeksaturday` | checkbox | Saturday | no | no |  |  |
| `weeksunday` | checkbox | Sunday | no | no |  |  |
| `weekthursday` | checkbox | Thursday | no | no |  |  |
| `weektuesday` | checkbox | Tuesday | no | no |  |  |
| `weekwednesday` | checkbox | Wednesday | no | no |  |  |

## Sublist `milestone` (Milestones)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `comments` | text | Comment | no |  |  |
| `milestoneactualcompletiondate` | date | Actual Completion Date | no |  |  |
| `milestoneamount` | percent | Amount | yes |  |  |
| `milestonecompleted` | checkbox | Completed | no |  |  |
| `milestonedate` | date | Estimated Completion Date | yes |  |  |
| `milestoneterms` | select | Payment Terms | no |  |  |
| `projecttask` | select | Task/Milestone | no |  |  |

## Sublist `recurrence` (Custom Recurrence Pattern)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `amount` | ratehighprecision | Amount | yes |  |  |
| `count` | integer | Count | yes |  |  |
| `paymentterms` | select | Payment Terms | no |  |  |
| `recurrencedate` | date | Date | yes |  |  |
| `relativetoprevious` | checkbox | Relative to Previous | no |  |  |
| `units` | select | Units | yes |  |  |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `externalid` | select | External ID |  |
| `externalidstring` | text | External ID (Text) |  |
| `formuladate` | date | Formula (Date) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulatext` | text | Formula (Text) |  |
| `frequency` | select | Recurring Frequency |  |
| `id` | integer | Billing Schedule ID |  |
| `inarrears` | checkbox | In Arrears |  |
| `initialamount` | currency | Initial Amount |  |
| `initialterms` | select | Initial Payment Terms |  |
| `internalid` | select | Internal ID |  |
| `internalidnumber` | integer | Internal ID (Number) |  |
| `isinactive` | checkbox | Inactive |  |
| `ispublic` | checkbox | Public |  |
| `name` | text | Name |  |
| `recurrencecount` | integer | Recurrence Count |  |
| `recurrenceterms` | select | Recurrence Payment Terms |  |
| `repeatevery` | integer | Repeat Every |  |
| `type` | select | Type |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `externalid` | select | External ID |  |
| `formulacurrency` | currency | Formula (Currency) |  |
| `formuladate` | date | Formula (Date) |  |
| `formuladatetime` | datetime | Formula (Date/Time) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulapercent` | percent | Formula (Percent) |  |
| `formulatext` | text | Formula (Text) |  |
| `frequency` | select | Recurring Frequency |  |
| `id` | integer | BillingSchedule ID |  |
| `inarrears` | checkbox | In Arrears |  |
| `initialamount` | currency | Initial Amount |  |
| `initialterms` | text | Initial Payment Terms |  |
| `internalid` | select | Internal ID |  |
| `isinactive` | checkbox | Inactive |  |
| `ispublic` | checkbox | Public |  |
| `name` | text | Name |  |
| `recurrencecount` | integer | Recurrence Count |  |
| `recurrencepattern` | select | Recurrence Pattern |  |
| `recurrenceterms` | text | Recurrence Payment Terms |  |
| `repeatevery` | integer | Repeat Every |  |
| `type` | select | Type |  |
