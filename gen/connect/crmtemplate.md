# Crmtemplate (`crmtemplate`)
<!-- id: connect:crmtemplate -->

| Fact | Value |
|---|---|
| Table | `crmtemplate` |
| Primary key | `crmtemplate_id` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `crmtemplate_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `template_id` |
| `crmtemplate_modified_by` | NUMBER |  | 39 | 0 |  |  | `purpose`: `crm_template_modified_by` |
| `crmtemplate_record_type` | VARCHAR2 | 11 |  |  |  |  | `purpose`: `crm_template_record_type` |
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `date_last_modified` |
| `description` | VARCHAR2 | 4000 |  |  |  |  | `purpose`: `description` |
| `from_email` | VARCHAR2 | 99 |  |  |  |  | `purpose`: `from_email` |
| `is_inactive` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `is_inactive` |
| `is_private` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `private` |
| `name` | VARCHAR2 | 4000 |  |  |  |  | `purpose`: `name` |
| `owner_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `owner_id` |
| `reply_email_address` | VARCHAR2 | 99 |  |  |  |  | `purpose`: `reply_email_address` |
| `subject` | VARCHAR2 | 199 |  |  |  |  | `purpose`: `subject` |

## Referenced by

| Key | Table | Columns | Columns here |
|---|---|---|---|
| `Campaignevent_crmtemplate_fk` | `campaignevent` | `template_id` | `crmtemplate_id` |
