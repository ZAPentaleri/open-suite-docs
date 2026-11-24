# Currencies (`currencies`)
<!-- id: connect:currencies -->

| Fact | Value |
|---|---|
| Table | `currencies` |
| Primary key | `currency_id` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `currency_extid` | VARCHAR2 | 255 |  |  |  |  | `purpose`: `currency_external_id` |
| `currency_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `currency_id` |
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `date_last_modified` |
| `is_inactive` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `is_inactive` |
| `name` | VARCHAR2 | 105 |  |  |  |  | `purpose`: `name` |
| `precision_0` | NUMBER |  | 1 | 0 |  |  | `purpose`: `precision` |
| `symbol` | VARCHAR2 | 4 |  |  |  |  | `purpose`: `symbol` |

## Referenced by

| Key | Table | Columns | Columns here |
|---|---|---|---|
| `Accounts_currencies_fk` | `accounts` | `currency_id` | `currency_id` |
| `Billing_rate_cards_prices_currencies_fk` | `billing_rate_cards_prices` | `currency_id` | `currency_id` |
| `Companies_currencies_fk` | `companies` | `currency_id` | `currency_id` |
| `Customer_currencies_currencies_fk` | `customer_currencies` | `currency_id` | `currency_id` |
| `Customers_currencies_fk` | `customers` | `currency_id` | `currency_id` |
| `Employee_currency_map_currencies_fk` | `employee_currency_map` | `currency_id` | `currency_id` |
| `Employees_currencies_fk` | `employees` | `def_expense_report_currency_id` | `currency_id` |
| `Imported_employee_expenses_currencies_fk` | `imported_employee_expenses` | `billed_currency_id` | `currency_id` |
| `Imported_employee_expenses_currencies_fk_2` | `imported_employee_expenses` | `card_currency_id` | `currency_id` |
| `Inbound_shipment_items_currencies_fk` | `inbound_shipment_items` | `currency_id` | `currency_id` |
| `Inbound_shipments_currencies_fk` | `inbound_shipments` | `currency_id` | `currency_id` |
| `Item_billing_rates_currencies_fk` | `item_billing_rates` | `currency_id` | `currency_id` |
| `Item_price_history_currencies_fk` | `item_price_history` | `currency_id` | `currency_id` |
| `Item_vendor_pricing_currencies_fk` | `item_vendor_pricing` | `currency_id` | `currency_id` |
| `Location_costing_groups_currencies_fk` | `location_costing_groups` | `currency_id` | `currency_id` |
| `Opportunities_currencies_fk` | `opportunities` | `currency_id` | `currency_id` |
| `Sales_reps_currencies_fk` | `sales_reps` | `def_expense_report_currency_id` | `currency_id` |
| `Support_reps_currencies_fk` | `support_reps` | `def_expense_report_currency_id` | `currency_id` |
