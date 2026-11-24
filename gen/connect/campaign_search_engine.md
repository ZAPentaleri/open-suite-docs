# Campaign_search_engine (`campaign_search_engine`)
<!-- id: connect:campaign_search_engine -->

| Fact | Value |
|---|---|
| Table | `campaign_search_engine` |
| Record | `campaignsearchengine` |
| Primary key | `search_engine_id` |
| Domains | Campaigns |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `date_last_modified` |
| `description` | VARCHAR2 | 500 |  |  |  |  | `purpose`: `search_engine_description` |
| `isinactive` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `is_inactive` |
| `name` | VARCHAR2 | 60 |  |  |  |  | `purpose`: `search_engine_name` |
| `search_engine_extid` | VARCHAR2 | 255 |  |  |  |  | `purpose`: `search_engine_external_id` |
| `search_engine_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `search_engine_id` |

## Referenced by

| Key | Table | Columns | Columns here |
|---|---|---|---|
| `Campaigns_campaign_search_engine_fk` | `campaigns` | `search_engine_id` | `search_engine_id` |
