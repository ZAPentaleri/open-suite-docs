# Campaign_subscription_statuses (`campaign_subscription_statuses`)
<!-- id: connect:campaign_subscription_statuses -->

| Fact | Value |
|---|---|
| Table | `campaign_subscription_statuses` |
| Primary key | `subscription_id`, `entity_id` |
| Domains | Campaignevents |
| Tags | `composite_primary_key` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `date_last_modified` |
| `entity_id` | NUMBER |  | 39 | 0 | `entity`.`entity_id` | primary_key | `purpose`: `entity_id` |
| `last_modified_date` | TIMESTAMP |  |  |  |  |  | `purpose`: `date_last_modified` |
| `subscription_id` | NUMBER |  | 39 | 0 | `campaign_subscriptions`.`campaignsubscription_id` | primary_key | `purpose`: `campaign_subscription_id` |
| `unsubscribed` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `is_unsubscribed` |

## Foreign keys

| Key | Columns | References table | Referenced columns |
|---|---|---|---|
| `campaign_subscription_statuses_campaign_subscriptions_fk` | `subscription_id` | `campaign_subscriptions` | `campaignsubscription_id` |
| `campaign_subscription_statuses_entity_fk` | `entity_id` | `entity` | `entity_id` |
