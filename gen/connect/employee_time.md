# Employee_time (`employee_time`)
<!-- id: connect:employee_time -->

| Fact | Value |
|---|---|
| Table | `employee_time` |
| Primary key | `employee_time_id` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `approved` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `approved` |
| `billable` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `billable` |
| `billed` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `billed` |
| `billing_class_id` | NUMBER |  | 39 | 0 | `billing_classes`.`billing_class_id` | foreign_key | `purpose`: `billing_class_id` |
| `billing_subsidiary_id` | NUMBER |  | 39 | 0 | `subsidiaries`.`subsidiary_id` | foreign_key | `purpose`: `billing_subsidiary_id` |
| `break_duration` | NUMBER |  | 12 | 5 |  |  | `purpose`: `break_duration` |
| `charge_billing_run_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `charge_billing_run_id` |
| `class_id` | NUMBER |  | 39 | 0 | `classes`.`class_id` | foreign_key | `purpose`: `class_id` |
| `closed` | TIMESTAMP |  |  |  |  |  | `purpose`: `closed` |
| `comments` | VARCHAR2 | 4000 |  |  |  |  | `purpose`: `comments` |
| `customer_id` | NUMBER |  | 39 | 0 | `customers`.`customer_id` | foreign_key | `purpose`: `customer_id` |
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `date_last_modified` |
| `day_0` | TIMESTAMP |  |  |  |  |  | `purpose`: `day` |
| `department_id` | NUMBER |  | 39 | 0 | `departments`.`department_id` | foreign_key | `purpose`: `department_id` |
| `duration` | NUMBER |  | 12 | 5 |  |  | `purpose`: `duration` |
| `employee_id` | NUMBER |  | 39 | 0 | `employees`.`employee_id` | foreign_key | `purpose`: `employee` |
| `employee_time_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `employee_time_id` |
| `ending` | TIMESTAMP |  |  |  |  |  | `purpose`: `ending` |
| `event_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `event_id` |
| `is_outside_project_plan` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `time_outside_project_plan` |
| `is_posted` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `time_posted` |
| `is_productive` | VARCHAR2 | 1 |  |  |  |  | `purpose`: `productive` |
| `is_utilized` | VARCHAR2 | 1 |  |  |  |  | `purpose`: `utilized` |
| `item_id` | NUMBER |  | 39 | 0 | `service_items`.`item_id` | foreign_key | `purpose`: `item_id` |
| `location_id` | NUMBER |  | 39 | 0 | `locations`.`location_id` | foreign_key | `purpose`: `location_id` |
| `next_approver_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `next_approver_id` |
| `payroll_item_id` | NUMBER |  | 39 | 0 | `payroll_items`.`payroll_item_id` | foreign_key | `purpose`: `payroll_item_id` |
| `payroll_workplace_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `payroll_workplace_id` |
| `price_type_id` | NUMBER |  | 39 | 0 | `price_types`.`price_type_id` | foreign_key | `purpose`: `price_type_id` |
| `rejection_note` | VARCHAR2 | 4000 |  |  |  |  | `purpose`: `rejection_note` |
| `remaining_time_to_charge` | NUMBER |  | 39 | 0 |  |  | `purpose`: `remaining_time_to_charge` |
| `resource_allocation_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `resource_allocation_id` |
| `starting` | TIMESTAMP |  |  |  |  |  | `purpose`: `starting` |
| `status_code` | VARCHAR2 | 200 |  |  |  |  | `purpose`: `status_code` |
| `subsidiary_id` | NUMBER |  | 39 | 0 | `subsidiaries`.`subsidiary_id` | foreign_key | `purpose`: `subsidiary_id` |
| `time_type` | VARCHAR2 | 9 |  |  |  |  | `purpose`: `time_type`; `options`: `actual`, `allocated`, `planned` |
| `timesheet_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `timesheet_id` |
| `transaction_id` | NUMBER |  | 39 | 0 |  | foreign_key | `purpose`: `transaction_id` |
| `transaction_line_id` | NUMBER |  | 39 | 0 | `transaction_lines`.`transaction_line_id` | foreign_key | `purpose`: `transaction_line_id` |
| `unit_cost` | NUMBER |  | 20 | 2 |  |  | `purpose`: `unit_cost` |
| `unit_price` | NUMBER |  | 30 | 15 |  |  | `purpose`: `unit_price` |

## Foreign keys

| Key | Columns | References table | Referenced columns |
|---|---|---|---|
| `employee_time_billing_classes_fk` | `billing_class_id` | `billing_classes` | `billing_class_id` |
| `employee_time_classes_fk` | `class_id` | `classes` | `class_id` |
| `employee_time_customers_fk` | `customer_id` | `customers` | `customer_id` |
| `employee_time_departments_fk` | `department_id` | `departments` | `department_id` |
| `employee_time_employees_fk` | `employee_id` | `employees` | `employee_id` |
| `employee_time_locations_fk` | `location_id` | `locations` | `location_id` |
| `employee_time_payroll_items_fk` | `payroll_item_id` | `payroll_items` | `payroll_item_id` |
| `employee_time_price_types_fk` | `price_type_id` | `price_types` | `price_type_id` |
| `employee_time_service_items_fk` | `item_id` | `service_items` | `item_id` |
| `employee_time_subsidiaries_fk` | `subsidiary_id` | `subsidiaries` | `subsidiary_id` |
| `employee_time_subsidiaries_fk_2` | `billing_subsidiary_id` | `subsidiaries` | `subsidiary_id` |
| `employee_time_transaction_lines_fk` | `transaction_id`, `transaction_line_id` | `transaction_lines` | `transaction_id`, `transaction_line_id` |
| `employee_time_transactions_fk` | `transaction_id` | `transactions` | `transaction_id` |
