# Expense_reports (`expense_reports`)
<!-- id: connect:expense_reports -->

| Fact | Value |
|---|---|
| Table | `expense_reports` |
| Record | `expensereport` |
| Primary key | `expense_report_id` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `date_created` | TIMESTAMP |  |  |  |  |  | `purpose`: `date_created` |
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `date_last_modified` |
| `expense_report_id` | NUMBER |  | 39 | 0 | `transactions`.`transaction_id` | primary_key | `purpose`: `expense_report_id` |
| `is_policy_violated` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `expense_policy_violated` |

## Foreign keys

| Key | Columns | References table | Referenced columns |
|---|---|---|---|
| `expense_reports_transactions_fk` | `expense_report_id` | `transactions` | `transaction_id` |
