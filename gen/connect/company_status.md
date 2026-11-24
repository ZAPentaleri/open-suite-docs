# Company_status (`company_status`)
<!-- id: connect:company_status -->

| Fact | Value |
|---|---|
| Table | `company_status` |
| Primary key | `company_status_id` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `company_status_extid` | VARCHAR2 | 255 |  |  |  |  | `purpose`: `company_status_external_id` |
| `company_status_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `company_status` |
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `date_last_modified`; `time_zone`: `gmt` |
| `description` | VARCHAR2 | 199 |  |  |  |  | `purpose`: `description` |
| `isinactive` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `inactive` |
| `name` | VARCHAR2 | 199 |  |  |  |  | `purpose`: `name` |
| `probability` | NUMBER |  | 6 | 2 |  |  | `purpose`: `probability` |
| `readonly` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `read_only` |

## Referenced by

| Key | Table | Columns | Columns here |
|---|---|---|---|
| `Opportunities_company_status_fk` | `opportunities` | `company_status_id` | `company_status_id` |
