# Time Entry (`timeentry`)
<!-- id: record:timeentry -->

| Fact | Value |
|---|---|
| Internal ID | `timeentry` |
| Subrecord | yes |
| Tags | `custom_fields_supported` |

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `approvalstatus` | select |  | no | no |  | `purpose`: `approval_status` |
| `billingclass` | text | Billing Class | no | no |  | `purpose`: `billing_class`; `default_behavior`: `employee_billing_class` |
| `casetaskevent` | select | Case/Task/Event | no | no |  | `purpose`: `related_activity`; `effect`: `time_added_to_selected_activity` |
| `class` | select | Class | no | no |  | `purpose`: `class`; `ui_effect`: `new_option_creates_record` |
| `customer` | select | Customer | no | no |  | `purpose`: `customer_or_project`; `requires`: `service_item` (when `customer_selected`); `ui_effect`: `new_option_creates_record` |
| `day` | date | Date | no | no |  | `purpose`: `entry_date` |
| `department` | select | Department | no | no |  | `purpose`: `department`; `ui_effect`: `new_option_creates_record` |
| `employee` | select | Employee | yes | no |  | `purpose`: `employee`; `ui_effect`: `new_option_creates_record` |
| `hours` | timetrack | Duration | yes | no |  | `purpose`: `duration`; `value_source`: `start_end_and_break_times`; `ui_effect`: `timer_link_times_work` |
| `isbillable` | checkbox | Billable | no | no |  | `purpose`: `billable` |
| `item` | select | Service Item | no | no |  | `purpose`: `service_item`; `requires`: `customer_or_project` (when `item_selected`); `ui_effect`: `new_option_creates_record` |
| `location` | select | Location | yes | no |  | `purpose`: `location`; `ui_effect`: `new_option_creates_record` |
| `memo` | textarea | Memo | no | no |  | `purpose`: `memo`; `displayed_on`: `invoice_line_description` (when `billed_to_customer`) |
| `overriderate` | text | Lock this Rate | no | no |  | `purpose`: `lock_rate`; `effect`: `rate_unaffected_by_later_rate_changes`; `caveat`: `charge_rules_discard_entered_rates`; `recommendation`: `leave_blank_for_payroll_items` |
| `paidexternally` | checkbox | Paid Externally | no | no |  | `purpose`: `paid_externally`; `effect`: `removed_from_payroll_but_billable` |
| `payrollitem` | select | Payroll Item | no | no |  | `purpose`: `payroll_item`; `effect`: `selects_pay_rate` |
| `price` | select | Price Level | no | no |  | `purpose`: `price_level`; `default_behavior`: `customer_custom_price_level_or_sales_price_1`; `special_values`: `Custom`; `caveat`: `charge_rules_override_on_charge_based_projects`; `recommendation`: `leave_blank_for_payroll_items` |
| `rate` | text | Rate | no | no |  | `purpose`: `rate`; `value_source`: `price_level`; `caveat`: `charge_rules_override_on_charge_based_projects`; `recommendation`: `leave_blank_for_payroll_items` |
| `subsidiary` | select | Subsidiary | no | no |  | `purpose`: `subsidiary`; `value_source`: `employee_subsidiary` |
| `timetype` | select | Type | no | no |  | `purpose`: `time_type`; `allowed_values`: `Actual`, `Planned`, `Allocated`; `option_meanings`: `actual:worked`, `planned:project_planning`, `allocated:resource_allocation` |
| `createddate` | datetime |  | no | no |  |  |
| `externalid` | text | ExternalId | no | no |  |  |
| `isexempt` | checkbox | Exempt | no | no |  |  |
| `isproductive` | checkbox | Productive | no | no |  |  |
| `isutilized` | checkbox | Utilized | no | no |  |  |
| `projecttaskassignment` | select |  | no | no |  |  |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `approvalstatus` | select | Approval Status |  |
| `billable` | checkbox | Billable |  |
| `billingclass` | select | Billing Class |  |
| `billingstatus` | checkbox | Billing Status |  |
| `class` | select | Class |  |
| `customer` | select | Customer:Job |  |
| `date` | date | Date |  |
| `datecreated` | datetime | Date Created |  |
| `department` | select | Department |  |
| `duration` | float | Duration |  |
| `employee` | select | Employee |  |
| `exempt` | checkbox | Exempt |  |
| `externalid` | select | External ID |  |
| `externalidstring` | text | External ID (Text) |  |
| `formuladate` | date | Formula (Date) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulatext` | text | Formula (Text) |  |
| `internalid` | select | Internal ID |  |
| `internalidnumber` | integer | Internal ID (Number) |  |
| `ischarged` | checkbox | Charged |  |
| `item` | select | Item |  |
| `lastmodified` | datetime | Last Modified |  |
| `location` | select | Location |  |
| `memo` | text | Memo |  |
| `nextapprover` | select | Next Approver |  |
| `paidbypayroll` | checkbox | Paid by Payroll |  |
| `paidexternally` | checkbox | Paid Externally |  |
| `payitem` | select | Payroll Item |  |
| `productive` | checkbox | Productive |  |
| `subsidiary` | select | Subsidiary |  |
| `type` | select | Type |  |
| `utilized` | checkbox | Utilized |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `approvalstatus` | select | Approval Status |  |
| `billingclass` | select | Billing Class |  |
| `billingstatus` | text | Billing Status |  |
| `break` | text | Break |  |
| `casetaskevent` | select | Case/Task/Event |  |
| `chargebillingrunid` | integer | Charge Billing Run ID |  |
| `class` | select | Class |  |
| `classnohierarchy` | select | Class (no hierarchy) |  |
| `customer` | select | Customer:Job |  |
| `date` | date | Date |  |
| `datecreated` | date | Date Created |  |
| `department` | select | Department |  |
| `departmentnohierarchy` | select | Department (no hierarchy) |  |
| `durationdecimal` | float | Duration (Decimal) |  |
| `employee` | select | Employee |  |
| `endtime` | timeofday | End Time |  |
| `externalid` | select | External ID |  |
| `formulacurrency` | currency | Formula (Currency) |  |
| `formuladate` | date | Formula (Date) |  |
| `formuladatetime` | datetime | Formula (Date/Time) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulapercent` | percent | Formula (Percent) |  |
| `formulatext` | text | Formula (Text) |  |
| `hours` | text | Duration |  |
| `internalid` | integer | Internal ID |  |
| `isbillable` | checkbox | Billable |  |
| `isexempt` | checkbox | Exempt |  |
| `isproductive` | checkbox | Productive |  |
| `isutilized` | checkbox | Utilized |  |
| `item` | text | Item |  |
| `lastmodified` | date | Last Modified |  |
| `location` | select | Location |  |
| `locationnohierarchy` | select | Location (no hierarchy) |  |
| `memo` | text | Note |  |
| `nextapprover` | select | Next Approver |  |
| `paidexternally` | checkbox | Paid Externally |  |
| `payitem` | select | Payroll Item |  |
| `payrolldate` | date | Payroll Date |  |
| `rate` | currency | Rate |  |
| `starttime` | timeofday | Start Time |  |
| `subsidiary` | text | Subsidiary |  |
| `subsidiarynohierarchy` | text | Subsidiary (no hierarchy) |  |
| `timesheet` | url | Timesheet |  |
| `type` | select | Type |  |

## Search joins

| Join ID | Label | Joined record |
|---|---|---|
| `billingclass` | BillingClass | Billing Class |
| `customer` | Customer | Customer |
| `department` | Department | Department |
| `employee` | Employee | Employee |
| `job` | Job | Job |
| `location` | Location | Location |
| `projecttask` | Project Task | Project Task |
| `projecttaskassignment` | Project Task Assignment | Project Task Assignment |
| `task` | Task | Task |
| `timesheet` | Timesheet | Timesheet |
| `user` | User | Employee |
| `vendor` | Vendor | Vendor |
