# Employee (`employee`)
<!-- id: record:employee -->

| Fact | Value |
|---|---|
| Internal ID | `employee` |
| Category | entity |
| Client scripts | yes |
| Server scripts | yes |
| Tags | `custom_fields_supported`, `supports_create`, `supports_delete`, `supports_read`, `supports_update` |

## Record facts

- `script_types`: `client`, `server`
- `not_supported`: `copy`, `search`, `transform`
- `script_functions`: `create`, `read`, `update`, `delete`
- `not_supported`: `copy`, `search`, `transform`
- `sublist_requires_feature`: `subscriptions:marketing_automation`, `subscriptions:subscription_categories`
- `unreadable_in_before_submit_for_external_roles`: `password`, `password2`
- `rule`: `before_submit_sees_only_fields_role_may_edit` (when `advanced_employee_permissions`)
- `recommendation`: `after_submit_as_admin_for_full_access` (when `advanced_employee_permissions`)
- `doc_inconsistency`: `transform_listed_unsupported_but_transform_targets_given`
- `value_examples`: `transform_to_expensereport`, `transform_to_timebill`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `accountnumber` | text | Account | no | yes |  | `purpose`: `employee_account_number`; `required_when`: `online_bill_pay_feature` |
| `aliennumber` | text | Alien Number | no | yes |  | `purpose`: `alien_number`; `required_when`: `work_status_alien_authorized` |
| `approvallimit` | currency | Expense Approval Limit | no | yes |  | `purpose`: `expense_approval_limit`; `effect`: `larger_expenses_need_other_approver` |
| `approver` | select | Expense Approver | no | yes |  | `purpose`: `expense_approver`; `default_behavior`: `supervisor_approves` (when `blank`); `effect`: `supervisor_removed_from_hierarchy` |
| `authworkdate` | date | Authorized to work until | no | yes |  | `purpose`: `work_authorization_end_date`; `required_when`: `work_status_alien_authorized` |
| `autoname` | checkbox | Auto | no | no |  | `purpose`: `auto_name`; `true_means`: `id_from_auto_generated_numbers`; `false_means`: `enter_id_manually`; `configured_by`: `auto_generated_numbers_setup` |
| `billingclass` | select | Billing Class | no | yes |  | `purpose`: `billing_class`; `effect`: `sets_default_billable_time_pricing`; `allowed_values_listed_in`: `accounting_list_billing_class` |
| `billpay` | checkbox | Enable Online Bill Pay | no | no |  | `purpose`: `pay_employee_online`; `not_supported_for`: `paychecks`; `doc_inconsistency`: `unresolved_navigation_placeholders` |
| `birthdate` | date | Birth Date | no | yes |  | `purpose`: `birth_date` |
| `bonustarget` | float | Bonus Target | no | no |  | `purpose`: `bonus_target`; `accepts_forms`: `percentage_of_base_wage`, `amount` |
| `bonustargetcomment` | textarea | Bonus Target Comment | no | no |  | `purpose`: `bonus_target_comment` |
| `bonustargetpayfrequency` | select | Bonus Target Pay Frequency | no | no |  | `purpose`: `bonus_pay_frequency`; `allowed_values`: `one_time`, `annually`, `monthly`, `quarterly` |
| `bonustargettype` | select | Bonus Target Type | no | no |  | `purpose`: `bonus_target_type`; `allowed_values`: `Percentage`, `Amount`; `value_examples`: `10_with_percentage_means_10_percent` |
| `class` | select | Class | no | yes |  | `purpose`: `class`; `allowed_values_listed_in`: `class_records`; `ui_effect`: `new_option_creates_record` |
| `comments` | textarea | Notes | no | yes |  | `purpose`: `comments` |
| `commissionpaymentpreference` | select | Pay Commissions Using | no | no |  | `purpose`: `commission_payor`; `options`: `Payroll`, `Accounts Payable`, `system_preference` |
| `currency` | select | Currency | no | no |  | `purpose`: `employee_currency`; `default_behavior`: `subsidiary_or_company_base_currency`; `constraint`: `used_for_all_employee_transactions` |
| `customform` | select | Custom Form | yes | no |  | `purpose`: `custom_form`; `allowed_values_listed_in`: `custom_entry_forms`; `ui_effect`: `customize_form_creates_form` |
| `datecreated` | datetime | Date Created | no | no |  | `purpose`: `date_time_created` |
| `defaultacctcorpcardexp` | select | Default Account for Corporate Card Expenses | no | no |  | `purpose`: `default_corporate_card_account` |
| `defaultaddress` | address | Address | no | no |  | `purpose`: `employee_address`; `value_source`: `address_sublist` |
| `defaultexpensereportcurrency` | select | Default Currency for Expense Reports | yes | no |  | `purpose`: `default_expense_report_currency` |
| `department` | select | Department | no | yes |  | `purpose`: `department`; `allowed_values_listed_in`: `department_records`; `ui_effect`: `new_option_creates_record` |
| `directdeposit` | checkbox | Use Direct Deposit | no | no |  | `purpose`: `direct_deposit`; `effect`: `real_time_transfer_with_immediate_accounting` |
| `eligibleforcommission` | checkbox | Eligible for Commission | no | no |  | `purpose`: `eligible_for_commission` |
| `email` | email | Email | no | no |  | `purpose`: `email_address`; `effect`: `part_of_employee_login` |
| `employeestatus` | select | Employee Status | no | yes |  | `purpose`: `employee_status`; `allowed_values_listed_in`: `employee_related_lists` |
| `employeetype` | select | Type | no | yes |  | `purpose`: `employee_type`; `effect`: `controls_inclusion_in_employee_kpis`; `allowed_values_listed_in`: `employee_related_lists` |
| `entityid` | text | Employee ID | yes | yes |  | `purpose`: `employee_id`; `value_source`: `auto_generated_number` (when `autoname`); `requires`: `manual_id` (when `autoname_cleared`) |
| `ethnicity` | select | Ethnicity | no | yes |  | `purpose`: `ethnicity`; `allowed_values_listed_in`: `employee_related_lists` |
| `expenselimit` | currency | Expense Limit | no | yes |  | `purpose`: `expense_limit_without_approval` |
| `externalid` | text | ExternalId | no | no |  | `purpose`: `external_id` |
| `fax` | phone | Fax | no | yes |  | `purpose`: `fax_number`; `format`: `as_dialed_including_prefix`; `ui_effect`: `fills_to_be_faxed_on_transactions`; `value_examples`: `leading_1_when_required`; `requires`: `fax_service_set_up` (when `faxing_forms`) |
| `firstname` | text | Name | no | yes |  | `purpose`: `first_name`; `effect`: `first_part_of_employee_name_and_initials`; `required_when`: `online_bill_pay_feature` |
| `gender` | select | Gender | no | yes |  | `purpose`: `gender` |
| `giveaccess` | checkbox | Give Access | no | no |  | `purpose`: `give_netsuite_access`; `requires`: `role_assignment`; `use_when`: `employee_center_for_time_and_expenses` |
| `globalsubscriptionstatus` | select | Global Subscription Status | no | no |  | `purpose`: `global_subscription_status`; `allowed_values`: `Confirmed Opt-In`, `Soft Opt-In`, `Soft Opt-Out`, `Confirmed Opt-Out`; `constraint`: `only_recipient_sets_confirmed_statuses`; `can`: `set_soft_statuses_manually_or_mass_update`; `option_meanings`: `confirmed_opt_in:recipient_wants_campaigns`, `soft_opt_in:receives_opt_in_messages`, `soft_opt_out:no_campaigns_opt_in_messages_ok`, `confirmed_opt_out:no_campaigns_or_opt_in_messages`; `caveat`: `re_opt_in_only_via_employee_center_or_old_link` |
| `hasofflineaccess` | checkbox | Offline Client Access | no | no |  | `purpose`: `offline_client_access`; `requires`: `sales_role`; `allowed_values`: `Sales Person`, `Sales Manager`, `Sales Administrator`, `custom_roles_with_sales_role` [`sales_roles`]; `can`: `download_client_from_preferences` |
| `hiredate` | date | Hire Date | yes | yes |  | `purpose`: `hire_date` |
| `homephone` | phone | Home Phone | no | yes |  | `purpose`: `home_phone` |
| `i9verified` | checkbox | I-9 Verified | no | yes |  | `purpose`: `i9_verified` |
| `image` | select | Image | no | no |  | `purpose`: `employee_image`; `value_source`: `file_cabinet_image`; `ui_effect`: `new_option_creates_record` |
| `inheritiprules` | checkbox | Inherit IP Rules from Company | yes | no |  | `purpose`: `inherit_company_ip_rules`; `false_means`: `login_only_from_ipaddressrule`; `true_means`: `login_from_company_ip_addresses`; `caveat`: `ipaddressrule_overrides_inherited_rules` |
| `initials` | text | Initials | no | no |  | `purpose`: `initials`; `default_behavior`: `built_from_first_middle_last_names`; `max_length`: `3` characters |
| `ipaddressrule` | textarea | IP Address Restriction | yes | no |  | `purpose`: `ip_address_restriction`; `accepts_forms`: `single_ip`, `dash_range`, `space_or_comma_list`, `ip_with_mask`, `ALL`; `max_length`: `4000` characters; `default_behavior`: `company_ip_rules` (when `blank`); `value_examples`: `209.209.48.32/255.255.0.0` |
| `isinactive` | checkbox | Inactive | no | yes |  | `purpose`: `inactivate_employee`; `ui_effect`: `hidden_from_all_lists_and_choices`; `ui_effect`: `show_inactives_lists_and_reactivates` |
| `isjobmanager` | checkbox | Project Manager | no | no |  | `purpose`: `project_manager`; `effect`: `listed_as_project_manager_choice`; `editable_only_when`: `isjobresource` |
| `isjobresource` | checkbox | Project Resource | no | yes |  | `purpose`: `project_resource`; `effect`: `assignable_to_projects_and_tasks` |
| `issalesrep` | checkbox | Sales Rep | no | yes |  | `purpose`: `sales_rep`; `effect`: `listed_in_sales_rep_and_territory_lists` |
| `issupportrep` | checkbox | Support Rep | no | yes |  | `purpose`: `support_rep`; `effect`: `listed_in_case_assignee_and_territory_lists` |
| `job` | select | Job | no | no |  | `purpose`: `job`; `allowed_values_listed_in`: `hcm_job_records` |
| `jobdescription` | textarea | Job Description | no | no |  | `purpose`: `job_description` |
| `laborcost` | currency | Labor Cost | no | yes |  | `purpose`: `hourly_labor_cost`; `effect`: `used_for_estimated_job_cost` |
| `lastmodifieddate` | datetime | Last Modified Date | no | no |  | `purpose`: `date_last_modified` |
| `lastname` | text |  | no | yes |  | `purpose`: `last_name`; `effect`: `last_part_of_employee_name_and_initials`; `required_when`: `online_bill_pay_feature` |
| `lastpaiddate` | date | Last Paid Date | no | yes |  | `purpose`: `last_paid_date`; `value_kind`: `pay_run_date_not_check_date`; `effect`: `updated_on_each_payroll_run`; `value_examples`: `pay_run_oct_7_check_oct_13_enter_oct_7`; `caveat`: `enter_once_then_payroll_updates` |
| `lastreviewdate` | date | Last Review Date | no | yes |  | `purpose`: `last_review_date` |
| `location` | select | Location | no | yes |  | `purpose`: `work_location`; `required_when`: `paid_with_netsuite_payroll`; `allowed_values_listed_in`: `location_records`; `ui_effect`: `new_option_creates_record`; `requires`: `staffed_location` (when `paid_with_netsuite_payroll`) |
| `maritalstatus` | select | Marital Status | no | yes |  | `purpose`: `marital_status`; `allowed_values_listed_in`: `employee_related_lists` |
| `middlename` | text |  | no | yes |  | `purpose`: `middle_name`; `effect`: `middle_part_of_employee_name_and_initials`; `max_length`: `25` characters |
| `mobilephone` | phone | Mobile Phone | no | yes |  | `purpose`: `mobile_phone` |
| `nextreviewdate` | date | Next Review Date | no | yes |  | `purpose`: `next_review_date` |
| `officephone` | phone | Office Phone | no | no |  | `purpose`: `office_phone` |
| `payfrequency` | select | Pay Frequency | yes | yes |  | `purpose`: `pay_frequency` |
| `phone` | phone | Phone | no | yes |  | `purpose`: `phone`; `required_when`: `online_bill_pay_feature` |
| `phoneticname` | text | Furigana | no | no |  | `purpose`: `furigana_sort_name` |
| `purchaseorderapprovallimit` | currency | Purchase Approval Limit | no | yes |  | `purpose`: `purchase_approval_limit` |
| `purchaseorderapprover` | select | Purchase Approver | no | yes |  | `purpose`: `purchase_approver`; `default_behavior`: `supervisor_approves` (when `blank`); `effect`: `supervisor_removed_from_hierarchy` |
| `purchaseorderlimit` | currency | Purchase Limit | no | yes |  | `purpose`: `purchase_limit_without_approval` |
| `releasedate` | date | Termination Date | no | yes |  | `purpose`: `release_date` |
| `requirepwdchange` | checkbox | Require Password Change On Next Login | no | no |  | `purpose`: `require_password_change`; `effect`: `forces_password_change_at_next_login`; `caveat`: `never_reads_back_as_checked`; `effect`: `save_sets_internal_flag_cleared_after_change` |
| `residentstatus` | select | Resident Status | no | yes |  | `purpose`: `resident_status`; `value_source`: `i9_form_section_1` |
| `salesrole` | select | Sales Role | no | yes |  | `purpose`: `sales_role`; `allowed_values_listed_in`: `crm_list_sales_role` |
| `salutation` | text | Mr./Ms... | no | yes |  | `purpose`: `salutation`; `value_examples`: `Mr.`, `Ms.` |
| `sendemail` | checkbox | Send Notification Email | no | no |  | `purpose`: `send_access_email`; `effect`: `emails_access_notice_with_password_link`; `false_means`: `password_must_be_assigned_manually` |
| `socialsecuritynumber` | ssnumber | Social Security | no | yes |  | `purpose`: `social_security_number`; `format`: `nnn_nn_nnnn` |
| `startdatetimeoffcalc` | date | Start Date for Time-Off Calculations | no | no |  | `purpose`: `time_off_tenure_start_date`; `required_when`: `assigning_time_off_plan` |
| `subsidiary` | select | Subsidiary | yes | no |  | `purpose`: `employee_subsidiary`; `requires`: `set_before_entering_transactions`; `effect`: `time_transactions_associated_with_subsidiary`; `cannot`: `change_after_transaction_posted` |
| `supervisor` | select | Supervisor | no | yes |  | `purpose`: `supervisor`; `effect`: `approves_expenses_and_purchases` |
| `targetutilization` | percent | Target Utilization | no | yes |  | `purpose`: `target_utilization`; `effect`: `reduces_available_hours_for_utilization`; `caveat`: `does_not_change_actual_availability` |
| `timeapprover` | select | Time Approver | no | yes |  | `purpose`: `time_approver`; `default_behavior`: `supervisor_approves` (when `blank`); `effect`: `only_time_approver_approves_when_both_set` |
| `timeoffplan` | select | Time-off plan | no | no |  | `purpose`: `time_off_plan` |
| `title` | text | Job Title | no | yes |  | `purpose`: `job_title` |
| `unsubscribe` | select | Unsubscribe from Campaigns | no | no |  | `purpose`: `unsubscribed_from_campaigns`; `true_means`: `no_campaign_email` |
| `useperquest` | checkbox | Include in Payroll | no | yes |  | `purpose`: `include_in_payroll`; `requires`: `address_hire_date_and_ssn` |
| `usetimedata` | select | Compensation Type | no | yes |  | `purpose`: `wage_or_salary`; `allowed_values`: `Wage`, `Salary` |
| `visaexpdate` | date | Visa Expiration Date | no | yes |  | `purpose`: `visa_expiration_date`; `value_source`: `employee_passport` |
| `visatype` | select | Visa Type | no | yes |  | `purpose`: `visa_type`; `doc_inconsistency`: `unresolved_navigation_placeholders` |
| `workassignment` | radio | Job | no | no |  | `purpose`: `work_assignment`; `allowed_values`: `job`, `position` |
| `workcalendar` | select | Work Calendar | yes | yes |  | `purpose`: `work_calendar` |
| `workplace` | select | Workplace | no | yes |  | `purpose`: `workplace`; `effect`: `address_defines_withholding_taxes` |
| `btemplate` | text | Employee is Template | no | no |  |  |
| `depmedicalinsavailability` | checkbox | Dependent Medical Insurance Availability | no |  |  |  |
| `depmedicalinsenrolled` | checkbox | Dependent Medical Insurance Enrolled | no |  |  |  |
| `empcenterqty` | text | EmpCenter current | no | no |  |  |
| `empcenterqtymax` | text | EmpCenter max | no | no |  |  |
| `fulluserqty` | text | FullUser current | no | no |  |  |
| `fulluserqtymax` | text | FullUser max | no | no |  |  |
| `isempcenterqtyenforced` | text | EmpCenter enforce | no | no |  |  |
| `isfulluserqtyenforced` | text | FullUser enforce | no | no |  |  |
| `isretailuserqtyenforced` | text | RetailUser enforce | no | no |  |  |
| `jurisdiction1display` | text | Federal: | no | no |  |  |
| `jurisdiction2display` | text | State: | no | no |  |  |
| `jurisdiction3display` | text | County: | no | no |  |  |
| `jurisdiction4display` | text | Local: | no | no |  |  |
| `jurisdiction5display` | text | School District: | no | no |  |  |
| `medicalinsavailability` | checkbox | Medical Insurance Availability | no |  |  |  |
| `medicalinseligibilitydate` | date | Medical Insurance Eligibility Date | no |  |  |  |
| `medicalinsenrolled` | checkbox | Medical Insurance Enrolled | no |  |  |  |
| `retailuserqty` | text | RetailUser current | no | no |  |  |
| `retailuserqtymax` | text | RetailUser max | no | no |  |  |
| `strength` | text | Password Strength | no | no |  |  |
| `terminationbydeath` | checkbox | Termination Due To Death | no | no |  |  |
| `wasempcenterhasaccess` | text | EmpCenter hasaccess | no | no |  |  |
| `wasfulluserhasaccess` | text | FullUser hasaccess | no | no |  |  |
| `wasinactive` | text | wasinactive | no | no |  |  |
| `wasretailuserhasaccess` | text | RetailUser hasaccess | no | no |  |  |
| `password` |  | Password |  |  |  |  |
| `password2` |  | Confirm Password |  |  |  |  |

