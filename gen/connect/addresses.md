# Addresses (`addresses`)
<!-- id: connect:addresses -->

| Fact | Value |
|---|---|
| Table | `addresses` |
| Primary key | `address_id` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `address` | VARCHAR2 | 999 |  |  |  |  | `purpose`: `full_address_text`; `value_source`: `built_from_address_component_fields` |
| `address_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `address_id` |
| `address_line_1` | VARCHAR2 | 150 |  |  |  |  | `purpose`: `address_line_1` |
| `address_line_2` | VARCHAR2 | 150 |  |  |  |  | `purpose`: `address_line_2` |
| `address_line_3` | VARCHAR2 | 150 |  |  |  |  | `purpose`: `address_line_3` |
| `attention` | VARCHAR2 | 150 |  |  |  |  | `purpose`: `attention` |
| `city` | VARCHAR2 | 50 |  |  |  |  | `purpose`: `city` |
| `company` | VARCHAR2 | 100 |  |  |  |  | `purpose`: `company` |
| `country` | VARCHAR2 | 50 |  |  |  |  | `purpose`: `country` |
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `date_last_modified` |
| `is_overridden` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `custom_address_text_used`; `false_means`: `generated_address_text_used` |
| `phone` | VARCHAR2 | 100 |  |  |  |  | `purpose`: `phone_number` |
| `state` | VARCHAR2 | 50 |  |  |  |  | `purpose`: `state` |
| `zip` | VARCHAR2 | 36 |  |  |  |  | `purpose`: `zip_code` |

## Referenced by

| Key | Table | Columns | Columns here |
|---|---|---|---|
| `Address_book_addresses_fk` | `address_book` | `address_id` | `address_id` |
