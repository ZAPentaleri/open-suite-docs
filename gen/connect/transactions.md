# Transactions (`transactions`)
<!-- id: connect:transactions -->

| Fact | Value |
|---|---|
| Table | `transactions` |
| Record | `transaction` |
| Primary key | `transaction_id` |
| Domains | Campaignevents, Expense_amortization, General_accounting, Invoice_with_amortization, Multibooks, Revenue_recognition |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `account_based_number` | VARCHAR2 | 138 |  |  |  |  | `purpose`: `account_based_number` |
| `accounting_book_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `accounting_book_id` |
| `accounting_period_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `accounting_period_id` |
| `acct_corp_card_expenses_id` | NUMBER |  | 39 | 0 | `accounts`.`account_id` | foreign_key | `purpose`: `corporate_card_expense_account` |
| `amount_unbilled` | NUMBER |  | 20 | 2 |  |  | `purpose`: `amount_unbilled` |
| `bill_pay_transaction` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `bill_pay_transaction` |
| `billaddress` | VARCHAR2 | 999 |  |  |  |  | `purpose`: `billing_address` |
| `billing_account_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `billing_account_id` |
| `billing_instructions` | VARCHAR2 | 999 |  |  |  |  | `purpose`: `billing_instructions` |
| `buildable` | NUMBER |  | 39 | 0 |  |  | `purpose`: `assemblies_buildable_from_current_supply` |
| `bulk_submission_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `bulk_submission_id` |
| `carrier` | VARCHAR2 | 100 |  |  |  |  | `purpose`: `carrier` |
| `closed` | TIMESTAMP |  |  |  |  |  | `purpose`: `closed` |
| `company_status_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `company_status_id` |
| `contract_cost_amount` | NUMBER |  | 20 | 2 |  |  | `purpose`: `contract_cost_amount` |
| `contract_defer_expense_acct_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `contract_deferred_expense_account` |
| `contract_expense_acct_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `contract_expense_account_id` |
| `contract_expense_src_acct_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `contract_acquisition_expense_source_account` |
| `create_date` | TIMESTAMP |  |  |  |  |  | `purpose`: `creation_date`; `time_zone`: `gmt` |
| `created_by_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `id_of_the_creator` |
| `created_from_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `created_from_id` |
| `currency_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `currency_id` |
| `custom_form_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `custom_form_id` |
| `date_actual_prod_end` | TIMESTAMP |  |  |  |  |  | `purpose`: `the_date_production_ends` |
| `date_actual_prod_start` | TIMESTAMP |  |  |  |  |  | `purpose`: `the_date_production_starts` |
| `date_as_of` | TIMESTAMP |  |  |  |  |  | `purpose`: `as_of_date` |
| `date_bid_close` | TIMESTAMP |  |  |  |  |  | `purpose`: `bid_close_date` |
| `date_bid_open` | TIMESTAMP |  |  |  |  |  | `purpose`: `bid_open_date` |
| `date_contract_cost_accrual` | TIMESTAMP |  |  |  |  |  | `purpose`: `date_contract_cost_accrual` |
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `date_last_modified`; `time_zone`: `gmt` |
| `date_tax_point` | TIMESTAMP |  |  |  |  |  | `purpose`: `tax_point_date`; `effect`: `determines_taxability_rules` |
| `due_date` | TIMESTAMP |  |  |  |  |  | `purpose`: `due_date` |
| `email` | VARCHAR2 | 256 |  |  |  |  | `purpose`: `email` |
| `end_date` | TIMESTAMP |  |  |  |  |  | `purpose`: `end_date` |
| `entity_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `entity_id` |
| `entity_tax_reg_num` | VARCHAR2 | 30 |  |  |  |  | `purpose`: `entity_s_tax_registration_number` |
| `exchange_rate` | NUMBER |  | 30 | 15 |  |  | `purpose`: `exchange_rate` |
| `expected_close` | TIMESTAMP |  |  |  |  |  | `purpose`: `expected_close` |
| `external_ref_number` | VARCHAR2 | 138 |  |  |  |  | `purpose`: `external_reference_number` |
| `fax` | VARCHAR2 | 100 |  |  |  |  | `purpose`: `fax` |
| `fob` | VARCHAR2 | 13 |  |  |  |  | `purpose`: `fob` |
| `forecast_type` | VARCHAR2 | 384 |  |  |  |  | `purpose`: `forecast_type` |
| `include_in_forecast` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `include_in_forecast` |
| `incoterm` | VARCHAR2 | 100 |  |  |  |  | `purpose`: `international_commercial_term_incoterm` |
| `intercompany_transaction_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `intercompany_transaction_id` |
| `is_actualprodenddate_manual` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `actual_production_end_date_manual` |
| `is_actualprodstartdate_manual` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `actual_production_start_date_manual` |
| `is_advanced_intercompany` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `is_advanced_intercompany_journal` |
| `is_autocalculate_lag` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `calculate_operation_lag_times` |
| `is_compliant` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `transaction_compliant` |
| `is_created_from_merge` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `created_from_merge` |
| `is_cross_sub_transactions` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `cross_subsidiary_transaction` |
| `is_finance_charge` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `is_finance_charge_line` |
| `is_firmed` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `transaction_firmed` |
| `is_in_transit_payment` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `payment_in_transit` |
| `is_intercompany` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `intercompany_transaction` |
| `is_merged_into_arrangements` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `to_be_merged_into_arrangements` |
| `is_non_posting` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `non_posting_transaction` |
| `is_override_installments` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `installments_overridden` |
| `is_payment_hold` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `payment_hold` |
| `is_recurring_bill` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `recurring_bill` |
| `is_reversal` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `is_reversal` |
| `is_ship_complete` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `is_ship_complete` |
| `is_tax_point_date_override` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `tax_point_date_set_manually` |
| `is_tax_reg_override` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `tax_registration_override_applied` |
| `is_wip` | VARCHAR2 | 1 |  |  |  |  | `purpose`: `is_wip` |
| `item_revision` | NUMBER |  | 39 | 0 |  |  | `purpose`: `item_revision` |
| `job_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `name_job_id` |
| `landed_cost_allocation_method` | VARCHAR2 | 8 |  |  |  |  | `purpose`: `landed_cost_allocation_method`; `value_examples`: `weight`, `quantity`, `value` |
| `last_modified_date` | TIMESTAMP |  |  |  |  |  | `purpose`: `date_last_modified`; `time_zone`: `gmt` |
| `lead_source_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `lead_source_id` |
| `location_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `location_id` |
| `memo` | VARCHAR2 | 4000 |  |  |  |  | `purpose`: `memo` |
| `memorized` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `memorized` |
| `message` | VARCHAR2 | 999 |  |  |  |  | `purpose`: `message` |
| `needs_bill` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `needs_to_be_billed` |
| `needs_revenue_commitment` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `needs_revenue_commitment` |
| `number_of_pricing_tiers` | NUMBER |  | 39 | 0 |  |  | `purpose`: `number_of_pricing_tiers` |
| `opening_balance_transaction` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `opening_balance_transaction` |
| `ownership_transfer_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `ownership_id` |
| `packing_list_instructions` | VARCHAR2 | 999 |  |  |  |  | `purpose`: `packing_list_instructions` |
| `partner_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `partner_id` |
| `payment_terms_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `payment_terms_id` |
| `pn_ref_num` | VARCHAR2 | 100 |  |  |  |  | `purpose`: `p_n_reference_number` |
| `probability` | NUMBER |  | 6 | 2 |  |  | `purpose`: `probability` |
| `product_label_instructions` | VARCHAR2 | 999 |  |  |  |  | `purpose`: `product_label_instructions` |
| `projected_total` | NUMBER |  | 20 | 2 |  |  | `purpose`: `projected_total` |
| `promotion_code_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `promotion_code_id` |
| `promotion_code_instance_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `promotion_code_insurance_id`; `doc_inconsistency`: `insurance_likely_means_instance` |
| `purchase_order_instructions` | VARCHAR2 | 999 |  |  |  |  | `purpose`: `purchase_order_instructions` |
| `related_tranid` | VARCHAR2 | 138 |  |  |  |  | `purpose`: `related_transaction_id` |
| `renewal` | TIMESTAMP |  |  |  |  |  | `purpose`: `renewal_date` |
| `revenue_commitment_status` | VARCHAR2 | 480 |  |  |  |  | `purpose`: `revenue_committed_status` |
| `revenue_committed` | TIMESTAMP |  |  |  |  |  | `purpose`: `revenue_committed` |
| `revenue_status` | VARCHAR2 | 480 |  |  |  |  | `purpose`: `revenue_status` |
| `reversing_transaction_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `reversing_transaction_id` |
| `sales_channel_id` | NUMBER |  | 39 | 0 | `sales_channels`.`sales_channel_id` | foreign_key | `purpose`: `sales_channel_id` |
| `sales_effective_date` | TIMESTAMP |  |  |  |  |  | `purpose`: `commission_sales_effective_date` |
| `sales_rep_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `sales_rep_id` |
| `scheduling_method_id` | VARCHAR2 | 15 |  |  |  |  | `purpose`: `scheduling_method_id` |
| `shipaddress` | VARCHAR2 | 999 |  |  |  |  | `purpose`: `shipping_address` |
| `shipment_received` | TIMESTAMP |  |  |  |  |  | `purpose`: `shipment_received` |
| `shipping_item_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `shipping_item_id` |
| `start_date` | TIMESTAMP |  |  |  |  |  | `purpose`: `start_date` |
| `status` | VARCHAR2 | 4000 |  |  |  |  | `purpose`: `status` |
| `tax_reg_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `tax_registration`; `not_supported`: `column_no_longer_supported` |
| `title` | VARCHAR2 | 200 |  |  |  |  | `purpose`: `title` |
| `trandate` | TIMESTAMP |  |  |  |  |  | `purpose`: `transaction_date` |
| `tranid` | VARCHAR2 | 138 |  |  |  |  | `purpose`: `check_or_document` |
| `trans_is_vsoe_bundle` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `transaction_is_vsoe_bundle` |
| `transaction_extid` | VARCHAR2 | 255 |  |  |  |  | `purpose`: `transaction_external_id` |
| `transaction_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `internal_transaction_id` |
| `transaction_number` | VARCHAR2 | 138 |  |  |  |  | `purpose`: `transaction_number` |
| `transaction_partner` | VARCHAR2 | 40 |  |  |  |  | `purpose`: `transaction_partner` |
| `transaction_source` | VARCHAR2 | 4000 |  |  |  |  | `purpose`: `transaction_source`; `value_kind`: `web_site_name_when_from_web_store` |
| `transaction_type` | VARCHAR2 | 192 |  |  |  |  | `purpose`: `transaction_type` |
| `transaction_website` | NUMBER |  | 39 | 0 |  |  | `purpose`: `source_web_site_id` |
| `transfer_location` | NUMBER |  | 39 | 0 |  |  | `purpose`: `transfer_location` |
| `use_item_cost_as_transfer_cost` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `use_item_cost_as_transfer_cost`; `configured_by`: `use_item_cost_as_transfer_cost_preference` |
| `visible_in_customer_center` | VARCHAR2 | 1 |  |  |  |  | `purpose`: `visible_in_customer_center` |
| `weighted_total` | NUMBER |  | 39 | 0 |  |  | `purpose`: `weighted_total` |

## Foreign keys

| Key | Columns | References table | Referenced columns |
|---|---|---|---|
| `transactions_accounts_fk` | `acct_corp_card_expenses_id` | `accounts` | `account_id` |
| `transactions_sales_channels_fk` | `sales_channel_id` | `sales_channels` | `sales_channel_id` |

## Referenced by

| Key | Table | Columns | Columns here |
|---|---|---|---|
| `Amortization_sched_lines_transactions_fk` | `amortization_sched_lines` | `journal_id` | `transaction_id` |
| `Billing_subscription_lines_transactions_fk` | `billing_subscription_lines` | `purchase_order_id` | `transaction_id` |
| `Billing_subscription_lines_transactions_fk_2` | `billing_subscription_lines` | `sales_order_id` | `transaction_id` |
| `Billing_subscriptions_transactions_fk` | `billing_subscriptions` | `sales_order_id` | `transaction_id` |
| `Campaignresponsehistory_transactions_fk` | `campaignresponsehistory` | `transaction_id` | `transaction_id` |
| `Employee_time_transactions_fk` | `employee_time` | `transaction_id` | `transaction_id` |
| `Expense_plan_lines_transactions_fk` | `expense_plan_lines` | `journal_id` | `transaction_id` |
| `Expense_plans_transactions_fk` | `expense_plans` | `related_revenue_arrangement_id` | `transaction_id` |
| `Expense_reports_transactions_fk` | `expense_reports` | `expense_report_id` | `transaction_id` |
| `Notes_user_transactions_fk` | `notes_user` | `transaction_id` | `transaction_id` |
| `Opportunities_transactions_fk` | `opportunities` | `reversing_transaction_id` | `transaction_id` |
| `Revenue_plan_lines_transactions_fk` | `revenue_plan_lines` | `journal_id` | `transaction_id` |
| `Revenue_plan_version_lines_transactions_fk` | `revenue_plan_version_lines` | `journal_id` | `transaction_id` |
| `Revrecschedulelines_transactions_fk` | `revrecschedulelines` | `journal_id` | `transaction_id` |
| `System_notes_custom_transactions_fk` | `system_notes_custom` | `transaction_id` | `transaction_id` |
| `System_notes_transactions_fk` | `system_notes` | `transaction_id` | `transaction_id` |
| `Transaction_address_transactions_fk` | `transaction_address` | `transaction_id` | `transaction_id` |
| `Transaction_book_map_transactions_fk` | `transaction_book_map` | `transaction_id` | `transaction_id` |
| `Transaction_history_transactions_fk` | `transaction_history` | `transaction_id` | `transaction_id` |
| `Transaction_lines_transactions_fk` | `transaction_lines` | `transaction_id` | `transaction_id` |
| `Transaction_links_applied_transactions_fk` | `transaction_links` | `applied_transaction_id` | `transaction_id` |
| `Transaction_links_original_transactions_fk` | `transaction_links` | `original_transaction_id` | `transaction_id` |
| `Transaction_shipping_groups_transactions_fk` | `transaction_shipping_groups` | `transaction_id` | `transaction_id` |

## Suspected source errors

> [!SOURCE-ERROR] `insurance_likely_means_instance` (on `promotion_code_instance_id`)
<!-- source-error: connect:transactions | promotion_code_instance_id | insurance_likely_means_instance -->
