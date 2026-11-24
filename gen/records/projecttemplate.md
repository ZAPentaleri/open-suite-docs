# Project Template (`projecttemplate`)
<!-- id: record:projecttemplate -->

| Fact | Value |
|---|---|
| Internal ID | `projecttemplate` |
| Category | entity |
| Client scripts | yes |
| Server scripts | yes |
| Tags | `custom_fields_supported`, `no_copy` |

## Record facts

- `script_functions`: `read`, `create`, `edit`, `delete`, `search`
- `requires_feature`: `Projects` [`project_management_for_advanced_tracking`, `real_name`]
- `script_types`: `client`, `server`
- `not_supported`: `copy`
- `purpose`: `reusable_starting_point_for_repeated_projects`
- `configured_by`: `enable_features_company_subtab` [`projects`, `project_management`]
- `caveat`: `project_management_box_needs_purchased_add_on`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `allowallresourcesfortasks` | checkbox | Display All Resources for Project Task Assignment | no | no |  | `purpose`: `allow_all_resources_for_tasks`; `applies_to`: `projects_created_from_template`; `false_means`: `only_resources_subtab_members` |
| `allowexpenses` | checkbox | Allow Expenses | no | no |  | `purpose`: `allow_expenses`; `applies_to`: `projects_created_from_template`; `effect`: `only_assignees_see_project` (when `limit_time_entry_to_assignees`) |
| `allowtasktimeforrsrcalloc` | checkbox | Allow Allocated Resources to Enter Time to All Tasks | no | no |  | `purpose`: `allocated_resources_enter_time_on_any_task`; `applies_to`: `projects_created_from_template` |
| `allowtime` | checkbox | Allow Time Entry | no | no |  | `purpose`: `allow_time_entry`; `applies_to`: `projects_created_from_template`; `effect`: `only_assignees_see_project` (when `limit_time_entry_to_assignees`) |
| `applyprojectexpensetypetoall` | checkbox | Apply to all time entries | no | no |  | `purpose`: `apply_expense_type_to_all_time`; `effect`: `overrides_other_expense_types` |
| `bbudgetshowcalculatedlines` | checkbox | Show Calculated Lines | no | no |  | `purpose`: `show_calculated_budget_lines` |
| `bbudgetusecalculatedvalues` | checkbox | Use Calculated Values for all Billing Budgets | no | no |  | `purpose`: `use_calculated_billing_budgets`; `cannot`: `enter_amounts_manually` (when `checked`) |
| `billingschedule` | select | Billing Schedule | no | no |  | `purpose`: `billing_schedule`; `option_meanings`: `fixed_bid_interval:public_shared`, `time_and_materials:public_shared`, `fixed_bid_milestone:private_to_project` |
| `cbudgetlaborbudgetfromalloc` | checkbox | Calculate Labor Budgets from Resource Allocations | no | no |  | `purpose`: `labor_budget_from_allocations`; `cannot`: `enter_amounts_manually` (when `checked`) |
| `cbudgetshowcalculatedlines` | checkbox | Show Calculated Lines | no | no |  | `purpose`: `show_calculated_cost_budget_lines` |
| `cbudgetusecalculatedvalues` | checkbox | Use Calculated Values for all Cost Budgets | no | no |  | `purpose`: `use_calculated_labor_budgets`; `cannot`: `enter_amounts_manually` (when `checked`) |
| `entityid` | text | Name | yes | yes |  | `purpose`: `template_name` |
| `estimatedgrossprofit` | currency | Estimated Gross Profit | no | no |  | `purpose`: `estimated_gross_profit`; `value_source`: `labor_revenue_minus_labor_cost` (when `time_and_materials`); `value_source`: `price_minus_estimated_labor_cost` (when `fixed_bid`) |
| `estimatedgrossprofitpercent` | percent | Estimated Gross Profit Percent | no | no |  | `purpose`: `estimated_gross_profit_percent`; `value_source`: `profit_divided_by_labor_revenue` (when `time_and_materials`); `value_source`: `profit_divided_by_project_price` (when `fixed_bid`) |
| `estimatedlaborcost` | currency | Estimated Labor Cost | no | no |  | `purpose`: `estimated_labor_cost`; `value_source`: `task_labor_costs` |
| `estimatedlaborrevenue` | currency | Estimated Labor Revenue | no | no |  | `purpose`: `estimated_labor_revenue`; `value_source`: `task_labor_prices`; `help_text_shared_with`: `projecttemplate.estimatedrevenuejc` |
| `estimatedrevenuejc` | currency | Estimated Revenue | no | no |  | `purpose`: `estimated_labor_revenue`; `value_source`: `task_labor_prices`; `help_text_shared_with`: `projecttemplate.estimatedlaborrevenue` |
| `estimatedtimeoverride` | time | Estimated Work | no | no |  | `purpose`: `estimated_work`; `value_source`: `sum_of_task_estimates`; `applies_to`: `crm_task_current_budgets` (when `include_crm_tasks_in_totals`) |
| `includecrmtasksintotals` | checkbox | Include CRM Tasks in Project Totals | no | no |  | `purpose`: `include_crm_tasks_in_totals`; `applies_to`: `projects_created_from_template` |
| `isexempttime` | checkbox | Classify Time as Exempt | no | no |  | `purpose`: `exempt_time`; `effect`: `excluded_from_utilization` |
| `isinactive` | checkbox | Inactive | no | yes |  | `purpose`: `inactivate_template`; `ui_effect`: `hidden_from_template_list`; `cannot`: `edit_inactive_template` |
| `isproductivetime` | checkbox | Classify Time as Productive | no | no |  | `purpose`: `productive_time`; `caveat`: `not_billed_not_in_revenue`; `value_examples`: `training`, `pre_sales_support` |
| `isutilizedtime` | checkbox | Classify Time as Utilized | no | no |  | `purpose`: `utilized_time`; `effect`: `contributes_to_project_revenue` |
| `jobbillingtype` | select | Billing Type | no | no |  | `purpose`: `project_billing_type`; `applies_to`: `projects_created_from_template`; `options`: `Charge-Based`, `Fixed Bid, Interval`, `Fixed Bid Milestone`, `Time and Materials`; `requires_feature`: `Charge-Based Billing` (when `charge_based`) [`real_name`] |
| `jobitem` | select | Billing Item | no | no |  | `purpose`: `billing_service_item`; `displayed_on`: `billed_transactions` |
| `jobprice` | currency | Project Price | no | no |  | `purpose`: `project_price`; `effect`: `used_in_gross_profit_margin` |
| `limittimetoassignees` | checkbox | Limit Time and Expenses To Resources | no | no |  | `purpose`: `limit_time_to_assignees`; `applies_to`: `projects_created_from_template`; `caveat`: `vendor_center_shows_only_to_listed_vendors` |
| `materializetime` | checkbox | Create Planned Time Entries | no | no |  | `purpose`: `materialize_planned_time`; `applies_to`: `projects_created_from_template`; `required_when`: `project_budgets_calculated_costs` |
| `projectexpensetype` | select | Project Expense Type | yes | no |  | `purpose`: `project_expense_type`; `effect`: `selects_account_debited_for_time` |
| `projectformtemplate` | select | Custom Project Form | yes | no |  | `purpose`: `project_form_for_created_projects` |
| `startdate` | date | Start Date | no | no |  | `purpose`: `template_start_date`; `can`: `change_when_project_created`; `effect`: `tasks_without_predecessors_start_here` |
| `subsidiary` | select | Subsidiary | yes | no |  | `purpose`: `subsidiary` |
| `useallocatedtimeforforecast` | checkbox | Use Allocated Time for Forecast | no | no |  | `purpose`: `use_allocated_time_for_forecast` |
| `customform` | select | Custom Form | yes | no |  |  |
| `estimatedcost` | currency | Estimated Cost | no | no |  |  |
| `estimatedrevenue` | currency | Estimated Revenue | no | no |  |  |
| `externalid` | text | ExternalId | no | no |  |  |

