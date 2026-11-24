# Support_incidents (`support_incidents`)
<!-- id: connect:support_incidents -->

| Fact | Value |
|---|---|
| Table | `support_incidents` |
| Primary key | `case_id` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `assigned_id` | NUMBER |  | 39 | 0 | `entity`.`entity_id` | foreign_key | `purpose`: `assigned_id` |
| `case_email` | VARCHAR2 | 300 |  |  |  |  | `purpose`: `case_email` |
| `case_extid` | VARCHAR2 | 255 |  |  |  |  | `purpose`: `case_external_id` |
| `case_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `case_id` |
| `case_issue_id` | NUMBER |  | 39 | 0 | `caseissue`.`case_issue_id` | foreign_key | `purpose`: `case_issue_id` |
| `case_number` | NUMBER |  | 39 | 0 |  |  | `purpose`: `case_number` |
| `case_numbercode` | VARCHAR2 | 99 |  |  |  |  | `purpose`: `case_number_code` |
| `case_origin_id` | NUMBER |  | 39 | 0 | `case_origins`.`case_origin_id` | foreign_key | `purpose`: `case_origin_id` |
| `case_phone` | VARCHAR2 | 100 |  |  |  |  | `purpose`: `case_phone_number` |
| `case_profile_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `case_profile_id` |
| `case_stage` | VARCHAR2 | 12 |  |  |  |  | `purpose`: `case_stage` |
| `case_type_id` | NUMBER |  | 39 | 0 | `case_types`.`case_type_id` | foreign_key | `purpose`: `case_type_id` |
| `comments` | VARCHAR2 | 4000 |  |  |  |  | `purpose`: `comments` |
| `company_id` | NUMBER |  | 39 | 0 | `entity`.`entity_id` | foreign_key | `purpose`: `company_id` |
| `contact_id` | NUMBER |  | 39 | 0 | `contacts`.`contact_id` | foreign_key | `purpose`: `contact_id` |
| `create_date` | TIMESTAMP |  |  |  |  |  | `purpose`: `create_date`; `time_zone`: `gmt` |
| `date_closed` | TIMESTAMP |  |  |  |  |  | `purpose`: `date_closed` |
| `date_last_cust_msg_received` | TIMESTAMP |  |  |  |  |  | `purpose`: `last_customer_message_received_at` |
| `date_last_message` | TIMESTAMP |  |  |  |  |  | `purpose`: `last_case_message_at`; `applies_to`: `sent`, `received` |
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `date_last_modified` |
| `date_support_first_reply` | TIMESTAMP |  |  |  |  |  | `purpose`: `first_reply_to_customer_at` |
| `entry_time` | TIMESTAMP |  |  |  |  |  | `purpose`: `incident_time` |
| `helpdesk` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `help_desk` |
| `inboundemail` | VARCHAR2 | 300 |  |  |  |  | `purpose`: `inbound_email` |
| `isinactive` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `inactive` |
| `item_id` | NUMBER |  | 39 | 0 | `items`.`item_id` | foreign_key | `purpose`: `item_id` |
| `last_modification` | TIMESTAMP |  |  |  |  |  | `purpose`: `last_modification` |
| `last_reopened_date` | TIMESTAMP |  |  |  |  |  | `purpose`: `last_reopened_date` |
| `module_name` | VARCHAR2 | 4000 |  |  |  |  | `purpose`: `module_name` |
| `name` | VARCHAR2 | 256 |  |  |  |  | `purpose`: `name` |
| `owner_id` | NUMBER |  | 39 | 0 | `entity`.`entity_id` | foreign_key | `purpose`: `owner_id` |
| `priority` | VARCHAR2 | 64 |  |  |  |  | `purpose`: `priority` |
| `product` | VARCHAR2 | 64 |  |  |  |  | `purpose`: `product` |
| `stage` | VARCHAR2 | 30 |  |  |  |  | `purpose`: `stage` |
| `status` | VARCHAR2 | 64 |  |  |  |  | `purpose`: `status` |
| `subsidiary_id` | NUMBER |  | 39 | 0 | `subsidiaries`.`subsidiary_id` | foreign_key | `purpose`: `subsidiary_id` |
| `territory_id` | NUMBER |  | 39 | 0 | `territory`.`territory_id` | foreign_key | `purpose`: `territory_id` |

## Foreign keys

| Key | Columns | References table | Referenced columns |
|---|---|---|---|
| `support_incidents_case_origins_fk` | `case_origin_id` | `case_origins` | `case_origin_id` |
| `support_incidents_case_types_fk` | `case_type_id` | `case_types` | `case_type_id` |
| `support_incidents_caseissue_fk` | `case_issue_id` | `caseissue` | `case_issue_id` |
| `support_incidents_contacts_fk` | `contact_id` | `contacts` | `contact_id` |
| `support_incidents_entity_fk` | `owner_id` | `entity` | `entity_id` |
| `support_incidents_entity_fk_2` | `assigned_id` | `entity` | `entity_id` |
| `support_incidents_entity_fk_3` | `company_id` | `entity` | `entity_id` |
| `support_incidents_items_fk` | `item_id` | `items` | `item_id` |
| `support_incidents_subsidiaries_fk` | `subsidiary_id` | `subsidiaries` | `subsidiary_id` |
| `support_incidents_territory_fk` | `territory_id` | `territory` | `territory_id` |

## Referenced by

| Key | Table | Columns | Columns here |
|---|---|---|---|
| `Case_stage_changes_support_incidents_fk` | `case_stage_changes` | `case_id` | `case_id` |
| `Caseescalationhistory_support_incidents_fk` | `caseescalationhistory` | `case_id` | `case_id` |
