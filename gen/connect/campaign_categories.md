# Campaign_categories (`campaign_categories`)
<!-- id: connect:campaign_categories -->

| Fact | Value |
|---|---|
| Table | `campaign_categories` |
| Primary key | `campaigncategory_id` |
| Domains | Campaigns |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `campaigncategory_extid` | VARCHAR2 | 255 |  |  |  |  | `purpose`: `campaign_category_external_id` |
| `campaigncategory_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `campaign_category_id` |
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `date_last_modified` |
| `default_campaign_id` | NUMBER |  | 39 | 0 | `campaigns`.`campaign_id` | foreign_key | `purpose`: `default_campaign_id` |
| `description` | VARCHAR2 | 1499 |  |  |  |  | `purpose`: `description` |
| `is_available_externally` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `available_externally` |
| `isinactive` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `is_inactive` |
| `name` | VARCHAR2 | 120 |  |  |  |  | `purpose`: `name` |
| `parent_id` | NUMBER |  | 39 | 0 | `campaign_categories`.`campaigncategory_id` | foreign_key | `purpose`: `parent_id` |

## Foreign keys

| Key | Columns | References table | Referenced columns |
|---|---|---|---|
| `campaign_categories_campaign_categories_fk` | `parent_id` | `campaign_categories` | `campaigncategory_id` |
| `campaign_categories_campaigns_fk` | `default_campaign_id` | `campaigns` | `campaign_id` |

## Referenced by

| Key | Table | Columns | Columns here |
|---|---|---|---|
| `Campaign_categories_campaign_categories_fk` | `campaign_categories` | `parent_id` | `campaigncategory_id` |
| `Campaigns_campaign_categories_fk` | `campaigns` | `category_id` | `campaigncategory_id` |
