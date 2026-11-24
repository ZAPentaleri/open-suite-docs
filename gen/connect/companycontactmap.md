# Companycontactmap (`companycontactmap`)
<!-- id: connect:companycontactmap -->

| Fact | Value |
|---|---|
| Table | `companycontactmap` |
| Primary key | `company_id`, `contact_id` |
| Tags | `composite_primary_key` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `company_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `company_id` |
| `contact_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `contact_id` |
| `contactrole_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `contact_role` |
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `date_last_modified` |
