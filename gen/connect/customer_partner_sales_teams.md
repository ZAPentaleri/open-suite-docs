# Customer_partner_sales_teams (`customer_partner_sales_teams`)
<!-- id: connect:customer_partner_sales_teams -->

| Fact | Value |
|---|---|
| Table | `customer_partner_sales_teams` |
| Primary key | `customer_id`, `partner_id` |
| Tags | `composite_primary_key` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `contribution` | NUMBER |  | 39 | 0 |  |  | `purpose`: `member_sales_contribution`; `value_unit`: `percent` |
| `customer_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `customer_id` |
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `date_last_modified` |
| `isprimary` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `primary_team_member` |
| `partner_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `partner_id` |
| `sales_role_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `sales_role_id` |
