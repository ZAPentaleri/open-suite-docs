# Project (`job`)
<!-- id: record:job -->

| Fact | Value |
|---|---|
| Internal ID | `job` |
| Category | entity |
| Client scripts | yes |
| Server scripts | yes |
| Searchable | yes |
| Tags | `custom_fields_supported`, `no_copy`, `no_transform`, `scripting_full` |

## Record facts

- `record_actions`: `markcompletelybilled`
- `action_preconditions`: `status_closed_or_allows_completely_billed`, `billing_type_charge_based`, `uses_project_revenue_rule`, `not_already_completely_billed`
- `action_effect`: `recognized_revenue_reconciled_with_charges`
- `governance_note`: `action:5`, `bulk:50`
- `parameters`: `record_id_only`
- `script_types`: `client`, `server`
- `requires_feature`: `Job Management` [`real_name`]
- `script_functions`: `read`, `create`, `update`, `search`, `delete`
- `not_supported`: `copy`, `transform`
- `not_scriptable`: `assigned_employees_sublist`, `job_requisition_sublist`
- `record_macros`: `clearBudgetAmounts`, `distributeBudgetTotalAmount` [`budget_machine`]
- `macro_params`: `requisites.params.sublistId:cbudget_or_bbudget`
- `related` (refs `N/record.Record#executeMacro`)
- `record_macros`: `selectAllBudgetLines`, `setBudgetAmountsToCalculated`, `unselectAllBudgetLines` [`budget_machine`]
- `applies_to`: `project`, `project_task`, `project_template` [`budget_machine`]
- `option_meanings`: `cbudget:cost_budget`, `bbudget:billing_budget`
- `ui_location`: `clear_values`, `distribute_total`, `mark_all`, `set_to_calculated`, `unmark_all` [`buttons`]
- `doc_inconsistency`: `parameter_notes_name_unselectallbudgetlines`
- `script_types`: `client`, `server`
- `script_functions`: `create`, `update`, `copy`, `delete`, `search`, `transform`
- `requires_feature`: `Projects` [`project_management_for_advanced_tracking`, `real_name`]
- `system_generated_fields`: `datecreated`
- `sublist_requires_feature`: `timeitem:time_tracking`
- `not_scriptable_when`: `over_9500_time_entries_static_list`
- `rule`: `resource_with_two_roles_added_as_two_lines`
- `unique_required`: `false` [`jobresource`]
- `arm_fields`: `accountingperiod`, `comments`, `percent`
- `purpose`: `manage_company_initiatives`
- `configured_by`: `enable_features_company_subtab` [`projects`, `project_management`]
- `caveat`: `project_management_box_needs_purchased_add_on`
- `ui_location`: `lists_relationships_projects`
- `workaround`: `custom_field_defaulting_to_today` (when `own_creation_date_needed`)
- `value_kind`: `inline_editor_sublist` [`time_tracking`]
- `difference`: `ui_one_resource_line_many_roles_script_one_line_per_role`
- `constraint`: `ui_resource_names_unique_per_line`
- `effect`: `script_added_role_lines_shown_merged_in_ui`
- `caveat`: `static_time_entry_lists_give_inaccurate_script_results`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `accountnumber` | text | Account | no | yes |  | `purpose`: `project_account_number` |
| `actualtime` | time | Actual Work | no | no |  | `purpose`: `actual_time`; `value_source`: `all_time_entries_any_status`; `applies_to`: `crm_tasks` (when `include_crm_tasks_in_totals`) |
| `allowallresourcesfortasks` | checkbox | Display All Resources for ProjectTask Assignment | no | yes |  | `purpose`: `allow_all_resources_for_tasks`; `false_means`: `only_resources_subtab_members`; `effect`: `overrides_allocation_requirement` (when `resource_allocations`); `caveat`: `generic_resources_always_assignable` |
| `allowexpenses` | checkbox | Allow Expenses | no | yes |  | `purpose`: `allow_expenses`; `effect`: `only_assignees_see_project` (when `limit_time_entry_to_assignees`) |
| `allowtime` | checkbox | Allow Time Entry | no | yes |  | `purpose`: `allow_time_entry`; `effect`: `only_assignees_see_project` (when `limit_time_entry_to_assignees`) |
| `applyprojectexpensetypetoall` | checkbox | Apply to all time entries | no | no |  | `purpose`: `apply_expense_type_to_all_time`; `effect`: `overrides_other_expense_types` |
| `autoname` | checkbox | Auto | no | no |  | `purpose`: `auto_name`; `true_means`: `id_copied_from_project_name` |
| `bbudgetshowcalculatedlines` | checkbox | Show Calculated Lines | no | no |  | `purpose`: `show_calculated_budget_lines` |
| `bbudgetusecalculatedvalues` | checkbox | Use Calculated Values for all Billing Budgets | no | no |  | `purpose`: `use_calculated_billing_budgets`; `cannot`: `enter_amounts_manually` (when `checked`); `not_supported_for`: `non_billable_tasks` |
| `billingratecard` | select | Billing Rate Card | no |  |  | `purpose`: `billing_rate_card`; `can`: `customer_specific_rate_card` |
| `billingschedule` | select | Billing Schedule | no | no |  | `purpose`: `billing_schedule`; `option_meanings`: `fixed_bid_interval:public_shared`, `time_and_materials:public_shared`, `fixed_bid_milestone:private_to_project` |
| `calculatedenddate` | date | Calculated End Date | no | no |  | `purpose`: `calculated_end_date`; `value_source`: `all_tasks_complete_per_schedule`; `effect`: `changes_with_schedule` |
| `calculatedenddatebaseline` | date | Calculated End Date Baseline | no | no |  | `purpose`: `baseline_calculated_end_date` |
| `calculatedstartdate` | date | Calculated Start Date | no | no |  | `purpose`: `calculated_start_date`; `applies_when`: `backward_scheduling` |
| `category` | select | Category | no | yes |  | `purpose`: `customer_category`; `allowed_values_listed_in`: `accounting_list_customer_category`; `value_examples`: `wholesale`, `retail`, `online` |
| `cbudgetshowcalculatedlines` | checkbox | Show Calculated Lines | no | no |  | `purpose`: `show_calculated_cost_budget_lines` |
| `cbudgetusecalculatedvalues` | checkbox | Use Calculated Values for all Cost Budgets | no | no |  | `purpose`: `use_calculated_labor_budgets`; `cannot`: `enter_amounts_manually` (when `checked`) |
| `comments` | textarea | Comments | no | yes |  | `purpose`: `comments` |
| `companyname` | text | Project Name | no | yes |  | `purpose`: `project_name`; `constraint`: `unique`; `recommendation`: `enter_when_auto_numbering` |
| `contact` | select | Primary Contact | no | no |  | `purpose`: `project_contact` |
| `currency` | select | Currency | no | no |  | `purpose`: `project_currency`; `value_source`: `customer_currency`; `effect`: `used_for_all_project_transactions`; `requires_feature`: `Multi-Currency Customers` (when `choosing_other_currency`) [`real_name`] |
| `currencyprecision` | text | Currency Precision | no | no |  | `purpose`: `currency_precision`; `value_source`: `system_displayed`; `value_examples`: `usd_precision_2` |
| `customform` | select | Custom Form | yes | no |  | `purpose`: `entry_form`; `special_values`: `Standard Project Form`; `ui_effect`: `new_option_creates_record` |
| `datecreated` | datetime | Date Created | no | no |  | `purpose`: `date_created`; `value_source`: `system_displayed` |
| `enddate` | date | Actual End Date | no | no |  | `purpose`: `project_end_date` |
| `entityid` | text | Project ID | yes | yes |  | `purpose`: `project_id`; `default_behavior`: `copied_from_project_name`; `value_source`: `auto_generated_number` (when `auto_generated_numbering`); `requires`: `autoname_cleared` (when `manual_id`) |
| `entitystatus` | select | Status | no | yes |  | `purpose`: `project_status`; `allowed_values_listed_in`: `accounting_list_project_status` |
| `estimatedcost` | currency | Estimated Cost | no | no |  | `purpose`: `estimated_cost` |
| `estimatedgrossprofit` | currency | Estimated Gross Profit | no | no |  | `purpose`: `estimated_gross_profit`; `value_source`: `price_minus_estimated_labor_cost` (when `fixed_bid_without_job_costing`); `value_source`: `labor_revenue_minus_labor_cost` (when `other_without_job_costing`); `value_source`: `estimated_revenue_minus_cost` (when `job_costing_or_estimates`) |
| `estimatedgrossprofitpercent` | percent | Estimated Gross Profit Percent | no | no |  | `purpose`: `estimated_gross_profit_percent`; `value_source`: `profit_divided_by_labor_revenue` (when `time_and_materials`); `value_source`: `profit_divided_by_project_price` (when `fixed_bid`) |
| `estimatedlaborcost` | currency | Estimated Labor Cost | no | no |  | `purpose`: `estimated_labor_cost` |
| `estimatedlaborcostbaseline` | currency | Estimated Labor Cost Baseline | no | no |  | `purpose`: `baseline_estimated_labor_cost`; `value_source`: `value_when_baseline_set` |
| `estimatedlaborrevenue` | currency | Estimated Labor Revenue | no | no |  | `purpose`: `estimated_labor_revenue` |
| `estimatedrevenue` | currency | Estimated Revenue | no | no |  | `purpose`: `estimated_revenue` |
| `estimatedtime` | time | Initial Time Budget | no | yes |  | `purpose`: `crm_task_budgeted_time`; `value_source`: `sum_of_crm_task_initial_budgets` |
| `estimatedtimeoverride` | time | Estimated Work | no | no |  | `purpose`: `estimated_work`; `value_source`: `sum_of_task_estimates`; `applies_to`: `crm_task_current_budgets` (when `include_crm_tasks_in_totals`) |
| `estimatedtimeoverridebaseline` | time | Estimated Work Baseline | no | no |  | `purpose`: `baseline_estimated_work`; `value_source`: `value_when_baseline_set`; `help_text_shared_with`: `projecttask.estimatedworkbaseline` |
| `estimaterevrectemplate` | select | Rev Rec Forecast Template | no | no |  | `purpose`: `revenue_forecast_template`; `effect`: `generates_forecast_schedule` |
| `externalid` | text | ExternalId | no | no |  | `purpose`: `external_id` |
| `fxrate` | currency2 | Exchange Rate | no | no |  | `purpose`: `project_exchange_rate`; `default_behavior`: `rate_at_project_creation`; `can`: `override_for_estimates` |
| `includecrmtasksintotals` | checkbox | Include CRM Tasks in Project Totals | no | yes |  | `purpose`: `include_crm_tasks_in_totals`; `effect`: `crm_tasks_in_cost_planned_and_actual_time` |
| `isexempttime` | checkbox | Classify Time as Exempt | no | yes |  | `purpose`: `exempt_time`; `effect`: `excluded_from_utilization` |
| `isinactive` | checkbox | Inactive | no | yes |  | `purpose`: `inactivate_project`; `ui_effect`: `hidden_from_projects_list`; `cannot`: `edit_inactive_project` |
| `isproductivetime` | checkbox | Classify Time as Productive | no | yes |  | `purpose`: `productive_time`; `caveat`: `not_billed_not_in_revenue`; `value_examples`: `training`, `pre_sales_support` |
| `isutilizedtime` | checkbox | Classify Time as Utilized | no | yes |  | `purpose`: `utilized_time`; `effect`: `contributes_to_project_revenue` |
| `jobbillingtype` | select | Billing Type | no | no |  | `purpose`: `project_billing_type`; `options`: `Charge-Based`, `Fixed Bid, Interval`, `Fixed Bid Milestone`, `Time and Materials`; `requires_feature`: `Charge-Based Billing` (when `charge_based`) [`real_name`]; `option_meanings`: `charge_based:charges_from_charge_rules`, `fixed_bid_interval:percent_complete_at_intervals`, `fixed_bid_milestone:percent_per_completed_milestone`, `time_and_materials:time_and_expenses` |
| `jobitem` | select | Billing Item | no | no |  | `purpose`: `billing_service_item`; `displayed_on`: `billed_transactions` |
| `jobprice` | currency | Project Price | no | no |  | `purpose`: `project_price`; `effect`: `used_in_gross_profit_margin` |
| `jobtype` | select | Project Type | no | no |  | `purpose`: `project_type`; `allowed_values_listed_in`: `user_defined_project_types` |
| `language` | select | Language | no | no |  | `purpose`: `project_language`; `effect`: `language_of_project_records_and_invoices` |
| `lastbaselinedate` | date | Last Baseline Date | no | no |  | `purpose`: `last_baseline_date` |
| `lastmodifieddate` | datetime | Last Modified Date | no | no |  | `purpose`: `date_last_modified` |
| `limittimetoassignees` | checkbox | Limit Time Entry to Assignees | no | yes |  | `purpose`: `limit_time_to_assignees`; `true_means`: `only_listed_resources_enter_time_and_expenses`; `caveat`: `vendor_center_shows_only_to_listed_vendors` |
| `materializetime` | checkbox | Create Planned Time Entries | no | yes |  | `purpose`: `materialize_planned_time`; `effect`: `planned_time_visible_in_timesheets`; `required_when`: `project_budgets_calculated_costs` |
| `parent` | select | Customer | no | no |  | `purpose`: `project_customer`; `accepts_forms`: `customer`, `customer_and_project` [`subproject`] |
| `percentcomplete` | percent | Percent Complete | no | no |  | `purpose`: `percent_complete_estimate`; `caveat`: `not_calculated_by_system` |
| `percenttimecomplete` | percent | Percent Work Complete | no | no |  | `purpose`: `percent_work_complete`; `value_source`: `actual_hours_divided_by_estimated_work`; `effect`: `100_when_all_tasks_completed` |
| `plannedwork` | time | Planned Work | no | no |  | `purpose`: `planned_work_total`; `caveat`: `not_updated_by_unplanned_time` |
| `projectedenddate` | date | Estimated End Date | no | no |  | `purpose`: `estimated_end_date`; `comparable_to`: `calculatedenddate` (when `advanced_projects`) |
| `projectedenddatebaseline` | date | Estimated End Date Baseline | no | no |  | `purpose`: `baseline_estimated_end_date`; `value_source`: `value_when_baseline_set`; `requires_feature`: `Advanced Projects` [`real_name`] |
| `projectexpensetype` | select | Project Expense Type | yes | no |  | `purpose`: `project_expense_type`; `effect`: `selects_account_debited_for_time` |
| `projectmanager` | select | Project Manager | no | yes |  | `purpose`: `project_manager`; `constraint`: `employee_marked_project_manager` |
| `scheduledenddate` | date | Scheduled End Date | no | no |  | `purpose`: `scheduled_end_date`; `applies_when`: `backward_scheduling` |
| `schedulingmethod` | select | Scheduling Method | yes | no |  | `purpose`: `scheduling_method`; `options`: `Forward`, `Backward`; `option_meanings`: `forward:start_date_given_end_calculated`, `backward:end_date_given_start_calculated` |
| `sourceserviceitemfromratecard` | checkbox | Source Service Item to Task Assignments from Rate Card | no |  |  | `purpose`: `source_service_item_from_rate_card`; `value_source`: `system_displayed` |
| `startdate` | date | Start Date | no | no |  | `purpose`: `project_start_date`; `effect`: `tasks_without_predecessors_start_here` |
| `startdatebaseline` | date | Start Date Baseline | no | no |  | `purpose`: `baseline_start_date`; `value_source`: `value_when_baseline_set`; `requires_feature`: `Advanced Projects` [`real_name`] |
| `subsidiary` | select | Subsidiary | yes | no |  | `purpose`: `subsidiary`; `value_source`: `system_displayed` |
| `timeapproval` | select | Time Approval | no | yes |  | `purpose`: `time_approval_method`; `options`: `approve_time_automatically`, `default_time_approver`, `project_time_approver`, `project_time_approver_or_default_time_approver`; `default_value`: `project_time_approver_or_default_time_approver`; `caveat`: `full_time_permission_roles_always_approve` |
| `timeremaining` | time | Remaining Work | no | no |  | `purpose`: `time_remaining`; `value_source`: `estimated_minus_actual_work` |
| `createchargerule` | checkbox | Create Charge Rule | no |  |  |  |
| `isbasecurrency` | checkbox | Base Currency | no | no |  |  |
| `isjob` | text |  | no | no |  |  |
| `otherrelationships` | select | Other Relationships | no | no |  |  |
| `plannedworkbaseline` | time | Planned Work Baseline | no | no |  |  |
| `stage` | text |  | no | no |  |  |

