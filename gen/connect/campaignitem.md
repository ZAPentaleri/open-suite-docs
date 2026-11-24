# Campaignitem (`campaignitem`)
<!-- id: connect:campaignitem -->

| Fact | Value |
|---|---|
| Table | `campaignitem` |
| Domains | Campaigns |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `campaign_id` | NUMBER |  | 39 | 0 | `campaigns`.`campaign_id` | foreign_key | `purpose`: `campaign_id` |
| `item_id` | NUMBER |  | 39 | 0 | `items`.`item_id` | foreign_key | `purpose`: `item_id` |

## Foreign keys

| Key | Columns | References table | Referenced columns |
|---|---|---|---|
| `campaignitem_campaigns_fk` | `campaign_id` | `campaigns` | `campaign_id` |
| `campaignitem_items_fk` | `item_id` | `items` | `item_id` |
