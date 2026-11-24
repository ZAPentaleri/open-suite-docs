# Sales_reps (`sales_reps`)
<!-- id: connect:sales_reps -->

| Fact | Value |
|---|---|
| Table | `sales_reps` |
| Primary key | `sales_rep_id` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `account_number` | VARCHAR2 | 4000 |  |  |  |  | `purpose`: `account_number` |
| `address` | VARCHAR2 | 999 |  |  |  |  | `purpose`: `address` |
| `address1` | VARCHAR2 | 150 |  |  |  |  | `purpose`: `address_line_1` |
| `address2` | VARCHAR2 | 150 |  |  |  |  | `purpose`: `address_line_2` |
| `address3` | VARCHAR2 | 150 |  |  |  |  | `purpose`: `address_line_3` |
| `approval_limit` | NUMBER |  | 20 | 2 |  |  | `purpose`: `approval_limit` |
| `approver_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `approver_id` |
| `birthdate` | TIMESTAMP |  |  |  |  |  | `purpose`: `birth_date` |
| `city` | VARCHAR2 | 50 |  |  |  |  | `purpose`: `city` |
| `class_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `class_id` |
| `country` | VARCHAR2 | 50 |  |  |  |  | `purpose`: `country` |
| `create_date` | TIMESTAMP |  |  |  |  |  | `purpose`: `create_date` |
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `date_last_modified`; `time_zone`: `gmt` |
| `def_acct_corp_card_expenses_id` | NUMBER |  | 39 | 0 | `accounts`.`account_id` | foreign_key | `purpose`: `default_corporate_card_expense_account` |
| `department_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `department_id` |
| `email` | VARCHAR2 | 254 |  |  |  |  | `purpose`: `email` |
| `employee_type_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `employee_type_id` |
| `firstname` | VARCHAR2 | 32 |  |  |  |  | `purpose`: `first_name` |
| `full_name` | VARCHAR2 | 1800 |  |  |  |  | `purpose`: `full_name` |
| `identification_number` | VARCHAR2 | 11 |  |  |  |  | `purpose`: `social_security_number` |
| `initials` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `initials` |
| `isinactive` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `employee_is_inactive` |
| `lastname` | VARCHAR2 | 32 |  |  |  |  | `purpose`: `last_name` |
| `loginaccess` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `login_access` |
| `midname` | VARCHAR2 | 32 |  |  |  |  | `purpose`: `middle_name` |
| `name` | VARCHAR2 | 83 |  |  |  |  | `purpose`: `name` |
| `notes` | VARCHAR2 | 4000 |  |  |  |  | `purpose`: `notes` |
| `phone` | VARCHAR2 | 100 |  |  |  |  | `purpose`: `phone` |
| `sales_rep_extid` | VARCHAR2 | 255 |  |  |  |  | `purpose`: `sales_rep_external_id` |
| `sales_rep_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `sales_rep_id` |
| `sales_role_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `sales_role` |
| `salutation` | VARCHAR2 | 30 |  |  |  |  | `purpose`: `salutation` |
| `state` | VARCHAR2 | 50 |  |  |  |  | `purpose`: `state` |
| `subsidiary_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `subsidiary_id` |
| `supervisor_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `supervisor_id` |
| `zipcode` | VARCHAR2 | 36 |  |  |  |  | `purpose`: `zip_code` |
| `def_expense_report_currency_id` | NUMBER |  | 39 | 0 | `currencies`.`currency_id` | foreign_key |  |

## Foreign keys

| Key | Columns | References table | Referenced columns |
|---|---|---|---|
| `sales_reps_accounts_fk` | `def_acct_corp_card_expenses_id` | `accounts` | `account_id` |
| `sales_reps_currencies_fk` | `def_expense_report_currency_id` | `currencies` | `currency_id` |

## Referenced by

| Key | Table | Columns | Columns here |
|---|---|---|---|
| `Posting_account_activity_pe_sales_reps_fk` | `posting_account_activity_pe` | `sales_rep_id` | `sales_rep_id` |
| `Posting_account_activity_sales_reps_fk` | `posting_account_activity` | `sales_rep_id` | `sales_rep_id` |
