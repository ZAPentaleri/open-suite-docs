# Distribution_categories (`distribution_categories`)
<!-- id: connect:distribution_categories -->

| Fact | Value |
|---|---|
| Table | `distribution_categories` |
| Primary key | `distribution_category_id` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `category_name` | VARCHAR2 | 60 |  |  |  |  | `purpose`: `category_name` |
| `date_created` | TIMESTAMP |  |  |  |  |  | `purpose`: `date_created` |
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `date_last_modified` |
| `distribution_category_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `distribution_category_id` |
| `is_inactive` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `whether_is_inactive` |
