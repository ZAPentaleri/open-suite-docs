# Expense_categories_rates (`expense_categories_rates`)
<!-- id: connect:expense_categories_rates -->

| Fact | Value |
|---|---|
| Table | `expense_categories_rates` |
| Primary key | `expense_category_id`, `subsidiary_id` |
| Tags | `composite_primary_key` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `default_rate` | NUMBER |  | 30 | 15 |  |  | `purpose`: `default_rate` |
| `expense_category_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `expense_category_id` |
| `subsidiary_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `subsidiary_id` |
