# Campaign_subscriptions (`campaign_subscriptions`)
<!-- id: connect:campaign_subscriptions -->

| Fact | Value |
|---|---|
| Table | `campaign_subscriptions` |
| Record | `campaignsubscription` |
| Primary key | `campaignsubscription_id` |
| Domains | Campaignevents |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `campaignsubscription_extid` | VARCHAR2 | 255 |  |  |  |  | `purpose`: `campaign_subscription_external_id` |
| `campaignsubscription_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `campaign_subscription_id` |
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `date_last_modified` |
| `description` | VARCHAR2 | 499 |  |  |  |  | `purpose`: `description` |
| `isinactive` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `subscription_is_inactive` |
| `name` | VARCHAR2 | 100 |  |  |  |  | `purpose`: `name` |

## Referenced by

| Key | Table | Columns | Columns here |
|---|---|---|---|
| `Campaign_subscription_statuses_campaign_subscriptions_fk` | `campaign_subscription_statuses` | `subscription_id` | `campaignsubscription_id` |
| `Campaignevent_campaign_subscriptions_fk` | `campaignevent` | `subscription_id` | `campaignsubscription_id` |
