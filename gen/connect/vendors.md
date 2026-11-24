# Vendors (`vendors`)
<!-- id: connect:vendors -->

| Fact | Value |
|---|---|
| Table | `vendors` |
| Record | `vendor` |
| Primary key | `vendor_id` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `accountnumber` | VARCHAR2 | 99 |  |  |  |  | `purpose`: `account_number` |
| `altemail` | VARCHAR2 | 254 |  |  |  |  | `purpose`: `alt_email` |
| `altphone` | VARCHAR2 | 100 |  |  |  |  | `purpose`: `alt_phone` |
| `billaddress` | VARCHAR2 | 999 |  |  |  |  | `purpose`: `billing_address` |
| `billing_class_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `billing_class_id` |
| `city` | VARCHAR2 | 50 |  |  |  |  | `purpose`: `city` |
| `comments` | VARCHAR2 | 4000 |  |  |  |  | `purpose`: `comments` |
| `companyname` | VARCHAR2 | 83 |  |  |  |  | `purpose`: `company_name` |
| `country` | VARCHAR2 | 50 |  |  |  |  | `purpose`: `country` |
| `create_date` | TIMESTAMP |  |  |  |  |  | `purpose`: `create_date`; `time_zone`: `gmt` |
| `creditlimit` | NUMBER |  | 20 | 2 |  |  | `purpose`: `credit_limit` |
| `currency_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `currency_id` |
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `date_last_modified` |
| `email` | VARCHAR2 | 254 |  |  |  |  | `purpose`: `email` |
| `expense_account_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `expense_account_id` |
| `fax` | VARCHAR2 | 100 |  |  |  |  | `purpose`: `fax` |
| `full_name` | VARCHAR2 | 1800 |  |  |  |  | `purpose`: `full_name` |
| `home_phone` | VARCHAR2 | 100 |  |  |  |  | `purpose`: `home_phone` |
| `in_transit_balance` | NUMBER |  | 39 | 0 |  |  | `purpose`: `in_transit_balance` |
| `incoterm` | VARCHAR2 | 100 |  |  |  |  | `purpose`: `international_commercial_term_incoterm` |
| `is_person` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `type_company_or_individual` |
| `is1099eligible` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `1099_eligible` |
| `isemailhtml` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `email_as_html` |
| `isemailpdf` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `email_as_pdf` |
| `isinactive` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `vendor_is_inactive` |
| `labor_cost` | NUMBER |  | 39 | 0 |  |  | `purpose`: `cost_of_labor` |
| `last_modified_date` | TIMESTAMP |  |  |  |  |  | `purpose`: `last_modified_date`; `time_zone`: `gmt` |
| `line1` | VARCHAR2 | 150 |  |  |  |  | `purpose`: `address_line_1` |
| `line2` | VARCHAR2 | 150 |  |  |  |  | `purpose`: `address_line_2` |
| `line3` | VARCHAR2 | 150 |  |  |  |  | `purpose`: `address_line_3` |
| `loginaccess` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `login_access` |
| `mobile_phone` | VARCHAR2 | 100 |  |  |  |  | `purpose`: `mobile_phone` |
| `name` | VARCHAR2 | 83 |  |  |  |  | `purpose`: `name` |
| `openbalance` | NUMBER |  | 39 | 0 |  |  | `purpose`: `vendor_payable_balance_due`; `value_unit`: `base_currency` |
| `openbalance_foreign` | NUMBER |  | 39 | 0 |  |  | `purpose`: `vendor_payable_balance_due`; `value_unit`: `foreign_currency` |
| `payables_account_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `default_payables_account` |
| `payment_terms_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `payment_terms_id` |
| `phone` | VARCHAR2 | 100 |  |  |  |  | `purpose`: `phone` |
| `prepayment_balance` | NUMBER |  | 39 | 0 |  |  | `purpose`: `prepayment_balance` |
| `printoncheckas` | VARCHAR2 | 83 |  |  |  |  | `purpose`: `print_on_checks_as` |
| `purchaseorderamount` | NUMBER |  | 20 | 2 |  |  | `purpose`: `purchase_order_amount` |
| `purchaseorderquantity` | NUMBER |  | 18 | 8 |  |  | `purpose`: `purchase_order_quantity` |
| `purchaseorderquantitydiff` | NUMBER |  | 18 | 8 |  |  | `purpose`: `purchase_order_quantity_difference` |
| `receiptamount` | NUMBER |  | 20 | 2 |  |  | `purpose`: `receipt_amount` |
| `receiptquantity` | NUMBER |  | 18 | 8 |  |  | `purpose`: `receipt_quantity` |
| `receiptquantitydiff` | NUMBER |  | 18 | 8 |  |  | `purpose`: `receipt_quantity_difference` |
| `represents_subsidiary_id` | NUMBER |  | 39 | 0 | `subsidiaries`.`subsidiary_id` | foreign_key | `purpose`: `represents_subsidiary_id` |
| `shipaddress` | VARCHAR2 | 999 |  |  |  |  | `purpose`: `shipping_address` |
| `state` | VARCHAR2 | 50 |  |  |  |  | `purpose`: `province_state` |
| `subsidiary` | NUMBER |  | 39 | 0 | `subsidiaries`.`subsidiary_id` | foreign_key | `purpose`: `subsidiary` |
| `taxidnum` | VARCHAR2 | 25 |  |  |  |  | `purpose`: `tax_id` |
| `time_approver_id` | NUMBER |  | 39 | 0 | `employees`.`employee_id` | foreign_key | `purpose`: `time_approver_id` |
| `unbilled_orders` | NUMBER |  | 39 | 0 |  |  | `purpose`: `vendor_unbilled_orders_total`; `value_unit`: `base_currency` |
| `unbilled_orders_foreign` | NUMBER |  | 39 | 0 |  |  | `purpose`: `vendor_unbilled_orders_total`; `value_unit`: `foreign_currency` |
| `url` | VARCHAR2 | 100 |  |  |  |  | `purpose`: `url` |
| `vendor_extid` | VARCHAR2 | 255 |  |  |  |  | `purpose`: `vendor_external_id` |
| `vendor_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `vendor_id` |
| `vendor_type_id` | NUMBER |  | 39 | 0 | `vendor_types`.`vendor_type_id` | foreign_key | `purpose`: `vendor_type`; `displayed_on`: `vendor_record_as_category`; `caveat`: `type_on_vendor_form_means_entity_type`; `caveat`: `no_vendor_category_id_column_in_connect` |
| `zipcode` | VARCHAR2 | 36 |  |  |  |  | `purpose`: `zip_code` |

## Foreign keys

| Key | Columns | References table | Referenced columns |
|---|---|---|---|
| `vendors_employees_fk` | `time_approver_id` | `employees` | `employee_id` |
| `vendors_subsidiaries_fk` | `subsidiary` | `subsidiaries` | `subsidiary_id` |
| `vendors_subsidiaries_fk_2` | `represents_subsidiary_id` | `subsidiaries` | `subsidiary_id` |
| `vendors_vendor_types_fk` | `vendor_type_id` | `vendor_types` | `vendor_type_id` |

## Referenced by

| Key | Table | Columns | Columns here |
|---|---|---|---|
| `Inbound_shipment_items_vendors_fk` | `inbound_shipment_items` | `vendor_id` | `vendor_id` |
| `Item_vendor_map_vendors_fk` | `item_vendor_map` | `vendor_id` | `vendor_id` |
| `Item_vendor_pricing_vendors_fk` | `item_vendor_pricing` | `vendor_id` | `vendor_id` |
| `Vendor_subsidiary_map_vendors_fk` | `vendor_subsidiary_map` | `vendor_id` | `vendor_id` |
