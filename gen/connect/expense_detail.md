# Expense_detail (`expense_detail`)
<!-- id: connect:expense_detail -->

| Fact | Value |
|---|---|
| Table | `expense_detail` |
| Primary key | `expense_detail_id` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `amount` | NUMBER |  | 20 | 2 |  |  | `purpose`: `amount` |
| `currency_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `currency` |
| `exchange_rate` | NUMBER |  | 30 | 15 |  |  | `purpose`: `exchange_rate` |
| `expense_category_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `expense_category_id` |
| `expense_date` | TIMESTAMP |  |  |  |  |  | `purpose`: `date_of_the_expense` |
| `expense_detail_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `expense_detail_id` |
| `foreign_amount` | NUMBER |  | 20 | 2 |  |  | `purpose`: `foreign_amount` |
| `foreign_rate` | NUMBER |  | 30 | 15 |  |  | `purpose`: `foreign_rate` |
| `has_receipt` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `line_has_receipt` |
| `is_paid_by_corp_credit_card` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `paid_by_corporate_card` |
| `memo` | VARCHAR2 | 999 |  |  |  |  | `purpose`: `memo` |
| `quantity` | NUMBER |  | 18 | 8 |  |  | `purpose`: `quantity` |
| `rate` | NUMBER |  | 30 | 15 |  |  | `purpose`: `rate` |
| `reference_number` | NUMBER |  | 5 | 0 |  |  | `purpose`: `reference_number` |
| `tax_amount` | NUMBER |  | 20 | 2 |  |  | `purpose`: `tax_amount` |
| `transaction_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `transaction_id` |
| `transaction_line_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `transaction_line_id` |
