# Caseissue (`caseissue`)
<!-- id: connect:caseissue -->

| Fact | Value |
|---|---|
| Table | `caseissue` |
| Primary key | `case_issue_id` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `case_issue_extid` | VARCHAR2 | 255 |  |  |  |  | `purpose`: `case_issue_external_id` |
| `case_issue_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `case_issue_id` |
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `date_last_modified` |
| `description` | VARCHAR2 | 200 |  |  |  |  | `purpose`: `description` |
| `is_inactive` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `is_inactive` |
| `name` | VARCHAR2 | 50 |  |  |  |  | `purpose`: `name` |

## Referenced by

| Key | Table | Columns | Columns here |
|---|---|---|---|
| `Support_incidents_caseissue_fk` | `support_incidents` | `case_issue_id` | `case_issue_id` |
