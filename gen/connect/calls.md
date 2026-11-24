# Calls (`calls`)
<!-- id: connect:calls -->

| Fact | Value |
|---|---|
| Table | `calls` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `call_date` | TIMESTAMP |  |  |  |  |  | `purpose`: `call_date` |
| `call_extid` | VARCHAR2 | 255 |  |  |  |  | `purpose`: `call_external_id` |
| `call_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `call_id` |
| `case_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `case_id` |
| `company_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `company_id` |
| `contact_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `contact_id` |
| `date_completed` | TIMESTAMP |  |  |  |  |  | `purpose`: `date_completed` |
| `date_created` | TIMESTAMP |  |  |  |  |  | `purpose`: `date_created` |
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `date_last_modified` |
| `note` | VARCHAR2 | 4000 |  |  |  |  | `purpose`: `note` |
| `opportunity_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `opportunity` |
| `organizer_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `organizer_id` |
| `owner_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `owner_id_created_by` |
| `phone` | VARCHAR2 | 100 |  |  |  |  | `purpose`: `phone_number` |
| `status` | VARCHAR2 | 64 |  |  |  |  | `purpose`: `status` |
| `subject` | VARCHAR2 | 256 |  |  |  |  | `purpose`: `subject` |
