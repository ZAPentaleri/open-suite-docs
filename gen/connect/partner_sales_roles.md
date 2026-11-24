# Partner_sales_roles (`partner_sales_roles`)
<!-- id: connect:partner_sales_roles -->

| Fact | Value |
|---|---|
| Table | `partner_sales_roles` |
| Primary key | `partner_sales_role_id` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `date_last_modified` |
| `isinactive` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `sales_role_inactive` |
| `name` | VARCHAR2 | 31 |  |  |  |  | `purpose`: `sales_role_name` |
| `parent_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `partner_id` |
| `partner_sales_role_extid` | VARCHAR2 | 255 |  |  |  |  | `purpose`: `partner_sales_role_external_id` |
| `partner_sales_role_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `partner_sales_role_id` |
