# Campaigns (`campaigns`)
<!-- id: connect:campaigns -->

| Fact | Value |
|---|---|
| Table | `campaigns` |
| Record | `campaign` |
| Primary key | `campaign_id` |
| Domains | Campaignevents, Campaigns |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `audience_id` | NUMBER |  | 39 | 0 | `campaign_audiences`.`campaignaudience_id` | foreign_key | `purpose`: `audience_id` |
| `campaign_extid` | VARCHAR2 | 255 |  |  |  |  | `purpose`: `campaign_external_id` |
| `campaign_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `campaign_id` |
| `category_id` | NUMBER |  | 39 | 0 | `campaign_categories`.`campaigncategory_id` | foreign_key | `purpose`: `category_id` |
| `cost_0` | NUMBER |  | 20 | 2 |  |  | `purpose`: `cost` |
| `date_created` | TIMESTAMP |  |  |  |  |  | `purpose`: `date_created` |
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `date_last_modified` |
| `end_date` | TIMESTAMP |  |  |  |  |  | `purpose`: `end_date` |
| `expectedrevenue` | NUMBER |  | 20 | 2 |  |  | `purpose`: `expected_revenue` |
| `family_id` | NUMBER |  | 39 | 0 | `campaign_families`.`campaignfamily_id` | foreign_key | `purpose`: `family_id` |
| `is_inactive` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `campaign_is_inactive` |
| `is_sales_campaign` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `is_sales_campaign`; `false_means`: `marketing_campaign` |
| `keyword` | VARCHAR2 | 99 |  |  |  |  | `purpose`: `keyword` |
| `last_modified_date` | TIMESTAMP |  |  |  |  |  | `purpose`: `last_modified_date` |
| `message` | VARCHAR2 | 4000 |  |  |  |  | `purpose`: `message` |
| `number_0` | VARCHAR2 | 99 |  |  |  |  | `purpose`: `number` |
| `offer_id` | NUMBER |  | 39 | 0 | `campaignoffer`.`campaign_offer_id` | foreign_key | `purpose`: `offer_id` |
| `organizer_id` | NUMBER |  | 39 | 0 | `entity`.`entity_id` | foreign_key | `purpose`: `organizer_id` |
| `search_engine_id` | NUMBER |  | 39 | 0 | `campaign_search_engine`.`search_engine_id` | foreign_key | `purpose`: `search_engine_id` |
| `start_date` | TIMESTAMP |  |  |  |  |  | `purpose`: `start_date` |
| `time_zone_0` | VARCHAR2 | 30 |  |  |  |  | `purpose`: `time_zone` |
| `title` | VARCHAR2 | 256 |  |  |  |  | `purpose`: `title` |
| `url` | VARCHAR2 | 255 |  |  |  |  | `purpose`: `url` |
| `vertical_id` | NUMBER |  | 39 | 0 | `campaign_verticals`.`campaignvertical_id` | foreign_key | `purpose`: `vertical_id` |

## Foreign keys

| Key | Columns | References table | Referenced columns |
|---|---|---|---|
| `campaigns_campaign_audiences_fk` | `audience_id` | `campaign_audiences` | `campaignaudience_id` |
| `campaigns_campaign_categories_fk` | `category_id` | `campaign_categories` | `campaigncategory_id` |
| `campaigns_campaign_families_fk` | `family_id` | `campaign_families` | `campaignfamily_id` |
| `campaigns_campaign_search_engine_fk` | `search_engine_id` | `campaign_search_engine` | `search_engine_id` |
| `campaigns_campaign_verticals_fk` | `vertical_id` | `campaign_verticals` | `campaignvertical_id` |
| `campaigns_campaignoffer_fk` | `offer_id` | `campaignoffer` | `campaign_offer_id` |
| `campaigns_entity_fk` | `organizer_id` | `entity` | `entity_id` |

## Referenced by

| Key | Table | Columns | Columns here |
|---|---|---|---|
| `Campaign_categories_campaigns_fk` | `campaign_categories` | `default_campaign_id` | `campaign_id` |
| `Campaignevent_campaigns_fk` | `campaignevent` | `campaign_id` | `campaign_id` |
| `Campaignitem_campaigns_fk` | `campaignitem` | `campaign_id` | `campaign_id` |
| `Customers_campaigns_fk` | `customers` | `lead_source_id` | `campaign_id` |
| `Opportunities_campaigns_fk` | `opportunities` | `lead_source_id` | `campaign_id` |
