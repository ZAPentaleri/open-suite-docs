# Distribution_networks (`distribution_networks`)
<!-- id: connect:distribution_networks -->

| Fact | Value |
|---|---|
| Table | `distribution_networks` |
| Primary key | `distribution_network_id` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `date_created` | TIMESTAMP |  |  |  |  |  | `purpose`: `date_created` |
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `date_last_modified` |
| `distribution_network_extid` | VARCHAR2 | 255 |  |  |  |  | `purpose`: `distribution_network_external_id` |
| `distribution_network_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `distribution_network_id` |
| `is_inactive` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `whether_is_inactive` |
| `memo` | VARCHAR2 | 4000 |  |  |  |  | `purpose`: `memo` |
| `network_name` | VARCHAR2 | 100 |  |  |  |  | `purpose`: `network_name` |