## Sublist `bbudget` (Billing Budget)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `selectline` | checkbox | Select | no |  |  |
| `totalamount` | currency | Project Total | no |  |  |

## Sublist `cbudget` (Cost Budget)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `selectline` | checkbox | Select | no |  |  |
| `totalamount` | currency | Project Total | no |  |  |

## Sublist `jobresources` (Resource Details)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `jobresource` | select | Name | yes |  |  |
| `overridencost` | currency | Cost Override | no |  |  |
| `role` | select | Role | yes |  |  |

## Tabs

| Tab ID | Label |
|---|---|
| `custom` | Custom |
| `workflow` | Workflow |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `address` | textarea | Address |  |
| `addressee` | text | Addressee |  |
| `addresslabel` | text | Address Label |  |
| `addressphone` | phone | Address Phone |  |
| `allowallresourcesfortasks` | checkbox | Display All Resources for ProjectTask Assignment |  |
| `allowexpenses` | checkbox | Allow Expenses |  |
| `allowtasktimeforrsrcalloc` | checkbox | Allow Allocated Resources to Enter Time to All Tasks |  |
| `allowtime` | checkbox | Allow Time Entry |  |
| `attention` | text | Attention |  |
| `billingschedule` | select | Billing Schedule |  |
| `bounced` | checkbox | Hard Bounced |  |
| `city` | text | City |  |
| `comments` | textarea | Comments |  |
| `country` | select | Country |  |
| `county` | text | County/State |  |
| `datecreated` | datetime | Date Created |  |
| `email` | text | Email |  |
| `entityid` | text | Name/ID |  |
| `estcost` | currency | Estimated Cost |  |
| `estenddate` | date | Projected End Date |  |
| `estimatedgrossprofit` | currency | Estimated Gross Profit |  |
| `estimatedgrossprofitpercent` | percent | Estimated Gross Profit % |  |
| `estimatedlaborcost` | currency | Estimated Labor Cost |  |
| `externalid` | select | External ID |  |
| `externalidstring` | text | External ID (Text) |  |
| `fax` | phone | Fax |  |
| `formuladate` | date | Formula (Date) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulatext` | text | Formula (Text) |  |
| `giveaccess` | checkbox | Login Access |  |
| `globalsubscriptionstatus` | select | Global Subscription Status |  |
| `image` | image | Image |  |
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
| `laborbudgetfromallocations` | checkbox | Calculate Labor Budgets from Resource Allocations |  |
| `language` | select | Language |  |
| `lastmodifieddate` | datetime | Last Modified |  |
| `level` | select | Level |  |
| `limittimetoassignees` | checkbox | Limit Time and Expenses To Resources |  |
| `materializetime` | checkbox | Create Planned Time Entries |  |
| `permission` | select | Permission |  |
| `phone` | phone | Phone |  |
| `phoneticname` | text | Phonetic Name |  |
| `representingsubsidiary` | select | Represents Subsidiary |  |
| `startdate` | date | Start Date |  |
| `state` | select | State/Province (Deprecated) |  |
| `subscription` | select | Subscription |  |
| `subscriptiondate` | date | Subscription Date |  |
| `subscriptionstatus` | checkbox | Subscription Status |  |
| `subsidiary` | select | Subsidiary |  |
| `type` | select | Entity Type |  |
| `useallocatedtimeforforecast` | checkbox | Use Allocated Time for Forecast |  |
| `usecalculatedbillingbudget` | checkbox | Use Calculated Values for all Billing Budgets |  |
| `usecalculatedcostbudget` | checkbox | Use Calculated Values for all Cost Budgets |  |
| `zipcode` | text | Zip |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `address` | text | Address |  |
| `addressee` | text | Addressee |  |
| `addressinternalid` | integer | Address Internal ID |  |
| `addresslabel` | text | Address Label |  |
| `addressphone` | text | Address Phone |  |
| `allowallresourcesfortasks` | checkbox | Display All Resources for ProjectTask Assignment |  |
| `allowexpenses` | checkbox | Allow Expenses |  |
| `allowtasktimeforrsrcalloc` | checkbox | Allow Allocated Resources to Enter Time to All Tasks |  |
| `allowtime` | checkbox | Allow Time Entry |  |
| `altemail` | email | Alt. Email |  |
| `altname` | text | Name |  |
| `altphone` | phone | Office Phone |  |
| `attention` | text | Attention |  |
| `bbudgetusecalculatedvalues` | checkbox | Use Calculated Values for all Billing Budgets |  |
| `billcountrycode` | text | Billing Country Code |  |
| `billingschedule` | select | Billing Schedule |  |
| `billzipcode` | text | Billing Zip |  |
| `bounced` | checkbox | Hard Bounced |  |
| `cbudgetlaborbudgetfromalloc` | checkbox | Calculate Labor Budgets from Resource Allocations |  |
| `cbudgetusecalculatedvalues` | checkbox | Use Calculated Values for all Cost Budgets |  |
| `city` | text | City |  |
| `comments` | text | Comments |  |
| `companyname` | text | Job Name |  |
| `country` | select | Country |  |
| `countrycode` | text | Country Code |  |
| `datecreated` | datetime | Date Created |  |
| `email` | email | Email |  |
| `entityid` | text | Name |  |
| `entitynumber` | integer | Number |  |
| `estimatedcost` | currency | Estimated Cost |  |
| `estimatedgrossprofit` | currency | Estimated Gross Profit |  |
| `estimatedgrossprofitpercent` | percent | Estimated Gross Profit % |  |
| `estimatedlaborcost` | currency | Estimated Labor Cost |  |
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
| `language` | select | Language |  |
| `lastmodifieddate` | datetime | Last Modified |  |
| `lastviewed` | datetime | Last Viewed |  |
| `level` | select | Level |  |
| `limittimetoassignees` | checkbox | Limit Time and Expenses to Resources |  |
| `materializetime` | checkbox | Create Planned Time Entries |  |
| `permission` | select | Permission |  |
| `phone` | phone | Phone |  |
| `phoneticname` | text | Phonetic Name |  |
| `representingsubsidiary` | select | Represents Subsidiary |  |
| `shipcountrycode` | text | Shipping Country Code |  |
| `startdate` | date | Start Date |  |
| `state` | select | State/Province |  |
| `statedisplayname` | select | State/Province Display Name |  |
| `subscription` | select | Subscription |  |
| `subscriptiondate` | datetime | Subscription Date |  |
| `subscriptionstatus` | checkbox | Subscription Status |  |
| `subsidiary` | select | Subsidiary |  |
| `subsidiarynohierarchy` | select | Subsidiary (no hierarchy) |  |
| `type` | select | Type |  |
| `useallocatedtimeforforecast` | checkbox | Use Allocated Time for Forecast |  |
| `zipcode` | text | Zip Code |  |

## Search joins

| Join ID | Label | Joined record |
|---|---|---|
| `genericresource` | Generic Resource | Generic Resource |
| `projecttask` | Project Task | Project Task |
| `rsrcallocation` | Resource Allocation | Resource Allocation |
