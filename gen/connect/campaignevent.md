# Campaignevent (`campaignevent`)
<!-- id: connect:campaignevent -->

| Fact | Value |
|---|---|
| Table | `campaignevent` |
| Primary key | `campaign_event_id` |
| Domains | Campaignevents, Campaigns |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `campaign_event_extid` | VARCHAR2 | 255 |  |  |  |  | `purpose`: `campaign_event_external_id` |
| `campaign_event_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `campaign_event_id` |
| `campaign_id` | NUMBER |  | 39 | 0 | `campaigns`.`campaign_id` | foreign_key | `purpose`: `campaign_id` |
| `channel_id` | NUMBER |  | 39 | 0 | `campaignchannel`.`campaign_channel_id` | foreign_key | `purpose`: `campaign_channel_id` |
| `cost_0` | NUMBER |  | 20 | 2 |  |  | `purpose`: `cost` |
| `date_scheduled` | TIMESTAMP |  |  |  |  |  | `purpose`: `date_scheduled` |
| `date_sent` | TIMESTAMP |  |  |  |  |  | `purpose`: `date_sent` |
| `group_id` | NUMBER |  | 39 | 0 |  | foreign_key | `purpose`: `group_id` |
| `number_clicked_thru` | NUMBER |  | 39 | 0 |  |  | `purpose`: `number_clicked_through` |
| `number_received` | NUMBER |  | 39 | 0 |  |  | `purpose`: `number_received` |
| `number_responded` | NUMBER |  | 39 | 0 |  |  | `purpose`: `number_responded` |
| `number_sent` | NUMBER |  | 39 | 0 |  |  | `purpose`: `number_sent` |
| `number_unsubscribed` | NUMBER |  | 39 | 0 |  |  | `purpose`: `number_unsubscribed` |
| `promotion_code_id` | NUMBER |  | 39 | 0 | `promotion_codes`.`promotion_code_id` | foreign_key | `purpose`: `promotion_code_id` |
| `status_id` | VARCHAR2 | 12 |  |  |  |  | `purpose`: `status_id` |
| `subscription_id` | NUMBER |  | 39 | 0 | `campaign_subscriptions`.`campaignsubscription_id` | foreign_key | `purpose`: `campaign_subscription_id` |
| `template_id` | NUMBER |  | 39 | 0 | `crmtemplate`.`crmtemplate_id` | foreign_key | `purpose`: `template_id` |
| `test_cell_id` | NUMBER |  | 39 | 0 | `group_test_cell`.`test_cell_id` | foreign_key | `purpose`: `test_cell_id` |

## Foreign keys

| Key | Columns | References table | Referenced columns |
|---|---|---|---|
| `campaignevent_campaign_subscriptions_fk` | `subscription_id` | `campaign_subscriptions` | `campaignsubscription_id` |
| `campaignevent_campaignchannel_fk` | `channel_id` | `campaignchannel` | `campaign_channel_id` |
| `campaignevent_campaigns_fk` | `campaign_id` | `campaigns` | `campaign_id` |
| `campaignevent_crmgroup_fk` | `group_id` | `crmgroup` | `group_id` |
| `campaignevent_crmtemplate_fk` | `template_id` | `crmtemplate` | `crmtemplate_id` |
| `campaignevent_group_test_cell_fk` | `test_cell_id`, `group_id` | `group_test_cell` | `test_cell_id`, `crm_group_id` |
| `campaignevent_promotion_codes_fk` | `promotion_code_id` | `promotion_codes` | `promotion_code_id` |

## Referenced by

| Key | Table | Columns | Columns here |
|---|---|---|---|
| `Campaignresponse_campaignevent_fk` | `campaignresponse` | `campaign_event_id` | `campaign_event_id` |
