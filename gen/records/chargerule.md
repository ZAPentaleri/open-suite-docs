# Charge Rule (`chargerule`)
<!-- id: record:chargerule -->

| Fact | Value |
|---|---|
| Internal ID | `chargerule` |
| Category | list |
| Client scripts | yes |
| Server scripts | yes |
| Tags | `custom_fields_supported`, `no_transform` |

## Record facts

- `script_types`: `client`, `server`
- `not_supported`: `transform`
- `script_functions`: `create`, `read`, `update`, `delete`, `copy`, `search`
- `requires_feature`: `Project Management`, `Charge-Based Billing` [`real_name`]
- `script_types`: `client`, `server`
- `script_functions`: `create`, `read`, `update`, `delete`, `copy`, `search`
- `not_supported`: `transform`
- `read_only_when`: `generated_charges_billed`
- `required_fields`: `chargeruletype`
- `record_macros`: `copyResources`
- `purpose`: `billing_rate_timing_and_stage_of_project_charges`
- `displayed_on`: `charge_based_project_financial_subtab`
- `value_examples`: `time_based_charge_rule`
- `record_macros`: `copyResources:copy_task_assigned_resources`
- `applies_to`: `time_based_charge_rules`
- `ui_location`: `copy_resources_from_tasks_button`
- `applies_to`: `resources_with_task_assignment_or_time`
- `configured_by`: `resource_allocations_feature` [`task_allocations_included`]
- `configured_by`: `allow_allocated_resources_to_enter_time_to_all_tasks` [`project_allocations_included`]
- `initialization_default_value`: `entity`, `entity_internal_id` (refs `N/record.create`, `N/record.copy`, `N/record.transform`, `N/record.load`)

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `amount` | poscurrency | Amount | yes | no |  | `purpose`: `rule_charge_amount` |
| `billingitem` | select | Billing Item | yes | no |  | `purpose`: `rule_service_item`; `effect`: `categorizes_charges_on_orders_and_invoices`; `caveat`: `item_price_used_only_for_time_rules_based_on_it` |
| `caphours` | time | Limit | no | no |  | `purpose`: `rule_cap_limit`; `effect`: `rule_stops_when_cap_reached` |
| `capmoney` | poscurrency | Limit | no | no |  | `purpose`: `rule_cap_type`; `options`: `currency_amount`, `hours`; `help_text_shared_with`: `chargerule.captype` |
| `captype` | select | Cap Type | no | no |  | `purpose`: `rule_cap_type`; `options`: `currency_amount`, `hours`; `help_text_shared_with`: `chargerule.capmoney` |
| `description` | textarea | Description | no | no |  | `purpose`: `rule_description` |
| `endbydate` | date | End By | no | no |  | `purpose`: `rule_end_date`; `default_behavior`: `project_calculated_end_date` (when `blank`) |
| `expamtmultiplier` | posfloat | Discount / Mark-up | no | no |  | `purpose`: `expense_amount_multiplier`; `value_kind`: `portion_charged_e_g_0_75_or_1_25`; `value_examples`: `0.75_for_25_percent_discount`, `1.25_for_25_percent_markup` |
| `isinactive` | checkbox | Inactive | no | no |  | `purpose`: `inactivate_rule`; `true_means`: `rule_generates_no_charges` |
| `name` | text | Name | yes | no |  | `purpose`: `rule_name` |
| `project` | select | Customer:Project | yes | no |  | `purpose`: `rule_project`; `value_source`: `system_displayed` |
| `projecttask` | select | Milestone/Task | yes | no |  | `purpose`: `milestone_task_triggering_charge` |
| `ratemultiplier` | posfloat | Rate Multiplier | no | no |  | `purpose`: `rate_multiplier`; `effect`: `billable_amount_is_rate_times_multiplier` |
| `rateroundingtype` | select | Time Rounding | no | no |  | `purpose`: `time_rounding_method` |
| `ratesourcetype` | select | Rate Basis | yes | no |  | `purpose`: `rate_source`; `options`: `time_entry_service_item`, `resource_billing_class`, `project_task_rate` |
| `ruleorder` | posinteger | Rule Order | yes | no |  | `purpose`: `rule_order_among_time_rules` |
| `saleunit` | select | Sale Units | no | no |  | `purpose`: `rule_sale_unit`; `allowed_values_listed_in`: `unitstype`; `effect`: `custom_interval_billing_rates` |
| `seriesstartdate` | date | Series Start Date | no | no |  | `purpose`: `rule_start_date` |
| `stage` | select | Initial Charge Stage | yes | no |  | `purpose`: `initial_charge_stage`; `can`: `change_stage_from_project_pending_or_charge_record` |
| `stopifcapped` | checkbox | Do Not Bill Entries Exceeding Cap | no | no |  | `purpose`: `stop_billing_beyond_cap` |
| `unitstype` | select | Units Type | no | no |  | `purpose`: `rule_units_type`; `effect`: `custom_interval_billing_rates` |
| `chargeruletype` | select | Charge Rule Type | yes | no |  |  |
| `customform` | select | Custom Form | yes | no |  |  |
| `externalid` | text | ExternalId | no | no |  |  |
| `frequency` | text |  | no | no |  |  |
| `noenddate` | checkbox | No End Date | no | no |  |  |
| `savedsearch` | integer |  | no | no |  |  |
| `scheduledate` | date | Date | no | no |  |  |

