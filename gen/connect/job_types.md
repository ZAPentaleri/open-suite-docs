# Job_types (`job_types`)
<!-- id: connect:job_types -->

| Fact | Value |
|---|---|
| Table | `job_types` |
| Record | `jobtype` |
| Primary key | `job_type_id` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `isinactive` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `item_is_inactive` |
| `job_type_extid` | VARCHAR2 | 255 |  |  |  |  | `purpose`: `job_type_external_id` |
| `job_type_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `job_type_id` |
| `name` | VARCHAR2 | 31 |  |  |  |  | `purpose`: `name` |
| `parent_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `subtype_of` |

## Referenced by

| Key | Table | Columns | Columns here |
|---|---|---|---|
| `Customers_job_types_fk` | `customers` | `job_type_id` | `job_type_id` |
