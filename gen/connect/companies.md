# Companies (`companies`)
<!-- id: connect:companies -->

| Fact | Value |
|---|---|
| Table | `companies` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `address` | VARCHAR2 | 999 |  |  |  |  | `purpose`: `address` |
| `address1` | VARCHAR2 | 150 |  |  |  |  | `purpose`: `address_line_1` |
| `address2` | VARCHAR2 | 150 |  |  |  |  | `purpose`: `address_line_2` |
| `address3` | VARCHAR2 | 150 |  |  |  |  | `purpose`: `address_line_3` |
| `city` | VARCHAR2 | 50 |  |  |  |  | `purpose`: `city` |
| `comments` | VARCHAR2 | 4000 |  |  |  |  | `purpose`: `comments` |
| `company_extid` | VARCHAR2 | 255 |  |  |  |  | `purpose`: `company_external_id` |
| `company_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `company_entity_id`; `allowed_values_listed_in`: `customers`, `vendors`, `partners`, `other_names`; `caveat`: `other_names_are_entities_outside_other_types`; `use_with`: `company_type_column_for_entity_type` |
| `company_type` | VARCHAR2 | 4000 |  |  |  |  | `purpose`: `company_entity_type`; `options`: `customer`, `vendor`, `partner`, `other_name` |
| `companyname` | VARCHAR2 | 83 |  |  |  |  | `purpose`: `company_name` |
| `country` | VARCHAR2 | 50 |  |  |  |  | `purpose`: `country` |
| `create_date` | TIMESTAMP |  |  |  |  |  | `purpose`: `create_date` |
| `currency_id` | NUMBER |  | 39 | 0 | `currencies`.`currency_id` | foreign_key | `purpose`: `currency_id` |
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `date_last_modified` |
| `email` | VARCHAR2 | 254 |  |  |  |  | `purpose`: `company_email` |
| `full_name` | VARCHAR2 | 1800 |  |  |  |  | `purpose`: `full_name` |
| `isinactive` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `company_is_inactive` |
| `last_modified_date` | TIMESTAMP |  |  |  |  |  | `purpose`: `date_last_modified` |
| `loginaccess` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `login_access` |
| `name` | VARCHAR2 | 83 |  |  |  |  | `purpose`: `name` |
| `parent_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `subcompany_of` |
| `phone` | VARCHAR2 | 100 |  |  |  |  | `purpose`: `phone` |
| `state` | VARCHAR2 | 50 |  |  |  |  | `purpose`: `province_state` |
| `subsidiary` | NUMBER |  | 39 | 0 | `subsidiaries`.`subsidiary_id` | foreign_key | `purpose`: `subsidiary` |
| `url` | VARCHAR2 | 100 |  |  |  |  | `purpose`: `web_address` |
| `zipcode` | VARCHAR2 | 36 |  |  |  |  | `purpose`: `zip_code` |

## Foreign keys

| Key | Columns | References table | Referenced columns |
|---|---|---|---|
| `companies_currencies_fk` | `currency_id` | `currencies` | `currency_id` |
| `companies_subsidiaries_fk` | `subsidiary` | `subsidiaries` | `subsidiary_id` |
