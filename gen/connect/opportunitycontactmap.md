# Opportunitycontactmap (`opportunitycontactmap`)
<!-- id: connect:opportunitycontactmap -->

| Fact | Value |
|---|---|
| Table | `opportunitycontactmap` |
| Primary key | `opportunity_id`, `contact_id` |
| Tags | `composite_primary_key` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `contact_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `contact_id` |
| `contactrole_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `contact_role_id` |
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `date_last_modified` |
| `opportunity_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `opportunity_id` |
