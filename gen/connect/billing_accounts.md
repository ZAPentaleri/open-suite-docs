# Billing_accounts (`billing_accounts`)
<!-- id: connect:billing_accounts -->

| Fact | Value |
|---|---|
| Table | `billing_accounts` |
| Record | `billingaccount` |
| Primary key | `billing_account_id` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `address_book_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `address_book_id` |
| `bill_to_address_book_id` | NUMBER |  | 39 | 0 | `address_book`.`address_book_id` | foreign_key | `purpose`: `bill_to_address_book_entry` |
| `billing_account_extid` | VARCHAR2 | 255 |  |  |  |  | `purpose`: `billing_account_external_id` |
| `billing_account_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `billing_account_id` |
| `billing_account_memo` | VARCHAR2 | 400 |  |  |  |  | `purpose`: `billing_account_memo` |
| `billing_account_name` | VARCHAR2 | 60 |  |  |  |  | `purpose`: `billing_account_name` |
| `billing_account_number` | VARCHAR2 | 60 |  |  |  |  | `purpose`: `billing_account_number` |
| `billing_schedule_id` | NUMBER |  | 39 | 0 | `billing_schedule_descriptions`.`billing_schedule_id` | foreign_key | `purpose`: `billing_schedule_id` |
| `class_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `class_id` |
| `currency_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `currency_id` |
| `customer_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `customer_id` |
| `date_created` | TIMESTAMP |  |  |  |  |  | `purpose`: `date_created` |
| `date_last_actual_bill` | TIMESTAMP |  |  |  |  |  | `purpose`: `last_actual_bill_date` |
| `date_last_bill_cycle` | TIMESTAMP |  |  |  |  |  | `purpose`: `last_bill_cycle_date` |
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `date_last_modified` |
| `date_next_bill_cycle` | TIMESTAMP |  |  |  |  |  | `purpose`: `next_bill_cycle_date` |
| `date_start` | TIMESTAMP |  |  |  |  |  | `purpose`: `start_date` |
| `department_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `department_id` |
| `has_off_cycle_bill_request` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `off_cycle_billing_used` |
| `is_customer_default` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `default_billing_account_for_customer` |
| `is_inactive` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `billing_account_inactive` |
| `location_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `location_id` |
| `ship_to_address_book_id` | NUMBER |  | 39 | 0 | `address_book`.`address_book_id` | foreign_key | `purpose`: `ship_to_address_book_entry` |

## Foreign keys

| Key | Columns | References table | Referenced columns |
|---|---|---|---|
| `billing_accounts_address_book_fk_1` | `bill_to_address_book_id` | `address_book` | `address_book_id` |
| `billing_accounts_address_book_fk_2` | `ship_to_address_book_id` | `address_book` | `address_book_id` |
| `billing_accounts_billing_schedule_descriptions_fk` | `billing_schedule_id` | `billing_schedule_descriptions` | `billing_schedule_id` |

## Referenced by

| Key | Table | Columns | Columns here |
|---|---|---|---|
| `Billing_subscriptions_billing_accounts_fk` | `billing_subscriptions` | `billing_account_id` | `billing_account_id` |