## Sublist `accruedtime` (Accrued Time)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `accrualmethod` | select | Accrue As | no |  |  |
| `accrualrate` | float | Accrual Rate | no |  |  |
| `accruedhours` | float | Accrued Hours | no |  |  |
| `inactive` | checkbox | Inactive | no |  |  |
| `maximumaccruedhours` | float | Maximum Hours | no |  |  |
| `monetaryrate` | currency | Monetary Rate | no |  |  |
| `payrollitem` | select | Sick/Vacation Time | yes |  |  |
| `resetaccruedhoursatyearend` | checkbox | Reset at Year End | no |  |  |

## Sublist `addressbook` (Address)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `addressbookaddress` | summary | Edit | no |  |  |
| `addressid` | text |  | no |  |  |
| `defaultbilling` | checkbox | Home | no |  |  |
| `defaultshipping` | checkbox | Default Shipping | no |  |  |
| `id` | integer | ID | no |  |  |
| `internalid` | integer |  | no |  |  |
| `label` | text | Label | no |  |  |

## Sublist `companycontribution` (Company Contributions)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `inactive` | checkbox | Inactive | no |  |  |
| `limit` | currency | Limit | no |  |  |
| `payrollitem` | select | Company Contribution | yes |  |  |
| `rate` | rate | Rate | no |  |  |

