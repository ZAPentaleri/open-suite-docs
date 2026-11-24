# Campaignresponse (`campaignresponse`)
<!-- id: connect:campaignresponse -->

| Fact | Value |
|---|---|
| Table | `campaignresponse` |
| Record | `campaignresponse` |
| Primary key | `campaign_response_id` |
| Domains | Campaignevents |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `campaign_event_id` | NUMBER |  | 39 | 0 | `campaignevent`.`campaign_event_id` | foreign_key | `purpose`: `campaign_event_id` |
| `campaign_response_extid` | VARCHAR2 | 255 |  |  |  |  | `purpose`: `campaign_response_external_id` |
| `campaign_response_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `campaign_response_id` |
| `contact_id` | NUMBER |  | 39 | 0 | `contacts`.`contact_id` | foreign_key | `purpose`: `contact_id` |
| `date_sent` | TIMESTAMP |  |  |  |  |  | `purpose`: `date_sent` |
| `entity_id` | NUMBER |  | 39 | 0 | `entity`.`entity_id` | foreign_key | `purpose`: `entity_id` |
| `response_id` | VARCHAR2 | 30 |  |  |  |  | `purpose`: `response_id` |

## Foreign keys

| Key | Columns | References table | Referenced columns |
|---|---|---|---|
| `campaignresponse_campaignevent_fk` | `campaign_event_id` | `campaignevent` | `campaign_event_id` |
| `campaignresponse_contacts_fk` | `contact_id` | `contacts` | `contact_id` |
| `campaignresponse_entity_fk` | `entity_id` | `entity` | `entity_id` |

## Referenced by

| Key | Table | Columns | Columns here |
|---|---|---|---|
| `Campaignresponsehistory_campaignresponse_fk` | `campaignresponsehistory` | `campaign_response_id` | `campaign_response_id` |
