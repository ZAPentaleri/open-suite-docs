# Locations (`locations`)
<!-- id: connect:locations -->

| Fact | Value |
|---|---|
| Table | `locations` |
| Record | `location` |
| Primary key | `location_id` |
| Domains | Expense_amortization, Invoice_with_amortization, Multibooks |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `address` | VARCHAR2 | 999 |  |  |  |  | `purpose`: `address` |
| `address_one` | VARCHAR2 | 150 |  |  |  |  | `purpose`: `address_one` |
| `address_three` | VARCHAR2 | 150 |  |  |  |  | `purpose`: `address_three` |
| `address_two` | VARCHAR2 | 150 |  |  |  |  | `purpose`: `address_two` |
| `addressee` | VARCHAR2 | 100 |  |  |  |  | `purpose`: `addressee` |
| `attention` | VARCHAR2 | 150 |  |  |  |  | `purpose`: `attention` |
| `city` | VARCHAR2 | 50 |  |  |  |  | `purpose`: `city` |
| `country` | VARCHAR2 | 50 |  |  |  |  | `purpose`: `country` |
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `date_last_modified` |
| `full_name` | VARCHAR2 | 4000 |  |  |  |  | `purpose`: `full_name` |
| `inventory_available` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `inventory_is_available` |
| `inventory_available_web_store` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `inventory_available_in_web_store` |
| `is_include_in_supply_planning` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `included_in_supply_planning` |
| `isinactive` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `location_is_inactive` |
| `location_extid` | VARCHAR2 | 255 |  |  |  |  | `purpose`: `location_external_id` |
| `location_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `location_id` |
| `name` | VARCHAR2 | 60 |  |  |  |  | `purpose`: `name` |
| `parent_id` | NUMBER |  | 39 | 0 | `locations`.`location_id` | foreign_key | `purpose`: `sublocation_of` |
| `phone` | VARCHAR2 | 100 |  |  |  |  | `purpose`: `phone` |
| `return_address_one` | VARCHAR2 | 150 |  |  |  |  | `purpose`: `return_address_one` |
| `return_address_two` | VARCHAR2 | 150 |  |  |  |  | `purpose`: `return_address_two` |
| `return_city` | VARCHAR2 | 50 |  |  |  |  | `purpose`: `return_city` |
| `return_country` | VARCHAR2 | 50 |  |  |  |  | `purpose`: `return_country` |
| `return_state` | VARCHAR2 | 50 |  |  |  |  | `purpose`: `return_state` |
| `return_zipcode` | VARCHAR2 | 36 |  |  |  |  | `purpose`: `return_zip_code` |
| `returnaddress` | VARCHAR2 | 999 |  |  |  |  | `purpose`: `return_address` |
| `state` | VARCHAR2 | 50 |  |  |  |  | `purpose`: `state` |
| `tran_num_prefix` | VARCHAR2 | 8 |  |  |  |  | `purpose`: `transaction_number_prefix` |
| `use_bins` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `use_bins` |
| `zipcode` | VARCHAR2 | 36 |  |  |  |  | `purpose`: `zip_code` |

## Foreign keys

| Key | Columns | References table | Referenced columns |
|---|---|---|---|
| `locations_locations_fk` | `parent_id` | `locations` | `location_id` |

## Referenced by

| Key | Table | Columns | Columns here |
|---|---|---|---|
| `Accounts_locations_fk` | `accounts` | `location_id` | `location_id` |
| `Billing_subscription_lines_locations_fk` | `billing_subscription_lines` | `location_id` | `location_id` |
| `Billing_subscriptions_locations_fk` | `billing_subscriptions` | `location_id` | `location_id` |
| `Budget_locations_fk` | `budget` | `location_id` | `location_id` |
| `Employee_time_locations_fk` | `employee_time` | `location_id` | `location_id` |
| `Employees_locations_fk` | `employees` | `location_id` | `location_id` |
| `Global_account_map_locations_fk` | `global_account_map` | `location_id` | `location_id` |
| `Item_account_map_locations_fk` | `item_account_map` | `location_id` | `location_id` |
| `Location_costing_grp_locations_locations_fk` | `location_costing_grp_locations` | `location_id` | `location_id` |
| `Locations_locations_fk` | `locations` | `parent_id` | `location_id` |
| `Opportunity_lines_locations_fk` | `opportunity_lines` | `location_id` | `location_id` |
| `Planned_standard_costs_locations_fk` | `planned_standard_costs` | `location_id` | `location_id` |
| `Posting_account_activity_locations_fk` | `posting_account_activity` | `location_id` | `location_id` |
| `Posting_account_activity_pe_locations_fk` | `posting_account_activity_pe` | `location_id` | `location_id` |
| `Subscription_plans_locations_fk` | `subscription_plans` | `location_id` | `location_id` |
| `Subsidiary_location_map_locations_fk` | `subsidiary_location_map` | `location_id` | `location_id` |