## Sublist `corporatecards` (Corporate Cards)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `corporatecardprofile` | select | Corporate Card Profile | no |  |  |
| `embossedname` | text | Name on Card | yes |  |  |
| `expiration` | date | Expiration Date | no |  |  |

## Sublist `currency` (Currencies)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `currency` | select | Currency | yes |  |  |

## Sublist `deduction` (Deductions)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `inactive` | checkbox | Inactive | no |  |  |
| `limit` | currency | Limit | no |  |  |
| `payrollitem` | select | Deduction | yes |  |  |
| `rate` | rate | Rate | no |  |  |

## Sublist `directdeposit` (Accounts)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `accountprenoted` | checkbox | Prenoted | no |  |  |
| `amount` | currency | Amount | no |  |  |
| `bankaccountnumber` | text | Account Number | yes |  |  |
| `bankname` | text | Bank Name | yes |  |  |
| `bankroutingnumber` | text | Routing Number | yes |  |  |
| `id` | text | ID | no |  |  |
| `inactive` | checkbox | Inactive | no |  |  |
| `netaccount` | checkbox | Net Account | no |  |  |
| `savingsaccount` | checkbox | Savings Account | no |  |  |

## Sublist `earning` (Earnings)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `defaultearning` | checkbox | Default | no |  |  |
| `defaulthours` | float | Default Hours | no |  |  |
| `inactive` | checkbox | Inactive | no |  |  |
| `payrate` | rate | Rate | no |  |  |
| `payrollitem` | select | Earning | yes |  |  |
| `primaryearning` | checkbox | Primary | no |  |  |

