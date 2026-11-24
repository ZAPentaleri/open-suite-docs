# Budget_category (`budget_category`)
<!-- id: connect:budget_category -->

| Fact | Value |
|---|---|
| Table | `budget_category` |
| Record | `budgetcategory` |
| Primary key | `budget_category_id` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `budget_category_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `budget_category_id` |
| `is_global` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `whether_is_global` |
| `isinactive` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `is_inactive` |
| `name` | VARCHAR2 | 50 |  |  |  |  | `purpose`: `name` |

## Referenced by

| Key | Table | Columns | Columns here |
|---|---|---|---|
| `Budget_budget_category_fk` | `budget` | `category_id` | `budget_category_id` |
