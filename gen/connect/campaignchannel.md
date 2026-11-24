# Campaignchannel (`campaignchannel`)
<!-- id: connect:campaignchannel -->

| Fact | Value |
|---|---|
| Table | `campaignchannel` |
| Record | `campaignchannel` |
| Primary key | `campaign_channel_id` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `campaign_channel_extid` | VARCHAR2 | 255 |  |  |  |  | `purpose`: `campaign_channel_external_id` |
| `campaign_channel_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `campaign_channel_id` |
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `date_last_modified` |
| `description` | VARCHAR2 | 500 |  |  |  |  | `purpose`: `description` |
| `event_type_id` | VARCHAR2 | 19 |  |  |  |  | `purpose`: `campaign_event_id` |
| `is_inactive` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `is_inactive` |
| `name` | VARCHAR2 | 60 |  |  |  |  | `purpose`: `name` |

## Referenced by

| Key | Table | Columns | Columns here |
|---|---|---|---|
| `Campaignevent_campaignchannel_fk` | `campaignevent` | `channel_id` | `campaign_channel_id` |
