# Transaction_sales_teams (`transaction_sales_teams`)
<!-- id: connect:transaction_sales_teams -->

| Fact | Value |
|---|---|
| Table | `transaction_sales_teams` |
| Primary key | `transaction_id`, `sales_rep_id` |
| Tags | `composite_primary_key` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `contribution` | NUMBER |  | 39 | 0 |  |  | `purpose`: `member_sales_contribution`; `value_unit`: `percent` |
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `date_last_modified`; `time_zone`: `gmt` |
| `isprimary` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `primary_team_member` |
| `sales_rep_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `sales_rep_id` |
| `sales_role_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `sales_role_id` |
| `transaction_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `transaction_id` |
