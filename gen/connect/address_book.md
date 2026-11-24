# Address_book (`address_book`)
<!-- id: connect:address_book -->

| Fact | Value |
|---|---|
| Table | `address_book` |
| Primary key | `address_book_id` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `address` | VARCHAR2 | 999 |  |  |  |  | `purpose`: `address` |
| `address_book_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `address_book_id` |
| `address_id` | NUMBER |  | 39 | 0 | `addresses`.`address_id` | foreign_key | `purpose`: `address_id` |
| `address_line_1` | VARCHAR2 | 150 |  |  |  |  | `purpose`: `address_line_1` |
| `address_line_2` | VARCHAR2 | 150 |  |  |  |  | `purpose`: `address_line_2` |
| `address_line_3` | VARCHAR2 | 150 |  |  |  |  | `purpose`: `address_line_3` |
| `attention` | VARCHAR2 | 150 |  |  |  |  | `purpose`: `attention` |
| `city` | VARCHAR2 | 50 |  |  |  |  | `purpose`: `city` |
| `company` | VARCHAR2 | 100 |  |  |  |  | `purpose`: `company` |
| `country` | VARCHAR2 | 50 |  |  |  |  | `purpose`: `country` |
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `date_last_modified` |
| `entity_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `entity_id` |
| `is_default_bill_address` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `default_billing_address` |
| `is_default_ship_address` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `default_shipping_address` |
| `is_inactive` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `whether_is_inactive` |
| `name` | VARCHAR2 | 150 |  |  |  |  | `purpose`: `name` |
| `phone` | VARCHAR2 | 100 |  |  |  |  | `purpose`: `phone_number` |
| `state` | VARCHAR2 | 50 |  |  |  |  | `purpose`: `state` |
| `zip` | VARCHAR2 | 36 |  |  |  |  | `purpose`: `zip` |

## Foreign keys

| Key | Columns | References table | Referenced columns |
|---|---|---|---|
| `address_book_addresses_fk` | `address_id` | `addresses` | `address_id` |

## Referenced by

| Key | Table | Columns | Columns here |
|---|---|---|---|
| `Billing_accounts_address_book_fk_1` | `billing_accounts` | `bill_to_address_book_id` | `address_book_id` |
| `Billing_accounts_address_book_fk_2` | `billing_accounts` | `ship_to_address_book_id` | `address_book_id` |
