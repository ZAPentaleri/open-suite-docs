# Expense_categories_subs_map (`expense_categories_subs_map`)
<!-- id: connect:expense_categories_subs_map -->

| Fact | Value |
|---|---|
| Table | `expense_categories_subs_map` |
| Primary key | `subsidiary_id`, `expense_category_id` |
| Tags | `composite_primary_key` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `expense_category_id` | NUMBER |  | 39 | 0 | `expense_categories`.`expense_category_id` | primary_key | `purpose`: `expense_category_id` |
| `subsidiary_id` | NUMBER |  | 39 | 0 | `subsidiaries`.`subsidiary_id` | primary_key | `purpose`: `subsidiary_id` |

## Foreign keys

| Key | Columns | References table | Referenced columns |
|---|---|---|---|
| `expense_categories_subs_map_expense_categories_fk` | `expense_category_id` | `expense_categories` | `expense_category_id` |
| `expense_categories_subs_map_subsidiaries_fk` | `subsidiary_id` | `subsidiaries` | `subsidiary_id` |