## Sublist `resourcerateoverride` (Resources)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `name` | select | Name | yes |  |  |
| `rate` | poscurrency | Rate | yes |  |  |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `billingitem` | select | Billing Item |  |
| `chargedate` | date | Charge Date |  |
| `chargeruletype` | select | Type |  |
| `description` | text | Description |  |
| `enddate` | date | End By |  |
| `expamtmultiplier` | float | Expense Amount Multiplier |  |
| `externalid` | select | External ID |  |
| `externalidstring` | text | External ID (Text) |  |
| `formuladate` | date | Formula (Date) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulatext` | text | Formula (Text) |  |
| `frequency` | text | Frequency |  |
| `internalid` | select | Internal ID |  |
| `internalidnumber` | integer | Internal ID (Number) |  |
| `name` | text | Name |  |
| `project` | select | Project |  |
| `projecttask` | text | Task / Milestone |  |
| `projecttaskcompletiondate` | date | Completion Date |  |
| `projecttaskstatus` | select | Status |  |
| `ratemultiplier` | float | Rate Multiplier |  |
| `rateroundingtype` | select | Rate Rounding |  |
| `ratesourcetype` | select | Rate Basis |  |
| `ruleorder` | integer | Rule Order |  |
| `saleunit` | select | Sale Units |  |
| `stage` | select | Initial Charge Stage |  |
| `unitstype` | select | Units Type |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `amount` | currency | Amount |  |
| `billingitem` | select | Billing Item |  |
| `chargedate` | date | Charge Date |  |
| `chargeruletype` | text | Type |  |
| `company` | select | Project |  |
| `description` | text | Description |  |
| `enddate` | date | End By |  |
| `expamtmultiplier` | float | Discount / Mark-up |  |
| `externalid` | select | External ID |  |
| `formulacurrency` | currency | Formula (Currency) |  |
| `formuladate` | date | Formula (Date) |  |
| `formuladatetime` | datetime | Formula (Date/Time) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulapercent` | percent | Formula (Percent) |  |
| `formulatext` | text | Formula (Text) |  |
| `frequency` | text | Frequency |  |
| `internalid` | select | Internal ID |  |
| `name` | text | Name |  |
| `projecttask` | select | Task / Milestone |  |
| `projecttaskcompletiondate` | date | Completion Date |  |
| `projecttaskstatus` | text | Status |  |
| `ratemultiplier` | float | Rate Multiplier |  |
| `rateroundingtype` | text | Rate Rounding |  |
| `ratesourcetype` | text | Rate Basis |  |
| `ruleorder` | integer | Rule Order |  |
| `saleunit` | select | Sale Units |  |
| `stage` | text | Initial Charge Stage |  |
| `unitstype` | select | Units Type |  |

## Search joins

| Join ID | Label | Joined record |
|---|---|---|
| `unitstype` | Units Type | Units Type |
| `user` | User | Employee |
