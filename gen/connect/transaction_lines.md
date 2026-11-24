# Transaction_lines (`transaction_lines`)
<!-- id: connect:transaction_lines -->

| Fact | Value |
|---|---|
| Table | `transaction_lines` |
| Primary key | `transaction_id`, `transaction_line_id` |
| Domains | Expense_amortization, General_accounting, Invoice_with_amortization, Multibooks, Revenue_recognition |
| Tags | `composite_primary_key` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `account_id` | NUMBER |  | 39 | 0 | `accounts`.`account_id` | foreign_key | `purpose`: `account_id` |
| `alt_sales_amount` | NUMBER |  | 20 | 2 |  |  | `purpose`: `alt_sales_amount` |
| `amortization_residual` | VARCHAR2 | 42 |  |  |  |  | `purpose`: `amortization_residual` |
| `amount` | NUMBER |  | 20 | 2 |  |  | `purpose`: `amount` |
| `amount_foreign` | NUMBER |  | 20 | 2 |  |  | `purpose`: `foreign_currency_amount` |
| `amount_foreign_linked` | NUMBER |  | 30 | 15 |  |  | `purpose`: `foreign_currency_amount_linked` |
| `amount_linked` | NUMBER |  | 20 | 2 |  |  | `purpose`: `amount_linked` |
| `amount_pending` | NUMBER |  | 20 | 2 |  |  | `purpose`: `amount_pending` |
| `amount_settlement` | NUMBER |  | 20 | 2 |  |  | `purpose`: `amount_settlement` |
| `amount_taxable` | NUMBER |  | 20 | 2 |  |  | `purpose`: `amount_taxable` |
| `amount_taxed` | NUMBER |  | 20 | 2 |  |  | `purpose`: `amount_taxed` |
| `bill_variance_status` | VARCHAR2 | 128 |  |  |  |  | `purpose`: `bill_variance_status` |
| `billing_schedule_id` | NUMBER |  | 39 | 0 | `billing_schedule_descriptions`.`billing_schedule_id` | foreign_key | `purpose`: `billing_schedule_id` |
| `billing_subsidiary_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `billing_subsidiary_id` |
| `bom_quantity` | NUMBER |  | 18 | 8 |  |  | `purpose`: `bom_quantity` |
| `catch_up_period_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `catch_up_period_id` |
| `charge_rule_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `charge_rule_id` |
| `charge_type` | NUMBER |  | 39 | 0 |  |  | `purpose`: `charge_type` |
| `class_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `class_id` |
| `company_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `company_id` |
| `component_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `component_id` |
| `component_yield` | NUMBER |  | 39 | 0 |  |  | `purpose`: `component_yield` |
| `cost_estimate_type` | VARCHAR2 | 200 |  |  |  |  | `purpose`: `cost_estimate_type` |
| `date_cleared` | TIMESTAMP |  |  |  |  |  | `purpose`: `date_cleared` |
| `date_closed` | TIMESTAMP |  |  |  |  |  | `purpose`: `date_closed` |
| `date_created` | TIMESTAMP |  |  |  |  |  | `purpose`: `date_created` |
| `date_last_modified_gmt` | TIMESTAMP |  |  |  |  |  | `purpose`: `last_modified_date`; `time_zone`: `gmt` |
| `date_requested` | TIMESTAMP |  |  |  |  |  | `purpose`: `date_requested` |
| `date_revenue_committed` | TIMESTAMP |  |  |  |  |  | `purpose`: `date_revenue_committed` |
| `delay_rev_rec` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `delay_revenue_recognition` |
| `department_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `department_id` |
| `do_not_display_line` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `do_not_display_line` |
| `do_not_print_line` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `do_not_print_line` |
| `do_restock` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `do_restock` |
| `estimated_cost` | NUMBER |  | 20 | 2 |  |  | `purpose`: `estimated_cost` |
| `estimated_cost_foreign` | NUMBER |  | 20 | 2 |  |  | `purpose`: `estimated_cost`; `value_unit`: `foreign_currency` |
| `expected_receipt_date` | TIMESTAMP |  |  |  |  |  | `purpose`: `expected_receipt_date` |
| `expense_category_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `expense_category_id` |
| `gl_number` | VARCHAR2 | 80 |  |  |  |  | `purpose`: `gl_number` |
| `gl_sequence` | VARCHAR2 | 256 |  |  |  |  | `purpose`: `gl_sequence` |
| `gl_sequence_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `gl_sequence_id` |
| `gross_amount` | NUMBER |  | 39 | 0 |  |  | `purpose`: `gross_amount` |
| `has_cost_line` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `has_cost_line` |
| `is_allocation` | VARCHAR2 | 1 |  |  |  |  | `purpose`: `is_allocation` |
| `is_amortization_rev_rec` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `is_amortization_revenue_recognition` |
| `is_commitment_confirmed` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `is_commitment_confirmed` |
| `is_cost_line` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `is_cost_line` |
| `is_custom_line` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `is_custom_line` |
| `is_exclude_from_rate_request` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `excluded_from_rate_request` |
| `is_fx_variance` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `fx_variance_line` |
| `is_item_value_adjustment` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `is_item_value_adjustment` |
| `is_landed_cost` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `landed_cost_line`; `doc_inconsistency`: `refers_to_note_not_in_table` |
| `is_scrap` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `is_scrap` |
| `is_vsoe_allocation_line` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `line_is_vsoe_allocation` |
| `isbillable` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `billable` |
| `iscleared` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `is_cleared` |
| `isnonreimbursable` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `is_non_reinbursable`; `doc_inconsistency`: `misspelled_reimbursable` |
| `istaxable` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `taxable` |
| `item_count` | NUMBER |  | 18 | 8 |  |  | `purpose`: `item_count` |
| `item_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `item_id` |
| `item_received` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `item_received` |
| `item_source` | VARCHAR2 | 30 |  |  |  |  | `purpose`: `item_source` |
| `item_unit_price` | VARCHAR2 | 42 |  |  |  |  | `purpose`: `item_unit_price` |
| `kit_part_number` | NUMBER |  | 39 | 0 |  |  | `purpose`: `kit_name_number` |
| `landed_cost_source_line_id` | NUMBER |  | 38 | 0 |  |  | `purpose`: `landed_cost_source_line`; `same_as`: `transaction_lines.transaction_line_id` |
| `location_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `location_id` |
| `match_bill_to_receipt` | VARCHAR2 | 1 |  |  |  |  | `purpose`: `match_bill_to_receipt` |
| `memo` | VARCHAR2 | 4000 |  |  |  |  | `purpose`: `memo` |
| `needs_revenue_element` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `needs_revenue_element` |
| `net_amount` | NUMBER |  | 39 | 0 |  |  | `purpose`: `net_amount` |
| `net_amount_foreign` | NUMBER |  | 39 | 0 |  |  | `purpose`: `net_amount`; `value_unit`: `foreign_currency` |
| `non_posting_line` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `non_posting_line` |
| `number_billed` | NUMBER |  | 18 | 8 |  |  | `purpose`: `number_billed` |
| `operation_sequence_number` | NUMBER |  | 39 | 0 |  |  | `purpose`: `operation_sequence_number` |
| `order_allocation_strategy_id` | NUMBER |  | 39 | 0 | `order_allocation_strategies`.`order_allocation_strategy_id` | foreign_key | `purpose`: `order_allocation_strategy_id` |
| `order_priority` | NUMBER |  | 20 | 2 |  |  | `purpose`: `order_priority` |
| `payment_method_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `payment_method_id` |
| `payroll_item_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `payroll_item_id` |
| `payroll_wage_base_amount` | NUMBER |  | 20 | 2 |  |  | `purpose`: `payroll_wage_base_amount` |
| `payroll_year_to_date_amount` | NUMBER |  | 20 | 2 |  |  | `purpose`: `payroll_ytd_amount` |
| `period_closed` | TIMESTAMP |  |  |  |  |  | `purpose`: `period_closed` |
| `price_type_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `price_type_id` |
| `project_task_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `project_task_id` |
| `purchase_contract_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `purchase_contract_id` |
| `quantity_allocated` | NUMBER |  | 30 | 15 |  |  | `purpose`: `quantity_allocated` |
| `quantity_committed` | NUMBER |  | 30 | 15 |  |  | `purpose`: `quantity_committed` |
| `quantity_packed` | NUMBER |  | 18 | 8 |  |  | `purpose`: `quantity_packed` |
| `quantity_picked` | NUMBER |  | 18 | 8 |  |  | `purpose`: `quantity_picked` |
| `quantity_received_in_shipment` | NUMBER |  | 18 | 8 |  |  | `purpose`: `quantity_received_in_shipment` |
| `receivebydate` | TIMESTAMP |  |  |  |  |  | `purpose`: `receive_by_date` |
| `reimbursement_type` | VARCHAR2 | 128 |  |  |  |  | `purpose`: `reimbursement_type` |
| `related_company_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `related_company_id` |
| `rev_rec_end_date` | TIMESTAMP |  |  |  |  |  | `purpose`: `revenue_recognition_end_date` |
| `rev_rec_rule_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `revenue_recognition_rule_id` |
| `rev_rec_start_date` | TIMESTAMP |  |  |  |  |  | `purpose`: `revenue_recognition_start_date` |
| `revenue_element_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `revenue_element_id` |
| `schedule_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `line_schedule`; `allowed_values_listed_in`: `revenue_recognition_schedules`, `amortization_schedules` |
| `shipdate` | TIMESTAMP |  |  |  |  |  | `purpose`: `ship_date` |
| `shipment_received` | TIMESTAMP |  |  |  |  |  | `purpose`: `shipment_received` |
| `shipping_group_id` | NUMBER |  | 39 | 0 | `transaction_shipping_groups`.`shipping_group_id` | foreign_key | `purpose`: `shipping_group_id` |
| `source_subsidiary_id` | NUMBER |  | 39 | 0 | `subsidiaries`.`subsidiary_id` | foreign_key | `purpose`: `source_subsidiary_id` |
| `subscription_line_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `subscription_line_id` |
| `subsidiary_id` | NUMBER |  | 39 | 0 | `subsidiaries`.`subsidiary_id` | foreign_key | `purpose`: `subsidiary_id` |
| `tax_item_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `tax_item_id` |
| `tax_type` | VARCHAR2 | 64 |  |  |  |  | `purpose`: `tax_type` |
| `term_in_months` | NUMBER |  | 39 | 0 |  |  | `purpose`: `term_in_months` |
| `tobeemailed` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `to_be_emailed` |
| `tobefaxed` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `to_be_faxed` |
| `tobeprinted` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `to_be_printed` |
| `transaction_discount_line` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `transaction_discount_line` |
| `transaction_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `transaction_id` |
| `transaction_line_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `transaction_line_id` |
| `transaction_order` | NUMBER |  | 39 | 0 |  |  | `purpose`: `transaction_order` |
| `transfer_order_item_line` | NUMBER |  | 39 | 0 |  |  | `purpose`: `transfer_order_item_line` |
| `transfer_order_line_type` | VARCHAR2 | 25 |  |  |  |  | `purpose`: `transfer_order_line_type` |
| `unique_key` | NUMBER |  | 39 | 0 |  |  | `purpose`: `unique_key` |
| `unit_cost_override` | NUMBER |  | 30 | 15 |  |  | `purpose`: `unit_cost_override` |
| `unit_of_measure_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `unit_of_measure_id` |
| `vsoe_allocation` | NUMBER |  | 20 | 2 |  |  | `purpose`: `vsoe_allocation` |
| `vsoe_amt` | NUMBER |  | 20 | 2 |  |  | `purpose`: `vsoe_amount` |
| `vsoe_deferral` | VARCHAR2 | 28 |  |  |  |  | `purpose`: `vsoe_deferral` |
| `vsoe_delivered` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `vsoe_delivered` |
| `vsoe_discount` | VARCHAR2 | 12 |  |  |  |  | `purpose`: `vsoe_discount` |
| `vsoe_price` | NUMBER |  | 30 | 15 |  |  | `purpose`: `vsoe_price` |

## Foreign keys

| Key | Columns | References table | Referenced columns |
|---|---|---|---|
| `transaction_lines_accounts_fk` | `account_id` | `accounts` | `account_id` |
| `transaction_lines_billing_schedule_descriptions_fk` | `billing_schedule_id` | `billing_schedule_descriptions` | `billing_schedule_id` |
| `transaction_lines_order_allocation_strategies_fk` | `order_allocation_strategy_id` | `order_allocation_strategies` | `order_allocation_strategy_id` |
| `transaction_lines_subsidiaries_fk_1` | `subsidiary_id` | `subsidiaries` | `subsidiary_id` |
| `transaction_lines_subsidiaries_fk_2` | `source_subsidiary_id` | `subsidiaries` | `subsidiary_id` |
| `transaction_lines_transaction_shipping_groups_fk` | `transaction_id`, `shipping_group_id` | `transaction_shipping_groups` | `transaction_id`, `shipping_group_id` |
| `transaction_lines_transactions_fk` | `transaction_id` | `transactions` | `transaction_id` |

## Referenced by

| Key | Table | Columns | Columns here |
|---|---|---|---|
| `Employee_time_transaction_lines_fk` | `employee_time` | `transaction_id`, `transaction_line_id` | `transaction_id`, `transaction_line_id` |
| `Expense_plans_transaction_lines_fk` | `expense_plans` | `transaction_doc_id` | `transaction_id` |
| `Expense_plans_transaction_lines_fk_1` | `expense_plans` | `transaction_line_id` | `transaction_line_id` |
| `System_notes_custom_transaction_lines_fk` | `system_notes_custom` | `line_transaction_id`, `line_id` | `transaction_id`, `transaction_line_id` |
| `System_notes_transaction_lines_fk` | `system_notes` | `line_transaction_id`, `line_id` | `transaction_id`, `transaction_line_id` |
| `Transaction_line_book_map_transaction_lines_fk` | `transaction_line_book_map` | `transaction_id`, `transaction_line_id` | `transaction_id`, `transaction_line_id` |
| `Transaction_links_applied_transaction_lines_fk` | `transaction_links` | `applied_transaction_id`, `applied_transaction_line_id` | `transaction_id`, `transaction_line_id` |
| `Transaction_links_original_transaction_lines_fk` | `transaction_links` | `original_transaction_id`, `original_transaction_line_id` | `transaction_id`, `transaction_line_id` |
| `Transaction_tax_detail_transaction_lines_fk` | `transaction_tax_detail` | `transaction_id`, `transaction_line_id` | `transaction_id`, `transaction_line_id` |

## Suspected source errors

> [!SOURCE-ERROR] `refers_to_note_not_in_table` (on `is_landed_cost`)
<!-- source-error: connect:transaction_lines | is_landed_cost | refers_to_note_not_in_table -->

> [!SOURCE-ERROR] `misspelled_reimbursable` (on `isnonreimbursable`)
<!-- source-error: connect:transaction_lines | isnonreimbursable | misspelled_reimbursable -->
