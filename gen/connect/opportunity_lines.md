# Opportunity_lines (`opportunity_lines`)
<!-- id: connect:opportunity_lines -->

| Fact | Value |
|---|---|
| Table | `opportunity_lines` |
| Primary key | `opportunity_id`, `opportunity_line_id` |
| Tags | `composite_primary_key` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `account_id` | NUMBER |  | 39 | 0 | `accounts`.`account_id` | foreign_key | `purpose`: `account_id` |
| `amount` | NUMBER |  | 20 | 2 |  |  | `purpose`: `amount` |
| `amount_foreign_linked` | NUMBER |  | 30 | 15 |  |  | `purpose`: `foreign_currency_amount_linked` |
| `amount_linked` | NUMBER |  | 20 | 2 |  |  | `purpose`: `amount_linked` |
| `amount_pending` | NUMBER |  | 20 | 2 |  |  | `purpose`: `amount_pending` |
| `amount_taxable` | NUMBER |  | 20 | 2 |  |  | `purpose`: `amount_taxable` |
| `amount_taxed` | NUMBER |  | 20 | 2 |  |  | `purpose`: `amount_taxed` |
| `class_id` | NUMBER |  | 39 | 0 | `classes`.`class_id` | foreign_key | `purpose`: `class_id` |
| `company_id` | NUMBER |  | 39 | 0 | `entity`.`entity_id` | foreign_key | `purpose`: `company_id` |
| `date_cleared` | TIMESTAMP |  |  |  |  |  | `purpose`: `date_cleared` |
| `date_closed` | TIMESTAMP |  |  |  |  |  | `purpose`: `date_closed` |
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `date_last_modified` |
| `department_id` | NUMBER |  | 39 | 0 | `departments`.`department_id` | foreign_key | `purpose`: `department_id` |
| `do_not_display_line` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `do_not_display_line` |
| `do_not_print_line` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `do_not_print_line` |
| `expense_category_id` | NUMBER |  | 39 | 0 | `expense_categories`.`expense_category_id` | foreign_key | `purpose`: `expense_category_id` |
| `gross_amount` | NUMBER |  | 39 | 0 |  |  | `purpose`: `gross_amount` |
| `has_cost_line` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `has_cost_line` |
| `is_cost_line` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `is_cost_line` |
| `is_custom_line` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `is_custom_line` |
| `is_exclude_from_rate_request` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `excluded_from_rate_request` |
| `is_item_value_adjustment` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `is_item_value_adjustment` |
| `isbillable` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `billable` |
| `iscleared` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `cleared` |
| `isnonreimbursable` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `is_non_reimbursable` |
| `istaxable` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `taxable` |
| `item_count` | NUMBER |  | 18 | 8 |  |  | `purpose`: `item_count` |
| `item_id` | NUMBER |  | 39 | 0 | `items`.`item_id` | foreign_key | `purpose`: `item_id` |
| `item_received` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `item_received` |
| `item_unit_price` | VARCHAR2 | 42 |  |  |  |  | `purpose`: `item_unit_price` |
| `kit_part_number` | NUMBER |  | 39 | 0 |  |  | `purpose`: `kit_name_number` |
| `location_id` | NUMBER |  | 39 | 0 | `locations`.`location_id` | foreign_key | `purpose`: `location_id` |
| `memo` | VARCHAR2 | 4000 |  |  |  |  | `purpose`: `memo` |
| `net_amount` | NUMBER |  | 39 | 0 |  |  | `purpose`: `net_amount` |
| `non_posting_line` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `non_posting_line` |
| `number_billed` | NUMBER |  | 18 | 8 |  |  | `purpose`: `number_billed` |
| `number_revenue_committed` | NUMBER |  | 18 | 8 |  |  | `purpose`: `number_revenue_committed` |
| `opportunity_discount_line` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `opportunity_discount_line` |
| `opportunity_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `opportunity_id` |
| `opportunity_line_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `opportunity_line_id` |
| `opportunity_order` | NUMBER |  | 39 | 0 |  |  | `purpose`: `opportunity_order` |
| `payment_method_id` | NUMBER |  | 39 | 0 | `payment_methods`.`payment_method_id` | foreign_key | `purpose`: `payment_method_id` |
| `payroll_item_id` | NUMBER |  | 39 | 0 | `payroll_items`.`payroll_item_id` | foreign_key | `purpose`: `payroll_item_id` |
| `payroll_wage_base_amount` | NUMBER |  | 20 | 2 |  |  | `purpose`: `payroll_wage_base_amount` |
| `payroll_year_to_date_amount` | NUMBER |  | 20 | 2 |  |  | `purpose`: `payroll_ytd_amount` |
| `period_closed` | TIMESTAMP |  |  |  |  |  | `purpose`: `period_closed` |
| `price_type_id` | NUMBER |  | 39 | 0 | `price_types`.`price_type_id` | foreign_key | `purpose`: `price_type_id` |
| `quantity_received_in_shipment` | NUMBER |  | 18 | 8 |  |  | `purpose`: `quantity_received_in_shipment` |
| `receivebydate` | TIMESTAMP |  |  |  |  |  | `purpose`: `receive_by_date` |
| `reimbursement_type` | VARCHAR2 | 128 |  |  |  |  | `purpose`: `reimbursement_type` |
| `related_company_id` | NUMBER |  | 39 | 0 | `entity`.`entity_id` | foreign_key | `purpose`: `related_company_id` |
| `shipdate` | TIMESTAMP |  |  |  |  |  | `purpose`: `ship_date` |
| `shipment_received` | TIMESTAMP |  |  |  |  |  | `purpose`: `shipment_received` |
| `subsidiary_id` | NUMBER |  | 39 | 0 | `subsidiaries`.`subsidiary_id` | foreign_key | `purpose`: `subsidiary_id` |
| `tax_item_id` | NUMBER |  | 39 | 0 | `items`.`item_id` | foreign_key | `purpose`: `tax_item_id` |
| `tax_type` | VARCHAR2 | 64 |  |  |  |  | `purpose`: `tax_type` |
| `tobeemailed` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `to_be_emailed` |
| `tobefaxed` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `to_be_faxed` |
| `tobeprinted` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `to_be_printed` |

## Foreign keys

| Key | Columns | References table | Referenced columns |
|---|---|---|---|
| `opportunity_lines_accounts_fk` | `account_id` | `accounts` | `account_id` |
| `opportunity_lines_classes_fk` | `class_id` | `classes` | `class_id` |
| `opportunity_lines_departments_fk` | `department_id` | `departments` | `department_id` |
| `opportunity_lines_entity_fk` | `company_id` | `entity` | `entity_id` |
| `opportunity_lines_entity_fk_2` | `related_company_id` | `entity` | `entity_id` |
| `opportunity_lines_expense_categories_fk` | `expense_category_id` | `expense_categories` | `expense_category_id` |
| `opportunity_lines_items_fk` | `item_id` | `items` | `item_id` |
| `opportunity_lines_items_fk_2` | `tax_item_id` | `items` | `item_id` |
| `opportunity_lines_locations_fk` | `location_id` | `locations` | `location_id` |
| `opportunity_lines_payment_methods_fk` | `payment_method_id` | `payment_methods` | `payment_method_id` |
| `opportunity_lines_payroll_items_fk` | `payroll_item_id` | `payroll_items` | `payroll_item_id` |
| `opportunity_lines_price_types_fk` | `price_type_id` | `price_types` | `price_type_id` |
| `opportunity_lines_subsidiaries_fk` | `subsidiary_id` | `subsidiaries` | `subsidiary_id` |
