# Employees (`employees`)
<!-- id: connect:employees -->

| Fact | Value |
|---|---|
| Table | `employees` |
| Record | `employee` |
| Primary key | `employee_id` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `accountnumber` | VARCHAR2 | 4000 |  |  |  |  | `purpose`: `account_number` |
| `address` | VARCHAR2 | 999 |  |  |  |  | `purpose`: `address` |
| `approvallimit` | NUMBER |  | 20 | 2 |  |  | `purpose`: `approval_limit` |
| `approver_id` | NUMBER |  | 39 | 0 | `employees`.`employee_id` | foreign_key | `purpose`: `approver_id` |
| `billing_class_id` | NUMBER |  | 39 | 0 | `billing_classes`.`billing_class_id` | foreign_key | `purpose`: `billing_class` |
| `birthdate` | TIMESTAMP |  |  |  |  |  | `purpose`: `birth_date` |
| `city` | VARCHAR2 | 50 |  |  |  |  | `purpose`: `city` |
| `class_id` | NUMBER |  | 39 | 0 | `classes`.`class_id` | foreign_key | `purpose`: `class_id` |
| `comments` | VARCHAR2 | 4000 |  |  |  |  | `purpose`: `comments` |
| `country` | VARCHAR2 | 50 |  |  |  |  | `purpose`: `country` |
| `create_date` | TIMESTAMP |  |  |  |  |  | `purpose`: `create_date`; `time_zone`: `gmt` |
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `date_last_modified` |
| `def_acct_corp_card_expenses_id` | NUMBER |  | 39 | 0 | `accounts`.`account_id` | foreign_key | `purpose`: `default_corporate_card_expense_account` |
| `default_job_resource_role_id` | NUMBER |  | 39 | 0 | `job_resource_role`.`job_resource_role_id` | foreign_key | `purpose`: `default_job_resource_role` |
| `department_id` | NUMBER |  | 39 | 0 | `departments`.`department_id` | foreign_key | `purpose`: `department_id` |
| `email` | VARCHAR2 | 254 |  |  |  |  | `purpose`: `email` |
| `employee_extid` | VARCHAR2 | 255 |  |  |  |  | `purpose`: `employee_external_id` |
| `employee_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `employee_id` |
| `employee_type_id` | NUMBER |  | 39 | 0 | `employee_types`.`employee_type_id` | foreign_key | `purpose`: `employee_type_id` |
| `firstname` | VARCHAR2 | 32 |  |  |  |  | `purpose`: `first_name` |
| `full_name` | VARCHAR2 | 1800 |  |  |  |  | `purpose`: `full_name` |
| `gender` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `gender` |
| `hireddate` | TIMESTAMP |  |  |  |  |  | `purpose`: `hire_date` |
| `home_phone` | VARCHAR2 | 100 |  |  |  |  | `purpose`: `home_phone` |
| `initials` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `initials` |
| `is_in_payroll` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `is_in_payroll` |
| `is_job_manager` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `employee_is_project_manager` |
| `is_job_resource` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `is_job_resource` |
| `isinactive` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `is_inactive` |
| `issalesrep` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `sales_rep` |
| `issupportrep` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `support_rep` |
| `job_description` | VARCHAR2 | 65534 |  |  |  |  | `purpose`: `job_description` |
| `jobtitle` | VARCHAR2 | 100 |  |  |  |  | `purpose`: `job_title` |
| `labor_cost` | NUMBER |  | 39 | 0 |  |  | `purpose`: `labor_cost` |
| `last_modified_date` | TIMESTAMP |  |  |  |  |  | `purpose`: `date_last_modified`; `time_zone`: `gmt` |
| `lastname` | VARCHAR2 | 32 |  |  |  |  | `purpose`: `last_name` |
| `lastreviewdate` | TIMESTAMP |  |  |  |  |  | `purpose`: `last_review_date` |
| `line1` | VARCHAR2 | 150 |  |  |  |  | `purpose`: `address_line_1` |
| `line2` | VARCHAR2 | 150 |  |  |  |  | `purpose`: `address_line_2` |
| `line3` | VARCHAR2 | 150 |  |  |  |  | `purpose`: `address_line_3` |
| `location_id` | NUMBER |  | 39 | 0 | `locations`.`location_id` | foreign_key | `purpose`: `location_id` |
| `loginaccess` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `login_access` |
| `marital_status_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `marital_status_id` |
| `middlename` | VARCHAR2 | 32 |  |  |  |  | `purpose`: `middle_name` |
| `mobile_phone` | VARCHAR2 | 100 |  |  |  |  | `purpose`: `mobile_phone` |
| `name` | VARCHAR2 | 83 |  |  |  |  | `purpose`: `name` |
| `nextreviewdate` | TIMESTAMP |  |  |  |  |  | `purpose`: `next_review_date` |
| `office_phone` | VARCHAR2 | 100 |  |  |  |  | `purpose`: `office_phone` |
| `phone` | VARCHAR2 | 100 |  |  |  |  | `purpose`: `phone` |
| `releasedate` | TIMESTAMP |  |  |  |  |  | `purpose`: `release_date` |
| `salutation` | VARCHAR2 | 30 |  |  |  |  | `purpose`: `salutation` |
| `socialsecuritynumber` | VARCHAR2 | 11 |  |  |  |  | `purpose`: `social_security_number` |
| `state` | VARCHAR2 | 50 |  |  |  |  | `purpose`: `state` |
| `status` | VARCHAR2 | 100 |  |  |  |  | `purpose`: `employee_status` |
| `subsidiary_id` | NUMBER |  | 39 | 0 | `subsidiaries`.`subsidiary_id` | foreign_key | `purpose`: `subsidiary_id` |
| `supervisor_id` | NUMBER |  | 39 | 0 | `employees`.`employee_id` | foreign_key | `purpose`: `supervisor_id` |
| `target_utilization` | NUMBER |  | 12 | 5 |  |  | `purpose`: `target_utilization` |
| `work_calendar_id` | NUMBER |  | 39 | 0 | `work_calendars`.`work_calendar_id` | foreign_key | `purpose`: `work_calendar_id` |
| `zipcode` | VARCHAR2 | 36 |  |  |  |  | `purpose`: `zip_code` |
| `def_expense_report_currency_id` | NUMBER |  | 39 | 0 | `currencies`.`currency_id` | foreign_key |  |

