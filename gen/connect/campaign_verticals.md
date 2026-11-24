# Campaign_verticals (`campaign_verticals`)
<!-- id: connect:campaign_verticals -->

| Fact | Value |
|---|---|
| Table | `campaign_verticals` |
| Record | `campaignvertical` |
| Primary key | `campaignvertical_id` |
| Domains | Campaigns |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `campaignvertical_extid` | VARCHAR2 | 255 |  |  |  |  | `purpose`: `campaign_vertical_external_id` |
| `campaignvertical_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `campaign_vertical_id` |
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `date_last_modified` |
| `description` | VARCHAR2 | 500 |  |  |  |  | `purpose`: `description` |
| `isinactive` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `is_inactive` |
| `name` | VARCHAR2 | 60 |  |  |  |  | `purpose`: `name` |

## Referenced by

| Key | Table | Columns | Columns here |
|---|---|---|---|
| `Campaigns_campaign_verticals_fk` | `campaigns` | `vertical_id` | `campaignvertical_id` |
