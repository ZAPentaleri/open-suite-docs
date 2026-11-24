# Territory (`territory`)
<!-- id: connect:territory -->

| Fact | Value |
|---|---|
| Table | `territory` |
| Primary key | `territory_id` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `date_last_modified`; `time_zone`: `gmt` |
| `description` | VARCHAR2 | 199 |  |  |  |  | `purpose`: `description` |
| `is_inactive` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `is_inactive` |
| `name` | VARCHAR2 | 50 |  |  |  |  | `purpose`: `name` |
| `territory_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `territory_id` |

## Referenced by

| Key | Table | Columns | Columns here |
|---|---|---|---|
| `Customers_territory_fk` | `customers` | `sales_territory_id` | `territory_id` |
| `Entityterritorymap_territory_fk` | `entityterritorymap` | `territory_id` | `territory_id` |
| `Support_incidents_territory_fk` | `support_incidents` | `territory_id` | `territory_id` |
