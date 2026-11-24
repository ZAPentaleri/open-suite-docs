# Originating_leads (`originating_leads`)
<!-- id: connect:originating_leads -->

| Fact | Value |
|---|---|
| Table | `originating_leads` |
| Record | `originatinglead` |
| Primary key | `customer_id` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `accountnumber` | VARCHAR2 | 99 |  |  |  |  | `purpose`: `account_number` |
| `altemail` | VARCHAR2 | 254 |  |  |  |  | `purpose`: `alternate_email_address` |
| `alternate_contact_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `alternate_contact` |
| `altphone` | VARCHAR2 | 100 |  |  |  |  | `purpose`: `alternate_phone` |
| `amount_complete` | NUMBER |  | 5 | 2 |  |  | `purpose`: `amount_complete` |
| `billaddress` | VARCHAR2 | 999 |  |  |  |  | `purpose`: `billing_address` |
| `calculated_end` | TIMESTAMP |  |  |  |  |  | `purpose`: `estimated_end_date` |
| `category_0` | VARCHAR2 | 30 |  |  |  |  | `purpose`: `category` |
| `city` | VARCHAR2 | 50 |  |  |  |  | `purpose`: `city` |
| `comments` | VARCHAR2 | 4000 |  |  |  |  | `purpose`: `comments` |
| `companyname` | VARCHAR2 | 83 |  |  |  |  | `purpose`: `company_name` |
| `consol_days_overdue` | NUMBER |  | 39 | 0 |  |  | `purpose`: `consolidated_days_overdue` |
| `consol_deposit_balance` | NUMBER |  | 39 | 0 |  |  | `purpose`: `consolidated_deposit_balance` |
| `consol_deposit_balance_foreign` | NUMBER |  | 39 | 0 |  |  | `purpose`: `consolidated_deposit_balance`; `value_unit`: `foreign_currency` |
| `consol_openbalance` | NUMBER |  | 39 | 0 |  |  | `purpose`: `consolidated_balance` |
| `consol_openbalance_foreign` | NUMBER |  | 39 | 0 |  |  | `purpose`: `consolidated_foreign_currency_balance` |
| `consol_unbilled_orders` | NUMBER |  | 39 | 0 |  |  | `purpose`: `consolidated_unbilled_orders` |
| `consol_unbilled_orders_foreign` | NUMBER |  | 39 | 0 |  |  | `purpose`: `consolidated_unbilled_orders`; `value_unit`: `foreign_currency` |
| `converted_to_contact_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `contact_converted_from_lead` |
| `converted_to_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `customer_converted_from_lead` |
| `cost_estimate` | NUMBER |  | 25 | 5 |  |  | `purpose`: `cost_estimate` |
| `country` | VARCHAR2 | 50 |  |  |  |  | `purpose`: `country` |
| `create_date` | TIMESTAMP |  |  |  |  |  | `purpose`: `create_date`; `time_zone`: `gmt` |
| `credithold` | VARCHAR2 | 4 |  |  |  |  | `purpose`: `credit_hold` |
| `creditlimit` | NUMBER |  | 20 | 2 |  |  | `purpose`: `credit_limit` |
| `currency_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `currency_id` |
| `customer_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `customer_id` |
| `customer_type_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `customer_type_id` |
| `date_closed` | TIMESTAMP |  |  |  |  |  | `purpose`: `date_closed`; `time_zone`: `gmt` |
| `date_convsersion` | TIMESTAMP |  |  |  |  |  | `purpose`: `lead_conversion_date`; `time_zone`: `gmt` |
| `date_first_order` | TIMESTAMP |  |  |  |  |  | `purpose`: `first_order_date`; `time_zone`: `gmt` |
| `date_first_sale` | TIMESTAMP |  |  |  |  |  | `purpose`: `first_sale_date`; `time_zone`: `gmt` |
| `date_gross_lead` | TIMESTAMP |  |  |  |  |  | `purpose`: `gross_lead_date`; `time_zone`: `gmt` |
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `date_last_modified` |
| `date_last_order` | TIMESTAMP |  |  |  |  |  | `purpose`: `last_sales_order_date`; `time_zone`: `gmt` |
| `date_last_sale` | TIMESTAMP |  |  |  |  |  | `purpose`: `last_sale_date`; `time_zone`: `gmt`; `applies_to`: `cash_sale`, `invoice` |
| `date_lead` | TIMESTAMP |  |  |  |  |  | `purpose`: `lead_date`; `time_zone`: `gmt` |
| `date_prospect` | TIMESTAMP |  |  |  |  |  | `purpose`: `prospect_date`; `time_zone`: `gmt` |
| `days_overdue` | NUMBER |  | 39 | 0 |  |  | `purpose`: `days_overdue`; `time_zone`: `gmt` |
| `default_order_priority` | NUMBER |  | 20 | 2 |  |  | `purpose`: `default_order_priority` |
| `default_receivables_account_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `default_receivables_account` |
| `deposit_balance` | NUMBER |  | 20 | 2 |  |  | `purpose`: `deposit_balance` |
| `deposit_balance_foreign` | NUMBER |  | 39 | 0 |  |  | `purpose`: `foreign_currency_deposit_balance` |
| `email` | VARCHAR2 | 254 |  |  |  |  | `purpose`: `email` |
| `expected_close` | TIMESTAMP |  |  |  |  |  | `purpose`: `expected_close_date`; `same_as`: `financial_history_start_date` |
| `fax` | VARCHAR2 | 100 |  |  |  |  | `purpose`: `fax` |
| `first_sale_period_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `first_sale_period`; `applies_to`: `cash_sale`, `invoice` |
| `first_visit` | TIMESTAMP |  |  |  |  |  | `purpose`: `first_visit` |
| `firstname` | VARCHAR2 | 32 |  |  |  |  | `purpose`: `first_name` |
| `full_name` | VARCHAR2 | 1800 |  |  |  |  | `purpose`: `full_name` |
| `home_phone` | VARCHAR2 | 100 |  |  |  |  | `purpose`: `home_phone` |
| `is_exempt_time` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `exempt_time` |
| `is_explicit_conversion` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `explicitly_converted_from_lead` |
| `is_job` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `job` |
| `is_person` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `type_company_or_individual` |
| `is_productive_time` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `is_productive_time` |
| `is_utilized_time` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `is_utilized_time` |
| `isemailhtml` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `email_as_html` |
| `isemailpdf` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `email_as_pdf` |
| `isinactive` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `customer_is_inactive` |
| `istaxable` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `taxable` |
| `job_end` | TIMESTAMP |  |  |  |  |  | `purpose`: `end_date` |
| `job_start` | TIMESTAMP |  |  |  |  |  | `purpose`: `start_date` |
| `job_type_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `job_type_id` |
| `last_modified_date` | TIMESTAMP |  |  |  |  |  | `purpose`: `last_modified_date`; `time_zone`: `gmt` |
| `last_sale_period_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `last_sale_period`; `applies_to`: `cash_sale`, `invoice` |
| `last_visit` | TIMESTAMP |  |  |  |  |  | `purpose`: `last_visit` |
| `lastname` | VARCHAR2 | 32 |  |  |  |  | `purpose`: `last_name` |
| `lead_source_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `lead_source_id` |
| `line1` | VARCHAR2 | 150 |  |  |  |  | `purpose`: `address_line_1` |
| `line2` | VARCHAR2 | 150 |  |  |  |  | `purpose`: `address_line_2` |
| `line3` | VARCHAR2 | 150 |  |  |  |  | `purpose`: `address_line_3` |
| `loginaccess` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `give_aacess`; `doc_inconsistency`: `misspelled_access` |
| `middlename` | VARCHAR2 | 32 |  |  |  |  | `purpose`: `middle_name` |
| `mobile_phone` | VARCHAR2 | 100 |  |  |  |  | `purpose`: `mobile_phone` |
| `multiple_price_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `multiple_price_id` |
| `name` | VARCHAR2 | 83 |  |  |  |  | `purpose`: `name` |
| `openbalance` | NUMBER |  | 39 | 0 |  |  | `purpose`: `open_balance`; `same_as`: `financial_history_balance` |
| `openbalance_foreign` | NUMBER |  | 39 | 0 |  |  | `purpose`: `foreign_currency_balance` |
| `parent_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `child_of` |
| `partner_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `partner_id` |
| `payment_terms_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `payment_terms` |
| `phone` | VARCHAR2 | 100 |  |  |  |  | `purpose`: `phone` |
| `primary_contact_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `primary_contact` |
| `print_on_check_as` | VARCHAR2 | 83 |  |  |  |  | `purpose`: `print_on_check_as` |
| `probability` | NUMBER |  | 6 | 2 |  |  | `purpose`: `probability` |
| `projected_end` | TIMESTAMP |  |  |  |  |  | `purpose`: `projected_end` |
| `referrer` | VARCHAR2 | 4000 |  |  |  |  | `purpose`: `referrer` |
| `reminderdays` | NUMBER |  | 39 | 0 |  |  | `purpose`: `reminder_days` |
| `renewal` | TIMESTAMP |  |  |  |  |  | `purpose`: `renewal` |
| `resalenumber` | VARCHAR2 | 20 |  |  |  |  | `purpose`: `resale_number` |
| `rev_rec_forecast_rule_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `revenue_recognition_forecast_rule` |
| `rev_rec_forecast_template` | NUMBER |  | 39 | 0 |  |  | `purpose`: `revenue_recognition_forecast_template` |
| `revenue_estimate` | NUMBER |  | 25 | 5 |  |  | `purpose`: `revenue_estimate` |
| `sales_rep_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `sales_rep_id` |
| `sales_territory_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `sales_territory_id` |
| `salutation` | VARCHAR2 | 30 |  |  |  |  | `purpose`: `salutation` |
| `ship_complete` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `ship_complete` |
| `shipaddress` | VARCHAR2 | 999 |  |  |  |  | `purpose`: `shipping_address` |
| `state` | VARCHAR2 | 50 |  |  |  |  | `purpose`: `state` |
| `status` | VARCHAR2 | 199 |  |  |  |  | `purpose`: `status` |
| `status_descr` | VARCHAR2 | 199 |  |  |  |  | `purpose`: `status_description` |
| `status_probability` | NUMBER |  | 6 | 2 |  |  | `purpose`: `status_probability` |
| `status_read_only` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `read_only_status` |
| `subsidiary_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `subsidiary_id` |
| `tax_item_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `tax_item_id` |
| `third_party_acct` | VARCHAR2 | 32 |  |  |  |  | `purpose`: `third_party_account` |
| `third_party_carrier` | VARCHAR2 | 64 |  |  |  |  | `purpose`: `third_party_carrier` |
| `third_party_country` | VARCHAR2 | 6 |  |  |  |  | `purpose`: `third_party_country` |
| `third_party_zip_code` | VARCHAR2 | 10 |  |  |  |  | `purpose`: `third_party_zip_code` |
| `top_level_parent_id` | NUMBER |  | 39 | 0 | `originating_leads`.`customer_id` | foreign_key | `purpose`: `top_level_parent_id` |
| `unbilled_orders` | NUMBER |  | 20 | 2 |  |  | `purpose`: `unbilled_orders` |
| `unbilled_orders_foreign` | NUMBER |  | 39 | 0 |  |  | `purpose`: `unbilled_foreign_currency_orders` |
| `url` | VARCHAR2 | 100 |  |  |  |  | `purpose`: `web_address` |
| `use_percent_complete_override` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `use_percent_complete_override` |
| `web_lead` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `web_lead` |
| `zipcode` | VARCHAR2 | 36 |  |  |  |  | `purpose`: `zip_code` |

## Foreign keys

| Key | Columns | References table | Referenced columns |
|---|---|---|---|
| `top_level_originating_lead_fk` | `top_level_parent_id` | `originating_leads` | `customer_id` |

## Referenced by

| Key | Table | Columns | Columns here |
|---|---|---|---|
| `Top_level_originating_lead_fk` | `originating_leads` | `top_level_parent_id` | `customer_id` |

## Suspected source errors

> [!SOURCE-ERROR] `misspelled_access` (on `loginaccess`)
<!-- source-error: connect:originating_leads | loginaccess | misspelled_access -->
