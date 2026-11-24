# Trans_partner_sales_teams (`trans_partner_sales_teams`)
<!-- id: connect:trans_partner_sales_teams -->

| Fact | Value |
|---|---|
| Table | `trans_partner_sales_teams` |
| Primary key | `transaction_id`, `partner_id` |
| Tags | `composite_primary_key` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `contribution` | NUMBER |  | 39 | 0 |  |  | `purpose`: `member_sales_contribution`; `value_unit`: `percent` |
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `date_last_modified`; `time_zone`: `gmt` |
| `isprimary` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `primary_team_member` |
| `partner_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `partner_id` |
| `partner_sales_role_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `partner_sales_role_id` |
| `transaction_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `transaction_id` |
