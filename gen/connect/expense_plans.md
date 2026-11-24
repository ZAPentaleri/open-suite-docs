# Expense_plans (`expense_plans`)
<!-- id: connect:expense_plans -->

| Fact | Value |
|---|---|
| Table | `expense_plans` |
| Record | `expenseplan` |
| Primary key | `plan_id` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `accounting_period_id` | NUMBER |  | 39 | 0 | `accounting_periods`.`accounting_period_id` | foreign_key | `purpose`: `accounting_period_id` |
| `comments` | VARCHAR2 | 500 |  |  |  |  | `purpose`: `comments` |
| `date_created` | TIMESTAMP |  |  |  |  |  | `purpose`: `date_created` |
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `date_last_modified` |
| `end_date` | TIMESTAMP |  |  |  |  |  | `purpose`: `end_date` |
| `expense_plan_status` | VARCHAR2 | 128 |  |  |  |  | `purpose`: `expense_plan_status` |
| `expense_plan_type` | VARCHAR2 | 128 |  |  |  |  | `purpose`: `expense_plan_type` |
| `expense_rule_id` | NUMBER |  | 39 | 0 | `expense_amortization_rules`.`expense_rule_id` | foreign_key | `purpose`: `expense_rule_id` |
| `is_hold_expense` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `is_hold_expense` |
| `plan_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `plan_id` |
| `plan_number` | VARCHAR2 | 99 |  |  |  |  | `purpose`: `plan_number` |
| `reforecast_method` | VARCHAR2 | 150 |  |  |  |  | `purpose`: `reforecast_method` |
| `related_revenue_arrangement_id` | NUMBER |  | 39 | 0 | `transactions`.`transaction_id` | foreign_key | `purpose`: `related_revenue_arrangement_id` |
| `related_revenue_element_id` | NUMBER |  | 39 | 0 | `revenue_elements`.`revenue_element_id` | foreign_key | `purpose`: `related_revenue_element_id` |
| `residual` | NUMBER |  | 20 | 2 |  |  | `purpose`: `residual` |
| `start_date` | TIMESTAMP |  |  |  |  |  | `purpose`: `start_date` |
| `transaction_book_id` | NUMBER |  | 39 | 0 | `accounting_books`.`accounting_book_id` | foreign_key | `purpose`: `transaction_book_id` |
| `transaction_doc_id` | NUMBER |  | 39 | 0 | `transaction_lines`.`transaction_id` | foreign_key | `purpose`: `transaction_document_id` |
| `transaction_line_id` | NUMBER |  | 39 | 0 | `transaction_lines`.`transaction_line_id` | foreign_key | `purpose`: `transaction_line_id` |

## Foreign keys

| Key | Columns | References table | Referenced columns |
|---|---|---|---|
| `expense_plans_accounting_books_fk` | `transaction_book_id` | `accounting_books` | `accounting_book_id` |
| `expense_plans_accounting_periods_fk` | `accounting_period_id` | `accounting_periods` | `accounting_period_id` |
| `expense_plans_expense_amortization_rules_fk` | `expense_rule_id` | `expense_amortization_rules` | `expense_rule_id` |
| `expense_plans_revenue_elements_fk` | `related_revenue_element_id` | `revenue_elements` | `revenue_element_id` |
| `expense_plans_transaction_lines_fk` | `transaction_doc_id` | `transaction_lines` | `transaction_id` |
| `expense_plans_transaction_lines_fk_1` | `transaction_line_id` | `transaction_lines` | `transaction_line_id` |
| `expense_plans_transactions_fk` | `related_revenue_arrangement_id` | `transactions` | `transaction_id` |

## Referenced by

| Key | Table | Columns | Columns here |
|---|---|---|---|
| `Expense_plan_lines_expense_plans_fk` | `expense_plan_lines` | `plan_id` | `plan_id` |
