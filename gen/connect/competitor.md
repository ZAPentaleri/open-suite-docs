# Competitor (`competitor`)
<!-- id: connect:competitor -->

| Fact | Value |
|---|---|
| Table | `competitor` |
| Record | `competitor` |
| Primary key | `competitor_id` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `competitor_extid` | VARCHAR2 | 255 |  |  |  |  | `purpose`: `competitor_external_id` |
| `competitor_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `competitor_id` |
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `date_last_modified` |
| `description` | VARCHAR2 | 1999 |  |  |  |  | `purpose`: `description` |
| `is_inactive` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `is_inactive` |
| `name` | VARCHAR2 | 50 |  |  |  |  | `purpose`: `name` |
| `products_services` | VARCHAR2 | 65534 |  |  |  |  | `purpose`: `products_services` |
| `strategy` | VARCHAR2 | 65534 |  |  |  |  | `purpose`: `strategy` |
| `strengths` | VARCHAR2 | 65534 |  |  |  |  | `purpose`: `strengths` |
| `url` | VARCHAR2 | 100 |  |  |  |  | `purpose`: `url` |
| `weaknesses` | VARCHAR2 | 65534 |  |  |  |  | `purpose`: `weaknesses` |
