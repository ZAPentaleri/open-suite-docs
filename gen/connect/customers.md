# Customers (`customers`)
<!-- id: connect:customers -->

| Fact | Value |
|---|---|
| Table | `customers` |
| Record | `customer` |
| Primary key | `customer_id` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `accountnumber` | VARCHAR2 | 99 |  |  |  |  | `purpose`: `account_number` |
| `alcohol_recipient_type` | VARCHAR2 | 32 |  |  |  |  | `purpose`: `alcohol_recipient_type` |
| `allow_task_time_for_allocation` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `allow_task_time_for_allocation` |
| `altemail` | VARCHAR2 | 254 |  |  |  |  | `purpose`: `alternate_email_address` |
| `alternate_contact_id` | NUMBER |  | 39 | 0 | `contacts`.`contact_id` | foreign_key | `purpose`: `alternate_contact` |
| `altphone` | VARCHAR2 | 100 |  |  |  |  | `purpose`: `alternate_phone` |
| `amount_complete` | NUMBER |  | 5 | 2 |  |  | `purpose`: `amount_complete` |
| `billaddress` | VARCHAR2 | 999 |  |  |  |  | `purpose`: `billing_address` |
| `billing_rate_card_id` | NUMBER |  | 39 | 0 | `billing_rate_cards`.`billing_rate_card_id` | foreign_key | `purpose`: `billing_rate_card_id` |
| `billing_schedule_id` | NUMBER |  | 39 | 0 | `billing_schedule_descriptions`.`billing_schedule_id` | foreign_key | `purpose`: `billing_schedule_id` |
| `billing_schedule_type` | VARCHAR2 | 60 |  |  |  |  | `purpose`: `billing_type` |
| `billing_transaction_type` | VARCHAR2 | 192 |  |  |  |  | `purpose`: `billing_transaction_type` |
| `calculated_end` | TIMESTAMP |  |  |  |  |  | `purpose`: `estimated_end_date` |
| `category_0` | VARCHAR2 | 30 |  |  |  |  | `purpose`: `status_customer_stage` |
| `city` | VARCHAR2 | 50 |  |  |  |  | `purpose`: `city` |
| `comments` | VARCHAR2 | 4000 |  |  |  |  | `purpose`: `comments` |
| `companyname` | VARCHAR2 | 83 |  |  |  |  | `purpose`: `company_name` |
| `consol_days_overdue` | NUMBER |  | 39 | 0 |  |  | `purpose`: `consolidated_days_overdue` |
| `consol_deposit_balance` | NUMBER |  | 39 | 0 |  |  | `purpose`: `consolidated_deposit_balance` |
| `consol_deposit_balance_foreign` | NUMBER |  | 39 | 0 |  |  | `purpose`: `consolidated_deposit_balance`; `value_unit`: `foreign_currency` |
| `consol_openbalance` | NUMBER |  | 39 | 0 |  |  | `purpose`: `consolidated_open_balance`; `value_source`: `sum_of_subcustomer_balances`; `value_unit`: `company_currency`; `caveat`: `rate_from_subcustomer_currency_to_company_currency`; `caveat`: `rate_date_is_subcustomer_billing_date` |
| `consol_openbalance_foreign` | NUMBER |  | 39 | 0 |  |  | `purpose`: `consolidated_open_balance_foreign`; `value_source`: `sum_of_subcustomer_balances`; `value_unit`: `parent_customer_primary_currency`; `caveat`: `rate_from_subcustomer_currency_to_parent_primary_currency`; `caveat`: `rate_date_is_current_date` |
| `consol_unbilled_orders` | NUMBER |  | 39 | 0 |  |  | `purpose`: `consolidated_unbilled_orders` |
| `consol_unbilled_orders_foreign` | NUMBER |  | 39 | 0 |  |  | `purpose`: `consolidated_unbilled_orders`; `value_unit`: `foreign_currency` |
| `converted_to_contact_id` | NUMBER |  | 39 | 0 | `entity`.`entity_id` | foreign_key | `purpose`: `contact_converted_from_lead` |
| `converted_to_id` | NUMBER |  | 39 | 0 | `entity`.`entity_id` | foreign_key | `purpose`: `customer_converted_from_lead` |
| `cost_estimate` | NUMBER |  | 25 | 5 |  |  | `purpose`: `cost_estimate` |
| `country` | VARCHAR2 | 50 |  |  |  |  | `purpose`: `country` |
| `create_date` | TIMESTAMP |  |  |  |  |  | `purpose`: `create_date`; `time_zone`: `gmt` |
| `credithold` | VARCHAR2 | 4 |  |  |  |  | `purpose`: `credit_hold` |
| `creditlimit` | NUMBER |  | 20 | 2 |  |  | `purpose`: `credit_limit` |
| `currency_id` | NUMBER |  | 39 | 0 | `currencies`.`currency_id` | foreign_key | `purpose`: `currency_id` |
| `customer_extid` | VARCHAR2 | 255 |  |  |  |  | `purpose`: `customer_external_id` |
| `customer_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `customer_id` |
| `customer_type_id` | NUMBER |  | 39 | 0 | `customer_types`.`customer_type_id` | foreign_key | `purpose`: `category` |
| `date_calculated_start` | TIMESTAMP |  |  |  |  |  | `purpose`: `calculated_start_date` |
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
| `date_scheduled_end` | TIMESTAMP |  |  |  |  |  | `purpose`: `scheduled_end_date` |
| `days_overdue` | NUMBER |  | 39 | 0 |  |  | `purpose`: `days_overdue`; `time_zone`: `gmt` |
| `default_order_priority` | NUMBER |  | 20 | 2 |  |  | `purpose`: `default_order_priority` |
| `default_receivables_account_id` | NUMBER |  | 39 | 0 | `accounts`.`account_id` | foreign_key | `purpose`: `default_receivables_account` |
| `deposit_balance` | NUMBER |  | 20 | 2 |  |  | `purpose`: `deposit_balance` |
| `deposit_balance_foreign` | NUMBER |  | 39 | 0 |  |  | `purpose`: `foreign_currency_deposit_balance` |
| `email` | VARCHAR2 | 254 |  |  |  |  | `purpose`: `email` |
| `expected_close` | TIMESTAMP |  |  |  |  |  | `purpose`: `expected_close_date`; `same_as`: `financial_history_start_date` |
| `fax` | VARCHAR2 | 100 |  |  |  |  | `purpose`: `fax` |
| `first_sale_period_id` | NUMBER |  | 39 | 0 | `accounting_periods`.`accounting_period_id` | foreign_key | `purpose`: `first_sale_period`; `applies_to`: `cash_sale`, `invoice` |
| `first_visit` | TIMESTAMP |  |  |  |  |  | `purpose`: `first_visit` |
| `firstname` | VARCHAR2 | 32 |  |  |  |  | `purpose`: `first_name` |
| `forecast_based_on_allocations` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `forecast_uses_allocated_time`; `true_means`: `switch_activated` |
| `forecast_charge_run_on_demand` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `forecast_charge_run_on_demand`; `true_means`: `switch_activated` |
| `full_name` | VARCHAR2 | 1800 |  |  |  |  | `purpose`: `full_name` |
| `home_phone` | VARCHAR2 | 100 |  |  |  |  | `purpose`: `home_phone` |
| `is_exempt_time` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `exempt_time` |
| `is_explicit_conversion` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `explicitly_converted_from_lead` |
| `is_job` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `job` |
| `is_limit_time_to_assignees` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `limit_time_and_expense_to_assignees` |
| `is_person` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `type_company_or_individual` |
| `is_productive_time` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `is_productive_time` |
| `is_project_completely_billed` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `is_project_completely_billed` |
| `is_source_item_from_brc` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `service_item_sourced_from_rate_card` |
| `is_utilized_time` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `is_utilized_time` |
| `isemailhtml` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `email_as_html` |
| `isemailpdf` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `email_as_pdf` |
| `isinactive` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `customer_is_inactive` |
| `istaxable` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `taxable` |
| `job_end` | TIMESTAMP |  |  |  |  |  | `purpose`: `end_date` |
| `job_start` | TIMESTAMP |  |  |  |  |  | `purpose`: `start_date` |
| `job_type_id` | NUMBER |  | 39 | 0 | `job_types`.`job_type_id` | foreign_key | `purpose`: `job_type_id` |
| `labor_budget_from_allocations` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `labor_budget_from_allocations` |
| `language_id` | VARCHAR2 | 30 |  |  |  |  | `purpose`: `language_id` |
| `last_modified_date` | TIMESTAMP |  |  |  |  |  | `purpose`: `last_modified_date`; `time_zone`: `gmt` |
| `last_sale_period_id` | NUMBER |  | 39 | 0 | `accounting_periods`.`accounting_period_id` | foreign_key | `purpose`: `last_sale_period`; `applies_to`: `cash_sale`, `invoice` |
| `last_visit` | TIMESTAMP |  |  |  |  |  | `purpose`: `last_visit` |
| `lastname` | VARCHAR2 | 32 |  |  |  |  | `purpose`: `last_name` |
| `lead_source_id` | NUMBER |  | 39 | 0 | `campaigns`.`campaign_id` | foreign_key | `purpose`: `lead_source_id` |
| `line1` | VARCHAR2 | 150 |  |  |  |  | `purpose`: `address_line_1` |
| `line2` | VARCHAR2 | 150 |  |  |  |  | `purpose`: `address_line_2` |
| `line3` | VARCHAR2 | 150 |  |  |  |  | `purpose`: `address_line_3` |
| `loginaccess` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `login_access` |
| `middlename` | VARCHAR2 | 32 |  |  |  |  | `purpose`: `middle_name` |
| `mobile_phone` | VARCHAR2 | 100 |  |  |  |  | `purpose`: `mobile_phone` |
| `multiple_price_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `multiple_price_id` |
| `name` | VARCHAR2 | 83 |  |  |  |  | `purpose`: `name` |
| `openbalance` | NUMBER |  | 39 | 0 |  |  | `purpose`: `open_balance`; `same_as`: `financial_history_balance` |
| `openbalance_foreign` | NUMBER |  | 39 | 0 |  |  | `purpose`: `foreign_currency_balance` |
| `parent_id` | NUMBER |  | 39 | 0 | `entity`.`entity_id` | foreign_key | `purpose`: `child_of` |
| `partner_id` | NUMBER |  | 39 | 0 | `partners`.`partner_id` | foreign_key | `purpose`: `partner_id` |
| `payment_terms_id` | NUMBER |  | 39 | 0 | `payment_terms`.`payment_terms_id` | foreign_key | `purpose`: `payment_terms` |
| `phone` | VARCHAR2 | 100 |  |  |  |  | `purpose`: `phone` |
| `primary_contact_id` | NUMBER |  | 39 | 0 | `contacts`.`contact_id` | foreign_key | `purpose`: `primary_contact` |
| `print_on_check_as` | VARCHAR2 | 83 |  |  |  |  | `purpose`: `print_on_check_as` |
| `probability` | NUMBER |  | 6 | 2 |  |  | `purpose`: `probability` |
| `project_expense_type_id` | NUMBER |  | 39 | 0 | `project_expense_types`.`project_expense_type_id` | foreign_key | `purpose`: `project_expense_type_id` |
| `project_manager_id` | NUMBER |  | 39 | 0 | `employees`.`employee_id` | foreign_key | `purpose`: `project_manager_id` |
| `projected_end` | TIMESTAMP |  |  |  |  |  | `purpose`: `projected_end` |
| `referrer` | VARCHAR2 | 4000 |  |  |  |  | `purpose`: `referrer` |
| `reminderdays` | NUMBER |  | 39 | 0 |  |  | `purpose`: `reminder_days` |
| `renewal` | TIMESTAMP |  |  |  |  |  | `purpose`: `renewal` |
| `represents_subsidiary_id` | NUMBER |  | 39 | 0 | `subsidiaries`.`subsidiary_id` | foreign_key | `purpose`: `represents_subsidiary_id` |
| `resalenumber` | VARCHAR2 | 20 |  |  |  |  | `purpose`: `resale_number` |
| `rev_rec_forecast_rule_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `revenue_recognition_forecast_rule` |
| `rev_rec_forecast_template` | NUMBER |  | 39 | 0 | `revrecschedules`.`schedule_id` | foreign_key | `purpose`: `revenue_recognition_forecast_template` |
| `revenue_estimate` | NUMBER |  | 25 | 5 |  |  | `purpose`: `revenue_estimate` |
| `sales_rep_id` | NUMBER |  | 39 | 0 | `entity`.`entity_id` | foreign_key | `purpose`: `sales_rep_id` |
| `sales_territory_id` | NUMBER |  | 39 | 0 | `territory`.`territory_id` | foreign_key | `purpose`: `sales_territory_id` |
| `salutation` | VARCHAR2 | 30 |  |  |  |  | `purpose`: `salutation` |
| `scheduling_method_id` | VARCHAR2 | 15 |  |  |  |  | `purpose`: `project_scheduling_method` |
| `ship_complete` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `ship_complete` |
| `shipaddress` | VARCHAR2 | 999 |  |  |  |  | `purpose`: `shipping_address` |
| `state` | VARCHAR2 | 50 |  |  |  |  | `purpose`: `state` |
| `status` | VARCHAR2 | 199 |  |  |  |  | `purpose`: `status` |
| `status_descr` | VARCHAR2 | 199 |  |  |  |  | `purpose`: `status_description` |
| `status_probability` | NUMBER |  | 6 | 2 |  |  | `purpose`: `status_probability` |
| `status_read_only` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `read_only_status` |
| `subsidiary_id` | NUMBER |  | 39 | 0 | `subsidiaries`.`subsidiary_id` | foreign_key | `purpose`: `subsidiary_id` |
| `tax_item_id` | NUMBER |  | 39 | 0 | `items`.`item_id` | foreign_key | `purpose`: `tax_item_id` |
| `third_party_acct` | VARCHAR2 | 32 |  |  |  |  | `purpose`: `third_party_account` |
| `third_party_carrier` | VARCHAR2 | 64 |  |  |  |  | `purpose`: `third_party_carrier` |
| `third_party_country` | VARCHAR2 | 6 |  |  |  |  | `purpose`: `third_party_country` |
| `third_party_zip_code` | VARCHAR2 | 10 |  |  |  |  | `purpose`: `third_party_zip_code` |
| `time_approval_type_id` | NUMBER |  | 39 | 0 | `project_time_approval_types`.`project_time_approval_type_id` | foreign_key | `purpose`: `time_approval_type_id` |
| `top_level_parent_id` | NUMBER |  | 39 | 0 | `customers`.`customer_id` | foreign_key | `purpose`: `top_level_parent_id` |
| `unbilled_orders` | NUMBER |  | 20 | 2 |  |  | `purpose`: `unbilled_orders` |
| `unbilled_orders_foreign` | NUMBER |  | 39 | 0 |  |  | `purpose`: `unbilled_foreign_currency_orders` |
| `url` | VARCHAR2 | 100 |  |  |  |  | `purpose`: `web_address` |
| `use_calculated_billing_budget` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `use_calculated_billing_budget` |
| `use_calculated_cost_budget` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `use_calculated_cost_budget` |
| `use_percent_complete_override` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `use_percent_complete_override` |
| `vat_reg_number` | VARCHAR2 | 20 |  |  |  |  | `purpose`: `vat_identification_number` |
| `web_lead` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `web_lead` |
| `zipcode` | VARCHAR2 | 36 |  |  |  |  | `purpose`: `zip_code` |

## Foreign keys

| Key | Columns | References table | Referenced columns |
|---|---|---|---|
| `customers_accounting_periods_fk` | `first_sale_period_id` | `accounting_periods` | `accounting_period_id` |
| `customers_accounting_periods_fk_2` | `last_sale_period_id` | `accounting_periods` | `accounting_period_id` |
| `customers_accounts_fk` | `default_receivables_account_id` | `accounts` | `account_id` |
| `customers_billing_rate_cards_fk` | `billing_rate_card_id` | `billing_rate_cards` | `billing_rate_card_id` |
| `customers_billing_schedule_descriptions_fk` | `billing_schedule_id` | `billing_schedule_descriptions` | `billing_schedule_id` |
| `customers_campaigns_fk` | `lead_source_id` | `campaigns` | `campaign_id` |
| `customers_contacts_fk` | `primary_contact_id` | `contacts` | `contact_id` |
| `customers_contacts_fk_2` | `alternate_contact_id` | `contacts` | `contact_id` |
| `customers_currencies_fk` | `currency_id` | `currencies` | `currency_id` |
| `customers_customer_types_fk` | `customer_type_id` | `customer_types` | `customer_type_id` |
| `customers_employees_fk` | `project_manager_id` | `employees` | `employee_id` |
| `customers_entity_fk` | `converted_to_id` | `entity` | `entity_id` |
| `customers_entity_fk_2` | `converted_to_contact_id` | `entity` | `entity_id` |
| `customers_entity_fk_3` | `sales_rep_id` | `entity` | `entity_id` |
| `customers_entity_fk_4` | `parent_id` | `entity` | `entity_id` |
| `customers_items_fk` | `tax_item_id` | `items` | `item_id` |
| `customers_job_types_fk` | `job_type_id` | `job_types` | `job_type_id` |
| `customers_partners_fk` | `partner_id` | `partners` | `partner_id` |
| `customers_payment_terms_fk` | `payment_terms_id` | `payment_terms` | `payment_terms_id` |
| `customers_project_expense_types_fk` | `project_expense_type_id` | `project_expense_types` | `project_expense_type_id` |
| `customers_project_time_approval_types_fk` | `time_approval_type_id` | `project_time_approval_types` | `project_time_approval_type_id` |
| `customers_revrecschedules_fk` | `rev_rec_forecast_template` | `revrecschedules` | `schedule_id` |
| `customers_subsidiaries_fk` | `subsidiary_id` | `subsidiaries` | `subsidiary_id` |
| `customers_subsidiaries_fk_2` | `represents_subsidiary_id` | `subsidiaries` | `subsidiary_id` |
| `customers_territory_fk` | `sales_territory_id` | `territory` | `territory_id` |
| `top_level_customer_fk` | `top_level_parent_id` | `customers` | `customer_id` |

## Referenced by

| Key | Table | Columns | Columns here |
|---|---|---|---|
| `Billing_rate_cards_customers_fk` | `billing_rate_cards` | `customer_id` | `customer_id` |
| `Billing_subscriptions_customers_fk` | `billing_subscriptions` | `customer_id` | `customer_id` |
| `Budget_customers_fk` | `budget` | `customer_id` | `customer_id` |
| `Customer_currencies_customers_fk` | `customer_currencies` | `customer_id` | `customer_id` |
| `Customer_subsidiary_map_customers_fk` | `customer_subsidiary_map` | `customer_id` | `customer_id` |
| `Employee_time_customers_fk` | `employee_time` | `customer_id` | `customer_id` |
| `Project_revenue_rules_customers_fk` | `project_revenue_rules` | `project_id` | `customer_id` |
| `Purchase_charge_rules_customers_fk` | `purchase_charge_rules` | `project_id` | `customer_id` |
| `Top_level_customer_fk` | `customers` | `top_level_parent_id` | `customer_id` |
