# Entitycategory (`entitycategory`)
<!-- id: connect:entitycategory -->

| Fact | Value |
|---|---|
| Table | `entitycategory` |
| Primary key | `entity_category_id`, `entity_type` |
| Tags | `composite_primary_key` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `entity_category_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `entity_category_id` |
| `entity_type` | VARCHAR2 | 12 |  |  |  | primary_key | `purpose`: `entity_type` |
| `is_inactive` | VARCHAR2 | 1 |  |  |  |  | `purpose`: `is_inactive` |
| `name` | VARCHAR2 | 99 |  |  |  |  | `purpose`: `name` |
| `parent_0` | NUMBER |  | 39 | 0 |  |  | `purpose`: `parent_entity_category_id` |
