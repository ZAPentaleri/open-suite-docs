# Sales_roles (`sales_roles`)
<!-- id: connect:sales_roles -->

| Fact | Value |
|---|---|
| Table | `sales_roles` |
| Record | `salesrole` |
| Primary key | `sales_role_id` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `date_last_modified`; `time_zone`: `gmt` |
| `description` | VARCHAR2 | 999 |  |  |  |  | `purpose`: `sales_role_description` |
| `is_inactive` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `sales_role_inactive` |
| `is_sales_rep_role` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `is_sales_rep_role` |
| `name` | VARCHAR2 | 50 |  |  |  |  | `purpose`: `sales_role_name` |
| `sales_role_extid` | VARCHAR2 | 255 |  |  |  |  | `purpose`: `sales_role_external_id` |
| `sales_role_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `sales_role_id` |
