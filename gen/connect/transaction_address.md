# Transaction_address (`transaction_address`)
<!-- id: connect:transaction_address -->

| Fact | Value |
|---|---|
| Table | `transaction_address` |
| Primary key | `transaction_id`, `transaction_address_id` |
| Tags | `composite_primary_key` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `bill_address_line_1` | VARCHAR2 | 150 |  |  |  |  | `purpose`: `billing_address_line_1` |
| `bill_address_line_2` | VARCHAR2 | 150 |  |  |  |  | `purpose`: `billing_address_line_2` |
| `bill_address_line_3` | VARCHAR2 | 150 |  |  |  |  | `purpose`: `billing_address_line_3` |
| `bill_city` | VARCHAR2 | 50 |  |  |  |  | `purpose`: `billing_city` |
| `bill_company` | VARCHAR2 | 100 |  |  |  |  | `purpose`: `billing_company` |
| `bill_country` | VARCHAR2 | 50 |  |  |  |  | `purpose`: `billing_country` |
| `bill_name` | VARCHAR2 | 150 |  |  |  |  | `purpose`: `billing_name` |
| `bill_phone_number` | VARCHAR2 | 100 |  |  |  |  | `purpose`: `billing_phone_number` |
| `bill_state` | VARCHAR2 | 50 |  |  |  |  | `purpose`: `billing_state` |
| `bill_zip` | VARCHAR2 | 36 |  |  |  |  | `purpose`: `billing_zip` |
| `created_by_id` | NUMBER |  | 39 | 0 | `entity`.`entity_id` | foreign_key | `purpose`: `created_by_id` |
| `date_created` | TIMESTAMP |  |  |  |  |  | `purpose`: `date_created`; `time_zone`: `gmt` |
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `last_modified_date`; `time_zone`: `gmt`; `caveat`: `no_query_data_before_2017_1` |
| `last_modified_by_id` | NUMBER |  | 39 | 0 | `entity`.`entity_id` | foreign_key | `purpose`: `last_modified_by_id` |
| `return_address_line_1` | VARCHAR2 | 150 |  |  |  |  | `purpose`: `return_address_line_1` |
| `return_address_line_2` | VARCHAR2 | 150 |  |  |  |  | `purpose`: `return_address_line_2` |
| `return_city` | VARCHAR2 | 50 |  |  |  |  | `purpose`: `return_city` |
| `return_country` | VARCHAR2 | 50 |  |  |  |  | `purpose`: `return_country` |
| `return_state` | VARCHAR2 | 50 |  |  |  |  | `purpose`: `return_state` |
| `return_zipcode` | VARCHAR2 | 36 |  |  |  |  | `purpose`: `return_zip_code` |
| `ship_address_line_1` | VARCHAR2 | 150 |  |  |  |  | `purpose`: `shipping_address_line_1` |
| `ship_address_line_2` | VARCHAR2 | 150 |  |  |  |  | `purpose`: `shipping_address_line_2` |
| `ship_address_line_3` | VARCHAR2 | 150 |  |  |  |  | `purpose`: `shipping_address_line_3` |
| `ship_attention` | VARCHAR2 | 249 |  |  |  |  | `purpose`: `shipping_address_attention` |
| `ship_city` | VARCHAR2 | 50 |  |  |  |  | `purpose`: `shipping_city` |
| `ship_company` | VARCHAR2 | 100 |  |  |  |  | `purpose`: `shipping_company` |
| `ship_country` | VARCHAR2 | 50 |  |  |  |  | `purpose`: `shipping_country` |
| `ship_name` | VARCHAR2 | 150 |  |  |  |  | `purpose`: `shipping_name` |
| `ship_phone_number` | VARCHAR2 | 100 |  |  |  |  | `purpose`: `shipping_phone_number` |
| `ship_state` | VARCHAR2 | 50 |  |  |  |  | `purpose`: `shipping_state` |
| `ship_zip` | VARCHAR2 | 36 |  |  |  |  | `purpose`: `shipping_zip_code` |
| `transaction_address_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `transaction_line_address_id` |
| `transaction_id` | NUMBER |  | 39 | 0 | `transactions`.`transaction_id` | primary_key | `purpose`: `transaction_id` |

## Foreign keys

| Key | Columns | References table | Referenced columns |
|---|---|---|---|
| `transaction_address_entity_fk` | `created_by_id` | `entity` | `entity_id` |
| `transaction_address_entity_fk_2` | `last_modified_by_id` | `entity` | `entity_id` |
| `transaction_address_transactions_fk` | `transaction_id` | `transactions` | `transaction_id` |
