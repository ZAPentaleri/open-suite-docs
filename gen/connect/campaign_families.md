# Campaign_families (`campaign_families`)
<!-- id: connect:campaign_families -->

| Fact | Value |
|---|---|
| Table | `campaign_families` |
| Primary key | `campaignfamily_id` |
| Domains | Campaigns |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `campaignfamily_extid` | VARCHAR2 | 255 |  |  |  |  | `purpose`: `campaign_family_external_id` |
| `campaignfamily_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `campaign_family_id` |
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `date_last_modified` |
| `description` | VARCHAR2 | 500 |  |  |  |  | `purpose`: `description` |
| `isinactive` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `is_inactive` |
| `name` | VARCHAR2 | 60 |  |  |  |  | `purpose`: `name` |

## Referenced by

| Key | Table | Columns | Columns here |
|---|---|---|---|
| `Campaigns_campaign_families_fk` | `campaigns` | `family_id` | `campaignfamily_id` |
