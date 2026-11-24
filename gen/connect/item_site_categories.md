# Item_site_categories (`item_site_categories`)
<!-- id: connect:item_site_categories -->

| Fact | Value |
|---|---|
| Table | `item_site_categories` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `category_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `category_id` |
| `category_sequence` | NUMBER |  | 39 | 0 |  |  | `purpose`: `category_sequence` |
| `category_type` | VARCHAR2 | 20 |  |  |  |  | `purpose`: `category_type` |
| `default_name` | VARCHAR2 | 299 |  |  |  |  | `purpose`: `default_name` |
| `description` | VARCHAR2 | 4000 |  |  |  |  | `purpose`: `description` |
| `full_name` | VARCHAR2 | 199 |  |  |  |  | `purpose`: `full_name` |
| `ishidden` | VARCHAR2 | 1 |  |  |  |  | `purpose`: `is_hidden` |
| `isinactive` | VARCHAR2 | 1 |  |  |  |  | `purpose`: `is_inactive` |
| `ispreferred` | VARCHAR2 | 1 |  |  |  |  | `purpose`: `preferred_site_category` |
| `item_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `item_id` |
| `long_description` | VARCHAR2 | 65534 |  |  |  |  | `purpose`: `long_description` |
| `parent_category_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `parent_category_id` |
| `section_created_by` | NUMBER |  | 39 | 0 |  |  | `purpose`: `section_created_by` |
| `section_created_date` | TIMESTAMP |  |  |  |  |  | `purpose`: `section_created_date` |
| `section_modified_by` | NUMBER |  | 39 | 0 |  |  | `purpose`: `section_modified_by` |
| `section_modified_date` | TIMESTAMP |  |  |  |  |  | `purpose`: `section_modified_date` |