## Sublist `bbudget` (Billing Budget)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `costcategoryref` | integer |  | no |  |  |
| `costcategorysubtype` | text |  | no |  |  |
| `costcategorytype` | text |  | no |  |  |
| `selectline` | checkbox | Select | no |  |  |
| `totalamount` | currency | Project Total | no |  |  |

## Sublist `cbudget` (Cost Budget)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `costcategoryref` | integer |  | no |  |  |
| `costcategorysubtype` | text |  | no |  |  |
| `costcategorytype` | text |  | no |  |  |
| `selectline` | checkbox | Select | no |  |  |
| `totalamount` | currency | Project Total | no |  |  |

## Sublist `jobresources` (Resource Details)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `defaultcost` | currency | Default Cost | no |  |  |
| `email` | email | Email | no |  |  |
| `jobresource` | select | Name | yes |  |  |
| `overridencost` | currency | Cost Override | no |  |  |
| `role` | select | Role | yes |  |  |

## Sublist `percentcompleteoverride` (Rev Rec Percent Complete Override)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `comments` | text | Comments | no |  |  |
| `percent` | percent | Cumulative Percent Complete | yes |  |  |
| `period` | select | Accounting Period | yes |  |  |

## Sublist `plstatement` (P&L)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `costcategoryref` | integer |  | no |  |  |
| `costcategorysubtype` | text |  | no |  |  |
| `costcategorytype` | text |  | no |  |  |

