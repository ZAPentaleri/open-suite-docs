# Transaction_history (`transaction_history`)
<!-- id: connect:transaction_history -->

| Fact | Value |
|---|---|
| Table | `transaction_history` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `account_id` | NUMBER |  | 39 | 0 | `accounts`.`account_id` | foreign_key | `purpose`: `account_id` |
| `action` | VARCHAR2 | 8 |  |  |  |  | `purpose`: `change_type` |
| `amount` | NUMBER |  | 20 | 2 |  |  | `purpose`: `amount` |
| `date_time` | TIMESTAMP |  |  |  |  |  | `purpose`: `change_timestamp` |
| `date_transaction` | TIMESTAMP |  |  |  |  |  | `purpose`: `the_transaction_date` |
| `document_number` | VARCHAR2 | 138 |  |  |  |  | `purpose`: `transaction_document_number` |
| `entity_id` | NUMBER |  | 39 | 0 | `entity`.`entity_id` | foreign_key | `purpose`: `entity_id` |
| `transaction_id` | NUMBER |  | 39 | 0 | `transactions`.`transaction_id` | foreign_key | `purpose`: `internal_transaction_id` |
| `transaction_number` | VARCHAR2 | 138 |  |  |  |  | `purpose`: `transaction_number` |
| `transaction_type` | VARCHAR2 | 8 |  |  |  |  | `purpose`: `transaction_type` |
| `user_id` | NUMBER |  | 39 | 0 | `entity`.`entity_id` | foreign_key | `purpose`: `user_id` |

## Foreign keys

| Key | Columns | References table | Referenced columns |
|---|---|---|---|
| `transaction_history_accounts_fk` | `account_id` | `accounts` | `account_id` |
| `transaction_history_entity_fk` | `user_id` | `entity` | `entity_id` |
| `transaction_history_entity_fk_2` | `entity_id` | `entity` | `entity_id` |
| `transaction_history_transactions_fk` | `transaction_id` | `transactions` | `transaction_id` |
