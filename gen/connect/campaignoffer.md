# Campaignoffer (`campaignoffer`)
<!-- id: connect:campaignoffer -->

| Fact | Value |
|---|---|
| Table | `campaignoffer` |
| Record | `campaignoffer` |
| Primary key | `campaign_offer_id` |
| Domains | Campaigns |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `campaign_offer_extid` | VARCHAR2 | 255 |  |  |  |  | `purpose`: `campaign_offer_external_id` |
| `campaign_offer_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `campaign_offer_id` |
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `date_last_modified` |
| `description` | VARCHAR2 | 500 |  |  |  |  | `purpose`: `description` |
| `is_inactive` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `is_inactive` |
| `name` | VARCHAR2 | 60 |  |  |  |  | `purpose`: `name` |

## Referenced by

| Key | Table | Columns | Columns here |
|---|---|---|---|
| `Campaigns_campaignoffer_fk` | `campaigns` | `offer_id` | `campaign_offer_id` |
