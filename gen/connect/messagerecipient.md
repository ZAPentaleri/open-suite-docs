# Messagerecipient (`messagerecipient`)
<!-- id: connect:messagerecipient -->

| Fact | Value |
|---|---|
| Table | `messagerecipient` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `entity_id` | NUMBER |  | 39 | 0 | `entity`.`entity_id` | foreign_key | `purpose`: `entity_id` |
| `message_id` | NUMBER |  | 39 | 0 | `message`.`message_id` | foreign_key | `purpose`: `message_id` |

## Foreign keys

| Key | Columns | References table | Referenced columns |
|---|---|---|---|
| `messagerecipient_entity_fk` | `entity_id` | `entity` | `entity_id` |
| `messagerecipient_message_fk` | `message_id` | `message` | `message_id` |
