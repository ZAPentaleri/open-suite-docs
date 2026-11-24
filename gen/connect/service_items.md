# Service_items (`service_items`)
<!-- id: connect:service_items -->

| Fact | Value |
|---|---|
| Table | `service_items` |
| Record | `serviceitem` |
| Primary key | `item_id` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `cost_0` | NUMBER |  | 30 | 15 |  |  | `purpose`: `cost` |
| `cost_category` | VARCHAR2 | 50 |  |  |  |  | `purpose`: `cost_category` |
| `create_plan_on_event_type` | VARCHAR2 | 128 |  |  |  |  | `purpose`: `create_revenue_plan_on_event_type` |
| `created` | TIMESTAMP |  |  |  |  |  | `purpose`: `created`; `time_zone`: `gmt` |
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `date_last_modified` |
| `date_of_last_transaction` | TIMESTAMP |  |  |  |  |  | `purpose`: `date_of_last_transaction` |
| `description` | VARCHAR2 | 4000 |  |  |  |  | `purpose`: `description` |
| `displayname` | VARCHAR2 | 4000 |  |  |  |  | `purpose`: `display_name_code` |
| `expense_account_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `expense_account_id` |
| `featureddescription` | VARCHAR2 | 65534 |  |  |  |  | `purpose`: `featured_description` |
| `full_name` | VARCHAR2 | 550 |  |  |  |  | `purpose`: `full_name` |
| `include_child_subsidiaries` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `include_child_subsidiaries` |
| `income_account_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `income_account_id` |
| `interco_expense_account_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `intercompany_expense_account_id` |
| `interco_income_account_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `intercompany_income_account_id` |
| `is_moss` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `is_moss` |
| `isinactive` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `item_is_inactive` |
| `isonline` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `is_online` |
| `istaxable` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `taxable` |
| `item_extid` | VARCHAR2 | 255 |  |  |  |  | `purpose`: `item_external_id` |
| `item_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `item_id` |
| `item_revenue_category` | VARCHAR2 | 250 |  |  |  |  | `purpose`: `item_revenue_category` |
| `manufacturing_charge_item` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `manufacturing_charge_item` |
| `modified` | TIMESTAMP |  |  |  |  |  | `purpose`: `modified`; `time_zone`: `gmt` |
| `name` | VARCHAR2 | 250 |  |  |  |  | `purpose`: `name` |
| `offersupport` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `offer_support` |
| `onspecial` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `on_special` |
| `parent_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `subitem_of` |
| `payment_method_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `payment_method_id` |
| `pref_purchase_tax_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `preferred_purchase_tax_id` |
| `pref_sale_tax_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `preferred_sales_tax_id` |
| `prices_include_tax` | VARCHAR2 | 1 |  |  |  |  | `purpose`: `prices_include_tax` |
| `print_sub_items` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `print_sub_items` |
| `purchasedescription` | VARCHAR2 | 4000 |  |  |  |  | `purpose`: `purchase_description` |
| `rate` | VARCHAR2 | 42 |  |  |  |  | `purpose`: `rate` |
| `rev_rec_forecast_rule_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `revenue_recognition_forecast_rule` |
| `rev_rec_rule_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `revenue_recognition_rule_id` |
| `revenue_allocation_group` | VARCHAR2 | 60 |  |  |  |  | `purpose`: `revenue_allocation_group` |
| `specialsdescription` | VARCHAR2 | 65534 |  |  |  |  | `purpose`: `specials_description` |
| `storedescription` | VARCHAR2 | 4000 |  |  |  |  | `purpose`: `store_description` |
| `storedetaileddescription` | VARCHAR2 | 65534 |  |  |  |  | `purpose`: `detailed_description` |
| `storedisplayname` | VARCHAR2 | 500 |  |  |  |  | `purpose`: `store_display_name` |
| `subtype` | VARCHAR2 | 10 |  |  |  |  | `purpose`: `subtype` |
| `upc_code` | VARCHAR2 | 180 |  |  |  |  | `purpose`: `upc_code` |
| `vendor_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `vendor_id` |
| `vendorname` | VARCHAR2 | 250 |  |  |  |  | `purpose`: `vendor_name` |
| `vsoe_deferral` | VARCHAR2 | 28 |  |  |  |  | `purpose`: `vsoe_deferral` |
| `vsoe_delivered` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `vsoe_delivered` |
| `vsoe_discount` | VARCHAR2 | 12 |  |  |  |  | `purpose`: `vsoe_discount` |
| `vsoe_price` | NUMBER |  | 30 | 15 |  |  | `purpose`: `vsoe_price` |
| `fx_adjustment_account_id` | NUMBER |  | 39 | 0 |  |  |  |
| `matrix_type` | VARCHAR2 | 6 |  |  |  |  |  |

## Referenced by

| Key | Table | Columns | Columns here |
|---|---|---|---|
| `Employee_time_service_items_fk` | `employee_time` | `item_id` | `item_id` |
| `Item_billing_rates_service_items_fk` | `item_billing_rates` | `item_id` | `item_id` |
| `Project_revenue_rules_service_items_fk` | `project_revenue_rules` | `service_item_id` | `item_id` |
