# Customer_sales_teams (`customer_sales_teams`)
<!-- id: connect:customer_sales_teams -->

| Fact | Value |
|---|---|
| Table | `customer_sales_teams` |
| Primary key | `customer_id`, `sales_rep_id` |
| Tags | `composite_primary_key` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `contribution` | NUMBER |  | 39 | 0 |  |  | `purpose`: `member_sales_contribution`; `value_unit`: `percent` |
| `customer_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `customer_id` |
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `date_last_modified` |
| `isprimary` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `primary_team_member` |
| `sales_rep_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `sales_rep_id` |
| `sales_role_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `sales_role_id` |
