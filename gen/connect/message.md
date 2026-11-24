# Message (`message`)
<!-- id: connect:message -->

| Fact | Value |
|---|---|
| Table | `message` |
| Record | `message` |
| Primary key | `message_id` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `author_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `author_id` |
| `bcc` | VARCHAR2 | 999 |  |  |  |  | `purpose`: `bcc` |
| `cc` | VARCHAR2 | 999 |  |  |  |  | `purpose`: `cc` |
| `company_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `company_id` |
| `date_0` | TIMESTAMP |  |  |  |  |  | `purpose`: `date` |
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `date_last_modified`; `time_zone`: `gmt` |
| `event_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `event_id` |
| `from_0` | VARCHAR2 | 254 |  |  |  |  | `purpose`: `from` |
| `internal_only` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `internal_only` |
| `is_emailed` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `message_was_emailed` |
| `is_incoming` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `message_was_received` |
| `message` | VARCHAR2 | 65534 |  |  |  |  | `purpose`: `message` |
| `message_extid` | VARCHAR2 | 255 |  |  |  |  | `purpose`: `message_external_id` |
| `message_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `message_id` |
| `message_type_id` | VARCHAR2 | 19 |  |  |  |  | `purpose`: `message_type_id` |
| `recipient_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `recipient_id` |
| `record_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `record_id` |
| `record_type_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `record_type_id` |
| `subject` | VARCHAR2 | 199 |  |  |  |  | `purpose`: `subject` |
| `template_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `template_id` |
| `to_0` | VARCHAR2 | 300 |  |  |  |  | `purpose`: `to` |
| `transaction_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `transaction_id` |

## Referenced by

| Key | Table | Columns | Columns here |
|---|---|---|---|
| `Messagerecipient_message_fk` | `messagerecipient` | `message_id` | `message_id` |
