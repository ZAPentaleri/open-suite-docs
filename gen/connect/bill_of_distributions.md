# Bill_of_distributions (`bill_of_distributions`)
<!-- id: connect:bill_of_distributions -->

| Fact | Value |
|---|---|
| Table | `bill_of_distributions` |
| Primary key | `bill_of_distribution_id` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `bill_of_distribution_extid` | VARCHAR2 | 255 |  |  |  |  | `purpose`: `bill_of_distribution_external_id` |
| `bill_of_distribution_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `bill_of_distribution_id` |
| `date_created` | TIMESTAMP |  |  |  |  |  | `purpose`: `date_created` |
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `date_last_modified` |
| `distribution_category_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `distribution_category_id` |
| `distribution_network_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `distribution_network_id` |
| `is_inactive` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `whether_is_inactive` |
| `location_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `location_id` |
| `subsidiary_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `subsidiary_id` |