## Foreign keys

| Key | Columns | References table | Referenced columns |
|---|---|---|---|
| `employees_accounts_fk` | `def_acct_corp_card_expenses_id` | `accounts` | `account_id` |
| `employees_billing_classes_fk` | `billing_class_id` | `billing_classes` | `billing_class_id` |
| `employees_classes_fk` | `class_id` | `classes` | `class_id` |
| `employees_currencies_fk` | `def_expense_report_currency_id` | `currencies` | `currency_id` |
| `employees_departments_fk` | `department_id` | `departments` | `department_id` |
| `employees_employee_types_fk` | `employee_type_id` | `employee_types` | `employee_type_id` |
| `employees_employees_fk` | `approver_id` | `employees` | `employee_id` |
| `employees_employees_fk_2` | `supervisor_id` | `employees` | `employee_id` |
| `employees_job_resource_role_fk` | `default_job_resource_role_id` | `job_resource_role` | `job_resource_role_id` |
| `employees_locations_fk` | `location_id` | `locations` | `location_id` |
| `employees_subsidiaries_fk` | `subsidiary_id` | `subsidiaries` | `subsidiary_id` |
| `employees_work_calendars_fk` | `work_calendar_id` | `work_calendars` | `work_calendar_id` |

## Referenced by

| Key | Table | Columns | Columns here |
|---|---|---|---|
| `Billing_rate_cards_versions_employees_fk` | `billing_rate_cards_versions` | `last_modified_by_id` | `employee_id` |
| `Customers_employees_fk` | `customers` | `project_manager_id` | `employee_id` |
| `Employee_corporate_cards_employees_fk` | `employee_corporate_cards` | `employee_id` | `employee_id` |
| `Employee_currency_map_employees_fk` | `employee_currency_map` | `employee_id` | `employee_id` |
| `Employee_time_employees_fk` | `employee_time` | `employee_id` | `employee_id` |
| `Employees_employees_fk` | `employees` | `approver_id` | `employee_id` |
| `Employees_employees_fk_2` | `employees` | `supervisor_id` | `employee_id` |
| `Imported_employee_expenses_employees_fk` | `imported_employee_expenses` | `employee_id` | `employee_id` |
| `Project_revenue_rules_employees_fk_1` | `project_revenue_rules` | `created_by` | `employee_id` |
| `Project_revenue_rules_employees_fk_2` | `project_revenue_rules` | `last_modified_by` | `employee_id` |
| `Project_templates_employees_fk` | `project_templates` | `project_manager_id` | `employee_id` |
| `Revenue_plan_versions_employees_fk` | `revenue_plan_versions` | `created_by_id` | `employee_id` |
| `Subscription_change_orders_employees_fk` | `subscription_change_orders` | `requester_id` | `employee_id` |
| `Unlocked_time_periods_employees_fk` | `unlocked_time_periods` | `employee_id` | `employee_id` |
| `Unlocked_time_periods_employees_fk_2` | `unlocked_time_periods` | `last_modified_by_id` | `employee_id` |
| `Vendors_employees_fk` | `vendors` | `time_approver_id` | `employee_id` |
