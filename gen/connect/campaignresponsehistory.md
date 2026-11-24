# Campaignresponsehistory (`campaignresponsehistory`)
<!-- id: connect:campaignresponsehistory -->

| Fact | Value |
|---|---|
| Table | `campaignresponsehistory` |
| Domains | Campaignevents |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `author` | NUMBER |  | 39 | 0 | `entity`.`entity_id` | foreign_key | `purpose`: `author` |
| `campaign_response_id` | NUMBER |  | 39 | 0 | `campaignresponse`.`campaign_response_id` | foreign_key | `purpose`: `campaign_response_id` |
| `date_0` | TIMESTAMP |  |  |  |  |  | `purpose`: `date` |
| `note` | VARCHAR2 | 4000 |  |  |  |  | `purpose`: `note` |
| `response_id` | VARCHAR2 | 30 |  |  |  |  | `purpose`: `response_id` |
| `status_detail` | VARCHAR2 | 4000 |  |  |  |  | `purpose`: `response_details` |
| `transaction_id` | NUMBER |  | 39 | 0 | `transactions`.`transaction_id` | foreign_key | `purpose`: `transaction_id` |

## Foreign keys

| Key | Columns | References table | Referenced columns |
|---|---|---|---|
| `campaignresponsehistory_campaignresponse_fk` | `campaign_response_id` | `campaignresponse` | `campaign_response_id` |
| `campaignresponsehistory_entity_fk` | `author` | `entity` | `entity_id` |
| `campaignresponsehistory_transactions_fk` | `transaction_id` | `transactions` | `transaction_id` |
