# Notes_user (`notes_user`)
<!-- id: connect:notes_user -->

| Fact | Value |
|---|---|
| Table | `notes_user` |
| Primary key | `note_id` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `author_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `author_id` |
| `comments` | VARCHAR2 | 4000 |  |  |  |  | `purpose`: `comments` |
| `company_id` | NUMBER |  | 39 | 0 | `entity`.`entity_id` | foreign_key | `purpose`: `company_id` |
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `date_last_modified`; `time_zone`: `gmt` |
| `direction` | VARCHAR2 | 128 |  |  |  |  | `purpose`: `direction` |
| `event_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `event_id` |
| `isinactive` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `is_inactive` |
| `item_id` | NUMBER |  | 39 | 0 | `items`.`item_id` | foreign_key | `purpose`: `item_id` |
| `name` | VARCHAR2 | 120 |  |  |  |  | `purpose`: `name` |
| `note_extid` | VARCHAR2 | 255 |  |  |  |  | `purpose`: `note_external_id` |
| `note_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `note_id` |
| `note_type` | VARCHAR2 | 40 |  |  |  |  | `purpose`: `note_type` |
| `note_type_description` | VARCHAR2 | 75 |  |  |  |  | `purpose`: `note_type_description` |
| `note_type_extid` | VARCHAR2 | 255 |  |  |  |  | `purpose`: `note_type_external_id` |
| `note_type_id` | NUMBER |  | 39 | 0 | `notetype`.`note_type_id` | foreign_key | `purpose`: `note_type_id` |
| `record_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `record_id` |
| `record_type_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `record_type_id` |
| `system_use` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `system_use` |
| `time_entered` | TIMESTAMP |  |  |  |  |  | `purpose`: `time_entered` |
| `transaction_id` | NUMBER |  | 39 | 0 | `transactions`.`transaction_id` | foreign_key | `purpose`: `transaction_id` |

## Foreign keys

| Key | Columns | References table | Referenced columns |
|---|---|---|---|
| `notes_user_entity_fk` | `company_id` | `entity` | `entity_id` |
| `notes_user_items_fk` | `item_id` | `items` | `item_id` |
| `notes_user_notetype_fk` | `note_type_id` | `notetype` | `note_type_id` |
| `notes_user_transactions_fk` | `transaction_id` | `transactions` | `transaction_id` |
