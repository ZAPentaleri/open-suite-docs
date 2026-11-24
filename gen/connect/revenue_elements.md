# Revenue_elements (`revenue_elements`)
<!-- id: connect:revenue_elements -->

| Fact | Value |
|---|---|
| Table | `revenue_elements` |
| Primary key | `revenue_element_id` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `accounting_book_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `accounting_book_id` |
| `accounting_period_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `accounting_period_id` |
| `allocated_contract_cost_amount` | NUMBER |  | 20 | 2 |  |  | `purpose`: `allocated_contract_cost_amount` |
| `allocation_amount` | NUMBER |  | 20 | 2 |  |  | `purpose`: `allocation_amount` |
| `allocation_amount_foreign` | NUMBER |  | 20 | 2 |  |  | `purpose`: `allocation_foreign_currency_amount` |
| `allocation_type` | VARCHAR2 | 128 |  |  |  |  | `purpose`: `allocation_type` |
| `alternate_quantity` | NUMBER |  | 39 | 0 |  |  | `purpose`: `alternate_quantity` |
| `alternate_unit_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `alternate_unit_id` |
| `amortization_end_date` | TIMESTAMP |  |  |  |  |  | `purpose`: `amortization_end_date` |
| `amortization_schedule_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `amortization_schedule_id` |
| `amortization_start_date` | TIMESTAMP |  |  |  |  |  | `purpose`: `amortization_start_date` |
| `amortization_template_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `amortization_template_id` |
| `calculated_amount` | NUMBER |  | 20 | 2 |  |  | `purpose`: `calculated_amount` |
| `calculated_amount_foreign` | NUMBER |  | 20 | 2 |  |  | `purpose`: `calculated_foreign_currency_amount` |
| `class_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `class_id` |
| `contract_cost_allocation_pct` | NUMBER |  | 39 | 0 |  |  | `purpose`: `contract_cost_allocation_percent` |
| `contract_exp_offset_acct_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `contract_expense_offset_account` |
| `contract_expense_account_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `contract_expense_account_id` |
| `cost_amortization_amount` | NUMBER |  | 20 | 2 |  |  | `purpose`: `cost_amortization_amount` |
| `create_plan_on_event_type` | VARCHAR2 | 128 |  |  |  |  | `purpose`: `create_revenue_plan_on_event_type` |
| `currency_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `currency_id` |
| `date_created` | TIMESTAMP |  |  |  |  |  | `purpose`: `date_created` |
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `date_last_modified` |
| `deferral_account_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `deferral_account_id` |
| `department_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `department_id` |
| `disc_sales_amount_foreign` | NUMBER |  | 20 | 2 |  |  | `purpose`: `discounted_sales_amount`; `value_unit`: `foreign_currency` |
| `discounted_sales_amount` | NUMBER |  | 20 | 2 |  |  | `purpose`: `discounted_sales_amount` |
| `end_date` | TIMESTAMP |  |  |  |  |  | `purpose`: `end_date` |
| `entity_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `entity_id` |
| `exchange_rate` | NUMBER |  | 30 | 15 |  |  | `purpose`: `exchange_rate` |
| `expense_migrate_adjust_acct_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `expense_migration_adjustment_account` |
| `fair_value` | NUMBER |  | 30 | 15 |  |  | `purpose`: `fair_value` |
| `fair_value_foreign` | NUMBER |  | 30 | 15 |  |  | `purpose`: `fair_value`; `value_unit`: `foreign_currency` |
| `forecast_end_date` | TIMESTAMP |  |  |  |  |  | `purpose`: `forecast_end_date` |
| `forecast_start_date` | TIMESTAMP |  |  |  |  |  | `purpose`: `forecast_start_date` |
| `fx_adjustment_account_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `fx_adjustment_account` |
| `is_bom_item_type` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `is_bom_item_type` |
| `is_fair_value_override` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `fair_value_override` |
| `is_fair_value_vsoe` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `fair_value_is_vsoe` |
| `is_hold_rev_rec` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `hold_revenue_recognition_plan` |
| `is_permit_discount` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `element_permits_discount` |
| `is_posting_discount_applied` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `posting_discount_applied` |
| `item_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `item_id` |
| `item_labor_cost_amount` | NUMBER |  | 20 | 2 |  |  | `purpose`: `item_labor_cost_amount` |
| `item_resale_cost_amount` | NUMBER |  | 20 | 2 |  |  | `purpose`: `item_resale_cost_amount` |
| `labor_deferred_expense_acct_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `labor_deferred_expense_account` |
| `labor_expense_acct_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `labor_expense_account_id` |
| `last_merge_from_arrangement_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `last_merged_from_arrangement` |
| `location_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `location_id` |
| `new_standard_migrate_date` | TIMESTAMP |  |  |  |  |  | `purpose`: `new_standard_migrate_date` |
| `parent_bom_element_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `parent_bom_element` |
| `pending_action` | VARCHAR2 | 250 |  |  |  |  | `purpose`: `pending_action` |
| `quantity` | NUMBER |  | 18 | 8 |  |  | `purpose`: `quantity` |
| `recognition_account_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `recognition_account_id` |
| `reference_id` | VARCHAR2 | 4000 |  |  |  |  | `purpose`: `reference_id` |
| `return_revenue_element_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `return_revenue_element_id` |
| `rev_rec_forecast_rule_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `revenue_recognition_forecast_rule` |
| `rev_rec_rule_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `revenue_recognition_rule_id` |
| `revenue_allocation_group` | VARCHAR2 | 60 |  |  |  |  | `purpose`: `revenue_allocation_group` |
| `revenue_allocation_ratio` | NUMBER |  | 39 | 0 |  |  | `purpose`: `revenue_allocation_ratio` |
| `revenue_element_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `revenue_elements_id` |
| `revenue_element_number` | VARCHAR2 | 99 |  |  |  |  | `purpose`: `revenue_element_number` |
| `revenue_migrate_adjust_acct_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `revenue_migration_adjustment_account` |
| `sales_amount` | NUMBER |  | 20 | 2 |  |  | `purpose`: `sales_amount` |
| `sales_amount_foreign` | NUMBER |  | 20 | 2 |  |  | `purpose`: `foreign_currency_sales_amount` |
| `source_date` | TIMESTAMP |  |  |  |  |  | `purpose`: `source_date` |
| `source_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `source_id` |
| `source_transaction_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `source_transaction`; `not_supported`: `column_no_longer_supported` |
| `source_type` | VARCHAR2 | 250 |  |  |  |  | `purpose`: `source_type` |
| `start_date` | TIMESTAMP |  |  |  |  |  | `purpose`: `start_date` |
| `subsidiary_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `subsidiary_id` |
| `term_in_days` | NUMBER |  | 39 | 0 |  |  | `purpose`: `term_in_days` |
| `term_in_months` | NUMBER |  | 39 | 0 |  |  | `purpose`: `term_in_months` |
| `unbilled_receivable_group` | VARCHAR2 | 60 |  |  |  |  | `purpose`: `unbilled_receivable_group` |
| `carve_in_amount` | NUMBER |  | 39 | 0 |  |  |  |
| `carve_in_ratio` | NUMBER |  | 30 | 15 |  |  |  |
| `carve_out_amount` | NUMBER |  | 39 | 0 |  |  |  |
| `carve_out_ratio` | NUMBER |  | 30 | 15 |  |  |  |
| `is_carve_override` | VARCHAR2 | 3 |  |  |  |  |  |
| `subscription_line` | NUMBER |  | 39 | 0 |  |  |  |

## Referenced by

| Key | Table | Columns | Columns here |
|---|---|---|---|
| `Expense_plans_revenue_elements_fk` | `expense_plans` | `related_revenue_element_id` | `revenue_element_id` |
| `Revenue_plans_revenue_elements_fk` | `revenue_plans` | `revenue_element_id` | `revenue_element_id` |
| `Subscription_line_revisions_revenue_elements_fk` | `subscription_line_revisions` | `revenue_element_id` | `revenue_element_id` |
