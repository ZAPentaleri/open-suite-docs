# Subsidiaries (`subsidiaries`)
<!-- id: connect:subsidiaries -->

| Fact | Value |
|---|---|
| Table | `subsidiaries` |
| Primary key | `subsidiary_id` |
| Domains | Multibooks |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `address` | VARCHAR2 | 999 |  |  |  |  | `purpose`: `subsidiary_address` |
| `address1` | VARCHAR2 | 150 |  |  |  |  | `purpose`: `address_line_1` |
| `address2` | VARCHAR2 | 150 |  |  |  |  | `purpose`: `address_line_2` |
| `base_currency_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `base_currency_id` |
| `city` | VARCHAR2 | 50 |  |  |  |  | `purpose`: `city` |
| `country` | VARCHAR2 | 2 |  |  |  |  | `purpose`: `country` |
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `date_last_modified`; `time_zone`: `gmt` |
| `edition` | VARCHAR2 | 20 |  |  |  |  | `purpose`: `netsuite_edition` |
| `federal_number` | VARCHAR2 | 40 |  |  |  |  | `purpose`: `federal_tax_id` |
| `fiscal_calendar_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `fiscal_calendar_id` |
| `full_name` | VARCHAR2 | 4000 |  |  |  |  | `purpose`: `full_name` |
| `is_elimination` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `whether_elimination_subsidiary` |
| `is_moss` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `subsidiary_uses_moss` |
| `isinactive` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `subsidiary_is_inactive` |
| `isinactive_bool` | VARCHAR2 | 1 |  |  |  |  | `purpose`: `subsidiary_inactive`; `use_instead`: `isinactive`; `benefit`: `faster_queries_than_isinactive_column` |
| `legal_name` | VARCHAR2 | 83 |  |  |  |  | `purpose`: `legal_name` |
| `moss_nexus_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `moss_nexus` |
| `name` | VARCHAR2 | 83 |  |  |  |  | `purpose`: `name` |
| `parent_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `parent_subsidiary_id` |
| `purchaseorderamount` | NUMBER |  | 20 | 2 |  |  | `purpose`: `purchase_order_amount` |
| `purchaseorderquantity` | NUMBER |  | 18 | 8 |  |  | `purpose`: `purchase_order_quantity` |
| `purchaseorderquantitydiff` | NUMBER |  | 18 | 8 |  |  | `purpose`: `purchase_order_quantity_difference` |
| `receiptamount` | NUMBER |  | 20 | 2 |  |  | `purpose`: `receipt_amount` |
| `receiptquantity` | NUMBER |  | 18 | 8 |  |  | `purpose`: `receipt_quantity` |
| `receiptquantitydiff` | NUMBER |  | 18 | 8 |  |  | `purpose`: `receipt_quantity_difference` |
| `return_address` | VARCHAR2 | 999 |  |  |  |  | `purpose`: `return_address` |
| `return_address1` | VARCHAR2 | 150 |  |  |  |  | `purpose`: `return_address_line_1` |
| `return_address2` | VARCHAR2 | 150 |  |  |  |  | `purpose`: `return_address_line_2` |
| `return_city` | VARCHAR2 | 50 |  |  |  |  | `purpose`: `return_address_city` |
| `return_country` | VARCHAR2 | 50 |  |  |  |  | `purpose`: `return_address_country` |
| `return_state` | VARCHAR2 | 50 |  |  |  |  | `purpose`: `return_address_state` |
| `return_zipcode` | VARCHAR2 | 36 |  |  |  |  | `purpose`: `return_address_postal_code` |
| `shipping_address` | VARCHAR2 | 999 |  |  |  |  | `purpose`: `shipping_address` |
| `shipping_address1` | VARCHAR2 | 150 |  |  |  |  | `purpose`: `shipping_address_line_1` |
| `shipping_address2` | VARCHAR2 | 150 |  |  |  |  | `purpose`: `shipping_address_line_2` |
| `shipping_city` | VARCHAR2 | 50 |  |  |  |  | `purpose`: `shipping_address_city` |
| `shipping_country` | VARCHAR2 | 50 |  |  |  |  | `purpose`: `shipping_address_country` |
| `shipping_state` | VARCHAR2 | 50 |  |  |  |  | `purpose`: `shipping_address_state` |
| `shipping_zipcode` | VARCHAR2 | 36 |  |  |  |  | `purpose`: `shipping_address_postal_code` |
| `state` | VARCHAR2 | 20 |  |  |  |  | `purpose`: `subsidiary_state` |
| `state_tax_number` | VARCHAR2 | 40 |  |  |  |  | `purpose`: `state_tax_id` |
| `subsidiary_extid` | VARCHAR2 | 255 |  |  |  |  | `purpose`: `subsidiary_external_id` |
| `subsidiary_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `subsidiary_id` |
| `tran_num_prefix` | VARCHAR2 | 8 |  |  |  |  | `purpose`: `transaction_number_prefix` |
| `url` | VARCHAR2 | 64 |  |  |  |  | `purpose`: `url` |
| `zipcode` | VARCHAR2 | 36 |  |  |  |  | `purpose`: `subsidiary_postal_code` |

## Referenced by

| Key | Table | Columns | Columns here |
|---|---|---|---|
| `Account_activity_subsidiaries_fk` | `account_activity` | `subsidiary_id` | `subsidiary_id` |
| `Account_period_activity_pe_subsidiaries_fk` | `account_period_activity_pe` | `subsidiary_id` | `subsidiary_id` |
| `Account_period_activity_subsidiaries_fk` | `account_period_activity` | `subsidiary_id` | `subsidiary_id` |
| `Billing_subscriptions_subsidiaries_fk` | `billing_subscriptions` | `subsidiary_id` | `subsidiary_id` |
| `Budget_subsidiaries_fk` | `budget` | `subsidiary_id` | `subsidiary_id` |
| `Companies_subsidiaries_fk` | `companies` | `subsidiary` | `subsidiary_id` |
| `Consolidated_exchange_rates_from_subsidiaries_fk` | `consolidated_exchange_rates` | `from_subsidiary_id` | `subsidiary_id` |
| `Consolidated_exchange_rates_to_subsidiaries_fk` | `consolidated_exchange_rates` | `to_subsidiary_id` | `subsidiary_id` |
| `Customer_subsidiary_map_subsidiaries_fk` | `customer_subsidiary_map` | `subsidiary_id` | `subsidiary_id` |
| `Customers_subsidiaries_fk` | `customers` | `subsidiary_id` | `subsidiary_id` |
| `Customers_subsidiaries_fk_2` | `customers` | `represents_subsidiary_id` | `subsidiary_id` |
| `Employee_time_subsidiaries_fk` | `employee_time` | `subsidiary_id` | `subsidiary_id` |
| `Employee_time_subsidiaries_fk_2` | `employee_time` | `billing_subsidiary_id` | `subsidiary_id` |
| `Employees_subsidiaries_fk` | `employees` | `subsidiary_id` | `subsidiary_id` |
| `Expense_categories_subs_map_subsidiaries_fk` | `expense_categories_subs_map` | `subsidiary_id` | `subsidiary_id` |
| `Global_account_map_subsidiaries_fk` | `global_account_map` | `subsidiary_id` | `subsidiary_id` |
| `Global_inventory_relationships_invt_subsidiaries_fk` | `global_inventory_relationships` | `inventory_subsidiary_id` | `subsidiary_id` |
| `Global_inventory_relationships_orig_subsidiaries_fk` | `global_inventory_relationships` | `originating_subsidiary_id` | `subsidiary_id` |
| `Item_account_map_subsidiaries_fk` | `item_account_map` | `subsidiary_id` | `subsidiary_id` |
| `Item_subsidiary_map_subsidiaries_fk` | `item_subsidiary_map` | `subsidiary_id` | `subsidiary_id` |
| `Item_vendor_map_subsidiaries_fk` | `item_vendor_map` | `subsidiary_id` | `subsidiary_id` |
| `Item_vendor_pricing_subsidiaries_fk` | `item_vendor_pricing` | `subsidiary_id` | `subsidiary_id` |
| `Opportunity_lines_subsidiaries_fk` | `opportunity_lines` | `subsidiary_id` | `subsidiary_id` |
| `Partners_subsidiaries_fk` | `partners` | `subsidiary` | `subsidiary_id` |
| `Posting_account_activity_pe_subsidiaries_fk` | `posting_account_activity_pe` | `subsidiary_id` | `subsidiary_id` |
| `Posting_account_activity_pe_subsidiaries_fk_2` | `posting_account_activity_pe` | `balance_from_subsidiary_id` | `subsidiary_id` |
| `Posting_account_activity_subsidiaries_fk` | `posting_account_activity` | `subsidiary_id` | `subsidiary_id` |
| `Role_subsidiary_map_subsidiaries_fk` | `role_subsidiary_map` | `subsidiary_id` | `subsidiary_id` |
| `Subscription_plans_subsidiaries_fk` | `subscription_plans` | `subsidiary_id` | `subsidiary_id` |
| `Subsidiary_class_map_subsidiaries_fk` | `subsidiary_class_map` | `subsidiary_id` | `subsidiary_id` |
| `Subsidiary_department_map_subsidiaries_fk` | `subsidiary_department_map` | `subsidiary_id` | `subsidiary_id` |
| `Subsidiary_location_map_subsidiaries_fk` | `subsidiary_location_map` | `subsidiary_id` | `subsidiary_id` |
| `Support_incidents_subsidiaries_fk` | `support_incidents` | `subsidiary_id` | `subsidiary_id` |
| `Transaction_lines_subsidiaries_fk_1` | `transaction_lines` | `subsidiary_id` | `subsidiary_id` |
| `Transaction_lines_subsidiaries_fk_2` | `transaction_lines` | `source_subsidiary_id` | `subsidiary_id` |
| `Vendor_subsidiary_map_subsidiaries_fk` | `vendor_subsidiary_map` | `subsidiary_id` | `subsidiary_id` |
| `Vendors_subsidiaries_fk` | `vendors` | `subsidiary` | `subsidiary_id` |
| `Vendors_subsidiaries_fk_2` | `vendors` | `represents_subsidiary_id` | `subsidiary_id` |
