# Campaign_audiences (`campaign_audiences`)
<!-- id: connect:campaign_audiences -->

| Fact | Value |
|---|---|
| Table | `campaign_audiences` |
| Record | `campaignaudience` |
| Primary key | `campaignaudience_id` |
| Domains | Campaigns |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `campaignaudience_extid` | VARCHAR2 | 255 |  |  |  |  | `purpose`: `campaign_audience_external_id` |
| `campaignaudience_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `campaign_audience_id` |
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `date_last_modified` |
| `description` | VARCHAR2 | 500 |  |  |  |  | `purpose`: `description` |
| `isinactive` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `is_inactive` |
| `name` | VARCHAR2 | 60 |  |  |  |  | `purpose`: `name` |

## Referenced by

| Key | Table | Columns | Columns here |
|---|---|---|---|
| `Campaigns_campaign_audiences_fk` | `campaigns` | `audience_id` | `campaignaudience_id` |