## Tabs

| Tab ID | Label |
|---|---|
| `address` | Address |
| `budget` | Budget |
| `custom` | Custom |
| `financial` | Financial |
| `general` | General |
| `info` | Info |
| `pl` | P&L |
| `resources` | Resources |
| `schedule` | Schedule |
| `workflow` | Workflow |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `accountnumber` | text | Account |  |
| `actualtime` | float | Actual Work |  |
| `address` | textarea | Address |  |
| `addressee` | text | Addressee |  |
| `addresslabel` | text | Address Label |  |
| `addressphone` | phone | Address Phone |  |
| `allocatepayrollexpenses` | checkbox | Allocate Payroll Expenses |  |
| `allowallresourcesfortasks` | checkbox | Display All Resources for ProjectTask Assignment |  |
| `allowexpenses` | checkbox | Allow Expenses |  |
| `allowtime` | checkbox | Allow Time Entry |  |
| `applyprojectexpensetypetoall` | checkbox | Apply to all time entries |  |
| `attention` | text | Attention |  |
| `billingratecard` | select | Billing Rate Card |  |
| `billingschedule` | select | Billing Schedule |  |
| `calculatedenddate` | date | Calculated End Date |  |
| `calculatedenddatebaseline` | date | Calculated End Date Baseline |  |
| `calculatedstartdate` | date | Calculated Start Date |  |
| `category` | select | Category |  |
| `city` | text | City |  |
| `comments` | textarea | Comments |  |
| `contact` | text | Contact |  |
| `country` | select | Country |  |
| `county` | text | County/State |  |
| `customer` | select | Customer |  |
| `datecreated` | datetime | Date Created |  |
| `email` | text | Email |  |
| `enddate` | date | Actual End Date |  |
| `entityid` | text | Name/ID |  |
| `estcost` | currency | Estimated Cost |  |
| `estenddate` | date | Projected End Date |  |
| `estimatedgrossprofit` | currency | Estimated Gross Profit |  |
| `estimatedgrossprofitpercent` | percent | Estimated Gross Profit % |  |
| `estimatedlaborcost` | currency | Estimated Labor Cost |  |
| `estimatedlaborcostbaseline` | currency | Estimated Labor Cost Baseline |  |
| `estimatedlaborrevenue` | currency | Estimated Labor Revenue |  |
| `estimatedtime` | float | Initial Time Budget |  |
| `estimatedtimeoverride` | float | Estimated Work |  |
| `estimatedtimeoverridebaseline` | float | Estimated Work Baseline |  |
| `estrevenue` | currency | Estimated Revenue |  |
| `externalid` | select | External ID |  |
| `externalidstring` | text | External ID (Text) |  |
| `fax` | phone | Fax |  |
| `formuladate` | date | Formula (Date) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulatext` | text | Formula (Text) |  |
| `giveaccess` | checkbox | Login Access |  |
| `globalsubscriptionstatus` | select | Global Subscription Status |  |
| `image` | image | Image |  |
| `includecrmtasksintotals` | checkbox | Include CRM Tasks in Job Totals |  |
| `internalid` | select | Internal ID |  |
| `internalidnumber` | integer | Internal ID (Number) |  |
| `isdefaultbilling` | checkbox | Default Billing Address |  |
| `isdefaultshipping` | checkbox | Default Shipping Address |  |
| `isexempttime` | checkbox | Classify Time as Exempt |  |
| `isinactive` | checkbox | Inactive |  |
| `isproductivetime` | checkbox | Classify Time as Productive |  |
| `isutilizedtime` | checkbox | Classify Time as Utilized |  |
| `jobbillingtype` | select | Billing Type |  |
| `jobitem` | select | Billing Item |  |
| `jobprice` | currency | Job Price |  |
| `jobresource` | select | Job Resource |  |
| `jobresourcerole` | select | Job Resource Role |  |
| `jobtype` | select | Job Type |  |
| `language` | select | Language |  |
| `lastbaselinedate` | date | Last Baseline Date |  |
| `lastmodifieddate` | datetime | Last Modified |  |
| `level` | select | Level |  |
| `limittimetoassignees` | checkbox | Limit Time and Expenses To Resources |  |
| `materializetime` | checkbox | Create Planned Time Entries |  |
| `parent` | select | Sub of |  |
| `pctcomplete` | integer | Percent Complete |  |
| `percenttimecomplete` | integer | Percent Time Complete |  |
| `permission` | select | Permission |  |
| `phone` | phone | Phone |  |
| `phoneticname` | text | Phonetic Name |  |
| `plannedwork` | float | Planned Work |  |
| `plannedworkbaseline` | float | Planned Work Baseline |  |
| `projectedenddatebaseline` | date | Projected End Date Baseline |  |
| `projectmanager` | select | Project Manager |  |
| `representingsubsidiary` | select | Represents Subsidiary |  |
| `scheduledenddate` | date | Scheduled End Date |  |
| `schedulingmethod` | select | Scheduling Method |  |
| `sourceserviceitemfromratecard` | checkbox | Source Service Item to Task Assignments from Rate Card |  |
| `startdate` | date | Start Date |  |
| `startdatebaseline` | date | Start Date Baseline |  |
| `state` | select | State/Province |  |
| `status` | select | Status |  |
| `subsidiary` | select | Subsidiary |  |
| `timeapproval` | select | Time Approval |  |
| `timeremaining` | float | Remaining Work |  |
| `zipcode` | text | Zip |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `accountnumber` | text | Account |  |
| `actualtime` | float | Actual Work |  |
| `address` | text | Address |  |
| `addressee` | text | Addressee |  |
| `addressinternalid` | text | Address Internal ID |  |
| `addresslabel` | text | Address Label |  |
| `addressphone` | text | Address Phone |  |
| `allocatepayrollexpenses` | checkbox | Allocate Payroll Expenses |  |
| `allowallresourcesfortasks` | checkbox | Display All Resources for ProjectTask Assignment |  |
| `allowexpenses` | checkbox | Allow Expenses |  |
| `allowtime` | checkbox | Allow Time Entry |  |
| `altcontact` | text | Alt. Contact |  |
| `altemail` | email | Alt. Email |  |
| `altname` | text | Name |  |
| `altphone` | phone | Office Phone |  |
| `attention` | text | Attention |  |
| `bbudgetusecalculatedvalues` | checkbox | Use Calculated Values for all Billing Budgets |  |
| `billcountrycode` | text | Billing Country Code |  |
| `billingratecard` | select | Billing Rate Card |  |
| `billingschedule` | select | Billing Schedule |  |
| `billzipcode` | text | Billing Zip |  |
| `calculatedenddate` | date | Calculated End Date |  |
| `calculatedenddatebaseline` | date | Calculated End Date Baseline |  |
| `calculatedstartdate` | date | Calculated Start Date |  |
| `category` | select | Category |  |
| `cbudgetlaborbudgetfromalloc` | checkbox | Calculate Labor Budgets from Resource Allocations |  |
| `cbudgetusecalculatedvalues` | checkbox | Use Calculated Values for all Cost Budgets |  |
| `city` | text | City |  |
| `comments` | text | Comments |  |
| `companyname` | text | Job Name |  |
| `contact` | text | Primary Contact |  |
| `country` | select | Country |  |
| `countrycode` | text | Country Code |  |
| `customer` | select | Customer |  |
| `datecreated` | datetime | Date Created |  |
| `email` | email | Email |  |
| `enddate` | date | Actual End Date |  |
| `entityid` | text | Name |  |
| `entitynumber` | integer | Number |  |
| `entitystatus` | select | Status |  |
| `estimatedcost` | currency | Estimated Cost |  |
| `estimatedgrossprofit` | currency | Estimated Gross Profit |  |
| `estimatedgrossprofitpercent` | percent | Estimated Gross Profit % |  |
| `estimatedlaborcost` | currency | Estimated Labor Cost |  |
| `estimatedlaborcostbaseline` | currency | Estimated Labor Cost Baseline |  |
| `estimatedlaborrevenue` | currency | Estimated Labor Revenue |  |
| `estimatedrevenue` | currency | Estimated Revenue |  |
| `estimatedtime` | float | Initial Time Budget |  |
| `estimatedtimeoverride` | float | Estimated Work |  |
| `estimatedtimeoverridebaseline` | float | Estimated Work Baseline |  |
| `externalid` | select | External ID |  |
| `fax` | text | Fax |  |
| `formulacurrency` | currency | Formula (Currency) |  |
| `formuladate` | date | Formula (Date) |  |
| `formuladatetime` | datetime | Formula (Date/Time) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulapercent` | percent | Formula (Percent) |  |
| `formulatext` | text | Formula (Text) |  |
| `globalsubscriptionstatus` | select | Global Subscription Status |  |
| `image` | select | Image |  |
| `includecrmtasksintotals` | checkbox | Include CRM Tasks in Job Totals |  |
| `internalid` | select | Internal ID |  |
| `isdefaultbilling` | checkbox | Default Billing Address |  |
| `isdefaultshipping` | checkbox | Default Shipping Address |  |
| `isexempttime` | checkbox | Classify Time as Exempt |  |
| `isinactive` | checkbox | Inactive |  |
| `isproductivetime` | checkbox | Classify Time as Productive |  |
| `isutilizedtime` | checkbox | Classify Time as Utilized |  |
| `jobbillingtype` | select | Billing Type |  |
| `jobitem` | select | Billing Item |  |
| `jobprice` | currency | Job Price |  |
| `jobresource` | select | Job Resource |  |
| `jobresourcerole` | select | Job Resource Role |  |
| `jobtype` | select | Job Type |  |
| `language` | select | Language |  |
| `lastbaselinedate` | date | Last Baseline Date |  |
| `lastmodifieddate` | datetime | Last Modified |  |
| `lastviewed` | datetime | Last Viewed |  |
| `level` | select | Level |  |
| `limittimetoassignees` | checkbox | Limit Time and Expenses to Resources |  |
| `materializetime` | checkbox | Create Planned Time Entries |  |
| `percentcomplete` | percent | Percent Complete |  |
| `percenttimecomplete` | percent | Percent Time Complete |  |
| `permission` | select | Permission |  |
| `phone` | phone | Phone |  |
| `phoneticname` | text | Phonetic Name |  |
| `plannedwork` | float | Planned Work |  |
| `plannedworkbaseline` | float | Planned Work Baseline |  |
| `projectedenddate` | date | Projected End Date |  |
| `projectedenddatebaseline` | date | Projected End Date Baseline |  |
| `projectmanager` | select | Job Manager |  |
| `representingsubsidiary` | select | Represents Subsidiary |  |
| `scheduledenddate` | date | Scheduled End Date |  |
| `schedulingmethod` | text | Scheduling Method |  |
| `shipcountrycode` | text | Shipping Country Code |  |
| `sourceserviceitemfromratecard` | checkbox | Source Service Item to Task Assignments from Rate Card |  |
| `startdate` | date | Start Date |  |
| `startdatebaseline` | date | Start Date Baseline |  |
| `state` | select | State/Province |  |
| `statedisplayname` | select | State/Province Display Name |  |
| `subscription` | select | Subscription |  |
| `subscriptiondate` | datetime | Subscription Date |  |
| `subscriptionstatus` | checkbox | Subscription Status |  |
| `subsidiary` | select | Subsidiary |  |
| `subsidiarynohierarchy` | select | Subsidiary (no hierarchy) |  |
| `timeapproval` | select | Time Approval |  |
| `timeremaining` | float | Remaining Work |  |
| `type` | select | Type |  |
| `zipcode` | text | Zip Code |  |

## Search joins

| Join ID | Label | Joined record |
|---|---|---|
| `billingschedule` | Billing Schedule | Billing Schedule |
| `campaignresponse` | Campaign Response | Campaign |
| `contactprimary` | Contact (Primary) | Contact |
| `customer` | Customer | Customer |
| `messages` | Messages | Message |
| `messagesfrom` | Messages From | Message |
| `messagesto` | Messages To | Message |
| `projecttask` | Project Task | ProjectTask |
| `task` | Task | Task |

## Suspected source errors

> [!SOURCE-ERROR] `parameter_notes_name_unselectallbudgetlines`
<!-- source-error: record:job | - | parameter_notes_name_unselectallbudgetlines -->
