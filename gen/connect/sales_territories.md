# Sales_territories (`sales_territories`)
<!-- id: connect:sales_territories -->

| Fact | Value |
|---|---|
| Table | `sales_territories` |
| Primary key | `sales_territory_id` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `date_last_modified`; `time_zone`: `gmt` |
| `description` | VARCHAR2 | 199 |  |  |  |  | `purpose`: `description` |
| `isinactive` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `territory_is_inactive` |
| `name` | VARCHAR2 | 50 |  |  |  |  | `purpose`: `name` |
| `priority` | NUMBER |  | 39 | 0 |  |  | `purpose`: `priority` |
| `sales_territory_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `sales_territory_id` |