## Sublist `hcmposition` (Positions)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `employmentcategory` | select | Employment Category | no |  |  |
| `fulltimeequivalent` | float | Full Time Equivalent | no |  |  |
| `location` | select | Location | no |  |  |
| `position` | select | Position | yes |  |  |
| `positionallocation` | float | Allocated Position FTE | no |  |  |
| `positionid` | text | Position ID | no |  |  |
| `primaryposition` | checkbox | Primary | yes |  |  |
| `reportsto` | select | Reports To | no |  |  |
| `subsidiary` | select | Subsidiary | yes |  |  |

## Sublist `rates` (Rates)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `entitycurrency` | select | Currency | yes |  |  |
| `rate` | currency | Hourly Rate | yes |  |  |

## Tabs

| Tab ID | Label |
|---|---|
| `access` | Access |
| `address` | Address |
| `commission` | Commission |
| `curjurisdictions` | Current |
| `custom` | Custom |
| `dd` | ACH/Direct Deposit |
| `education` | Education |
| `general` | General |
| `hr` | Human Resources |
| `hrpersonal` | Personal |
| `hrworkstatus` | Work Status |
| `info` | Info |
| `jurisdictions` | Jurisdictions |
| `marketing` | Marketing |
| `payfed` | Federal Withholdings |
| `payroll` | Payroll |
| `support` | Support |
| `workflow` | Workflow |

