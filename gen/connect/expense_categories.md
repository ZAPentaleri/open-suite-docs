# Expense_categories (`expense_categories`)
<!-- id: connect:expense_categories -->

| Fact | Value |
|---|---|
| Table | `expense_categories` |
| Primary key | `expense_category_id` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `description` | VARCHAR2 | 999 |  |  |  |  | `purpose`: `description` |
| `expense_account_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `expense_account_id` |
| `expense_category_extid` | VARCHAR2 | 255 |  |  |  |  | `purpose`: `expense_category_external_id` |
| `expense_category_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `expense_category_id` |
| `expense_item_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `expense_item_id` |
| `isinactive` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `category_is_inactive` |
| `name` | VARCHAR2 | 61 |  |  |  |  | `purpose`: `name` |
| `rate_required` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `rate_required` |
| `is_personal_corp_card_expense` | VARCHAR2 | 3 |  |  |  |  |  |

## Referenced by

| Key | Table | Columns | Columns here |
|---|---|---|---|
| `Expense_categories_subs_map_expense_categories_fk` | `expense_categories_subs_map` | `expense_category_id` | `expense_category_id` |
| `Imported_employee_expenses_expense_categories_fk` | `imported_employee_expenses` | `expense_category_id` | `expense_category_id` |
| `Opportunity_lines_expense_categories_fk` | `opportunity_lines` | `expense_category_id` | `expense_category_id` |