## Transform targets

| Target record | Default fields |
|---|---|
| `expensereport` | `customform` |
| `timebill` | `customform` |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `address` | textarea | Address |  |
| `addressee` | text | Addressee |  |
| `addresslabel` | text | Address Label |  |
| `addressphone` | phone | Address Phone |  |
| `aliennumber` | text | Alien Number |  |
| `allocation` | percent | Allocation |  |
| `anniversary` | date | Anniversary |  |
| `approvallimit` | currency | Expense Approval Limit |  |
| `approver` | select | Expense Approver |  |
| `attention` | text | Attention |  |
| `authworkdate` | date | Authorized to work until date |  |
| `basewage` | currency | Base Wage |  |
| `basewagetype` | select | Base Wage Type |  |
| `billingclass` | select | Billing Class |  |
| `birthdate` | date | Birth Date |  |
| `birthday` | date | Birthday |  |
| `bonustarget` | float | Bonus Target |  |
| `bonustargetcomment` | text | Bonus Target Comment |  |
| `bonustargetpayfrequency` | select | Bonus Target Pay Frequency |  |
| `bonustargettype` | select | Bonus Target Type |  |
| `ccontribution` | select | Company Contribution |  |
| `city` | text | City |  |
| `class` | select | Class |  |
| `comments` | textarea | Comments |  |
| `commissionplan` | select | Commission Plan |  |
| `compensationcurrency` | select | Currency |  |
| `concurrentwebservicesuser` | checkbox | Concurrent Web Services User |  |
| `corporatecardprofile` | select | Corporate Card Profile |  |
| `country` | select | Country |  |
| `county` | text | County/State |  |
| `datecreated` | datetime | Date Created |  |
| `deduction` | select | Deduction |  |
| `defaultacctcorpcardexp` | select | Default Account for Corporate Card Expenses |  |
| `defaultexpensereportcurrency` | select | Default Currency for Expense Reports |  |
| `department` | select | Department |  |
| `earning` | select | Earning |  |
| `education` | select | Education |  |
| `eligibleforcommission` | checkbox | Eligible For Commission |  |
| `email` | text | Email |  |
| `embossedname` | text | Name on Card |  |
| `employeestatus` | select | Employee Status |  |
| `employeetype` | select | Type |  |
| `employeetypekpi` | checkbox | Include in KPI |  |
| `entityid` | text | Name/ID |  |
| `ethnicity` | select | Ethnicity |  |
| `expenselimit` | currency | Expense Limit |  |
| `expensereportcurrency` | select | Expense Report Currency |  |
| `expiration` | date | Expiration Date |  |
| `externalid` | select | External ID |  |
| `externalidstring` | text | External ID (Text) |  |
| `fax` | phone | Fax |  |
| `firstname` | text | First Name |  |
| `formuladate` | date | Formula (Date) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulatext` | text | Formula (Text) |  |
| `gender` | checkbox | Gender |  |
| `giveaccess` | checkbox | Login Access |  |
| `globalsubscriptionstatus` | select | Global Subscription Status |  |
| `group` | multiselect | Group |  |
| `hiredate` | date | Hire Date |  |
| `i9verified` | checkbox | I9 Verified |  |
| `image` | image | Image |  |
| `internalid` | select | Internal ID |  |
| `internalidnumber` | integer | Internal ID (Number) |  |
| `isdefaultbilling` | checkbox | Default Billing Address |  |
| `isdefaultshipping` | checkbox | Default Shipping Address |  |
| `isinactive` | checkbox | Inactive |  |
| `isjobmanager` | checkbox | Job Manager |  |
| `isjobresource` | checkbox | Is Job Resource |  |
| `istemplate` | checkbox | Is Template |  |
| `job` | select | Job |  |
| `jobdescription` | text | Job Description |  |
| `laborcost` | currency | Labor Cost |  |
| `language` | select | Language |  |
| `lastmodifieddate` | datetime | Last Modified |  |
| `lastname` | text | Last Name |  |
| `lastpaiddate` | date | Last Paid Date |  |
| `lastreviewdate` | date | Last Review Date |  |
| `level` | select | Level |  |
| `location` | select | Location |  |
| `maritalstatus` | select | Marital Status |  |
| `middlename` | text | Middle Name |  |
| `nextreviewdate` | date | Next Review Date |  |
| `offlineaccess` | checkbox | Offline Access |  |
| `payfrequency` | select | Pay Frequency |  |
| `permchangedate` | datetime | Permission Change Date |  |
| `permission` | select | Permission |  |
| `permissionchange` | select | Permission Change |  |
| `phone` | phone | Phone |  |
| `phoneticname` | text | Phonetic Name |  |
| `positiontitle` | select | Position Title |  |
| `primaryearningamount` | currency | Primary Earning Amount |  |
| `primaryearningitem` | text | Primary Earning Item |  |
| `primaryearningtype` | text | Primary Earning Type |  |
| `purchaseorderapprovallimit` | currency | Purchase Approval Limit |  |
| `purchaseorderapprover` | select | Purchase Approver |  |
| `purchaseorderlimit` | currency | Purchase Limit |  |
| `releasedate` | date | Release Date |  |
| `representingsubsidiary` | select | Represents Subsidiary |  |
| `residentstatus` | select | Resident Status |  |
| `role` | select | Role |  |
| `rolechange` | select | Role Change |  |
| `rolechangedate` | datetime | Role Change Date |  |
| `salesrep` | checkbox | Is Sales Rep |  |
| `salesrole` | select | Sales Role |  |
| `salutation` | text | Mr./Mrs... |  |
| `socialsecuritynumber` | ssnumber | Social Security Number |  |
| `startdatetimeoffcalc` | date | Start Date for Time-Off Calculations |  |
| `state` | select | State/Province |  |
| `subsidiary` | select | Subsidiary |  |
| `supervisor` | select | Supervisor |  |
| `supportrep` | checkbox | Is Support Rep |  |
| `targetutilization` | percent | Target Utilization |  |
| `terminationcategory` | select | Termination Category |  |
| `terminationdetails` | text | Termination Details |  |
| `terminationreason` | select | Termination Reason |  |
| `terminationregretted` | select | Termination Regretted |  |
| `timeapprover` | select | Time Approver |  |
| `timeoffplan` | select | Time-Off Plan |  |
| `title` | text | Job Title |  |
| `useperquest` | checkbox | Include In US Payroll |  |
| `usetimedata` | checkbox | Use Time Data |  |
| `visaexpdate` | date | Visa Expiration Date |  |
| `visatype` | select | Visa Type |  |
| `withholding` | select | Withholding |  |
| `workcalendar` | select | Work Calendar |  |
| `workplace` | select | Workplace |  |
| `zipcode` | text | Zip |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `accountnumber` | text | Account |  |
| `address` | text | Address |  |
| `addressee` | text | Addressee |  |
| `addressinternalid` | integer | Address Internal ID |  |
| `addresslabel` | text | Address Label |  |
| `addressphone` | text | Address Phone |  |
| `aliennumber` | text | Alien # |  |
| `allocation` | float | Allocated Position FTE |  |
| `altemail` | email | Alt. Email |  |
| `altname` | text | Name |  |
| `altphone` | phone | Office Phone |  |
| `approvallimit` | currency | Expense Approval Limit |  |
| `approver` | select | Expense Approver |  |
| `attention` | text | Attention |  |
| `authworkdate` | date | Authorized to work until date |  |
| `basewage` | currency | Base Wage |  |
| `basewagetype` | select | Base Wage Type |  |
| `billcountrycode` | text | Billing Country Code |  |
| `billingclass` | select | Billing Class |  |
| `billzipcode` | text | Billing Zip |  |
| `birthdate` | date | Birth Date |  |
| `birthday` | date | Birth Day |  |
| `bonustarget` | currency | Bonus Target |  |
| `bonustargetcomment` | text | Bonus Target Comment |  |
| `bonustargetpayfrequency` | select | Bonus Target Pay Frequency |  |
| `bonustargettype` | select | Bonus Target Type |  |
| `city` | text | City |  |
| `class` | select | Class |  |
| `classnohierarchy` | select | Class (no hierarchy) |  |
| `comments` | text | Comments |  |
| `compensationcurrency` | select | Compensation Currency |  |
| `concurrentwebservicesuser` | checkbox | Concurrent Web Services User |  |
| `corporatecardprofile` | select | Corporate Card Profile |  |
| `country` | select | Country |  |
| `countrycode` | text | Country Code |  |
| `datecreated` | datetime | Date Created |  |
| `defaultacctcorpcardexp` | select | Default Account for Corporate Card Expenses |  |
| `defaultexpensereportcurrency` | select | Default Currency for Expense Reports |  |
| `department` | select | Department |  |
| `departmentnohierarchy` | select | Department (no hierarchy) |  |
| `eligibleforcommission` | checkbox | Eligible for Commission |  |
| `email` | email | Email |  |
| `embossedname` | text | Name on Card |  |
| `employeestatus` | select | Employee Status |  |
| `employeetype` | select | Type |  |
| `entityid` | text | Name |  |
| `entitynumber` | integer | Number |  |
| `ethnicity` | select | Ethnicity |  |
| `expenselimit` | currency | Expense Limit |  |
| `expensereportcurrency` | select | Expense Report Currency |  |
| `expiration` | date | Expiration Date |  |
| `externalid` | select | External ID |  |
| `fax` | text | Fax |  |
| `firstname` | text | First Name |  |
| `formulacurrency` | currency | Formula (Currency) |  |
| `formuladate` | date | Formula (Date) |  |
| `formuladatetime` | datetime | Formula (Date/Time) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulapercent` | percent | Formula (Percent) |  |
| `formulatext` | text | Formula (Text) |  |
| `gender` | select | Gender |  |
| `giveaccess` | checkbox | Login Access |  |
| `globalsubscriptionstatus` | select | Global Subscription Status |  |
| `hiredate` | date | Hire Date |  |
| `homephone` | phone | Home Phone |  |
| `i9verified` | checkbox | I9 Verified |  |
| `image` | select | Image |  |
| `internalid` | select | Internal ID |  |
| `isdefaultbilling` | checkbox | Default Billing Address |  |
| `isdefaultshipping` | checkbox | Default Shipping Address |  |
| `isinactive` | checkbox | Inactive |  |
| `isjobmanager` | checkbox | Job Manager |  |
| `isjobresource` | checkbox | Job Resource |  |
| `issalesrep` | checkbox | Sales Rep |  |
| `issupportrep` | checkbox | Support Rep |  |
| `istemplate` | checkbox | Is Template |  |
| `job` | select | Job |  |
| `laborcost` | currency | Labor Cost |  |
| `language` | select | Language |  |
| `lastmodifieddate` | datetime | Last Modified |  |
| `lastname` | text | Last Name |  |
| `lastpaiddate` | date | Last Paid Date |  |
| `lastreviewdate` | date | Last Review |  |
| `lastviewed` | datetime | Last Viewed |  |
| `level` | select | Level |  |
| `location` | select | Location |  |
| `locationnohierarchy` | select | Location (no hierarchy) |  |
| `maritalstatus` | select | Marital Status |  |
| `middlename` | text | Middle Name |  |
| `mobilephone` | phone | Mobile Phone |  |
| `nextreviewdate` | date | Next Review |  |
| `offlineaccess` | checkbox | Offline Access |  |
| `payfrequency` | select | Pay Frequency |  |
| `permchangedate` | datetime | Permission Change Date |  |
| `permchangelevel` | text | Permission Change Level |  |
| `permission` | select | Permission |  |
| `permissionchange` | text | Permission Change |  |
| `phone` | phone | Phone |  |
| `phoneticname` | text | Phonetic Name |  |
| `positiontitle` | text | Position Title |  |
| `primaryearningamount` | currency | Primary Earning Amount |  |
| `primaryearningitem` | text | Primary Earning Item |  |
| `primaryearningtype` | text | Primary Earning Type |  |
| `purchaseorderapprovallimit` | currency | Purchase Approval Limit |  |
| `purchaseorderapprover` | select | Purchase Approver |  |
| `purchaseorderlimit` | currency | Purchase Limit |  |
| `releasedate` | date | Release Date |  |
| `representingsubsidiary` | select | Represents Subsidiary |  |
| `residentstatus` | select | Resident Status |  |
| `role` | select | Role |  |
| `rolechange` | text | Role Change |  |
| `rolechangeaction` | text | Role Change Action |  |
| `rolechangedate` | datetime | Role Change Date |  |
| `salesrole` | select | Sales Role |  |
| `salutation` | text | Mr./Mrs... |  |
| `shipcountrycode` | text | Shipping Country Code |  |
| `socialsecuritynumber` | ssnumber | Social Security # |  |
| `startdatetimeoffcalc` | date | Start Date for Time-Off Calculations |  |
| `state` | select | State/Province |  |
| `statedisplayname` | select | State/Province Display Name |  |
| `subscription` | select | Subscription |  |
| `subscriptiondate` | datetime | Subscription Date |  |
| `subscriptionstatus` | checkbox | Subscription Status |  |
| `subsidiary` | select | Subsidiary |  |
| `subsidiarynohierarchy` | select | Subsidiary (no hierarchy) |  |
| `supervisor` | select | Supervisor |  |
| `targetutilization` | percent | Target Utilization |  |
| `terminationcategory` | select | Termination Category |  |
| `terminationdetails` | text | Termination Details |  |
| `terminationreason` | select | Termination Reason |  |
| `terminationregretted` | select | Termination Regretted |  |
| `timeapprover` | select | Time Approver |  |
| `timeoffplan` | text | Time-Off Plan |  |
| `title` | text | Job Title |  |
| `useperquest` | checkbox | Include In US Payroll |  |
| `usetimedata` | checkbox | Use Time Data |  |
| `visaexpdate` | date | Visa Expiration Date |  |
| `visatype` | select | Visa Type |  |
| `workcalendar` | select | Work Calendar |  |
| `workplace` | select | Workplace |  |
| `zipcode` | text | Zip Code |  |

## Search joins

| Join ID | Label | Joined record |
|---|---|---|
| `campaignresponse` | Campaign Response | Campaign |
| `department` | Department | Department |
| `location` | Location | Location |
| `messages` | Messages | Message |
| `messagesfrom` | Messages From | Message |
| `messagesto` | Messages To | Message |
| `subsidiary` | Subsidiary | Subsidiary |

## Suspected source errors

> [!SOURCE-ERROR] `transform_listed_unsupported_but_transform_targets_given`
<!-- source-error: record:employee | - | transform_listed_unsupported_but_transform_targets_given -->

> [!SOURCE-ERROR] `unresolved_navigation_placeholders` (on `billpay`)
<!-- source-error: record:employee | billpay | unresolved_navigation_placeholders -->

> [!SOURCE-ERROR] `unresolved_navigation_placeholders` (on `visatype`)
<!-- source-error: record:employee | visatype | unresolved_navigation_placeholders -->
