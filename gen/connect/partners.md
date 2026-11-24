# Partners (`partners`)
<!-- id: connect:partners -->

| Fact | Value |
|---|---|
| Table | `partners` |
| Record | `partner` |
| Primary key | `partner_id` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `account_number` | VARCHAR2 | 99 |  |  |  |  | `purpose`: `account_number` |
| `address` | VARCHAR2 | 999 |  |  |  |  | `purpose`: `address` |
| `address1` | VARCHAR2 | 150 |  |  |  |  | `purpose`: `address_line_1` |
| `address2` | VARCHAR2 | 150 |  |  |  |  | `purpose`: `address_line_2` |
| `address3` | VARCHAR2 | 150 |  |  |  |  | `purpose`: `address_line_3` |
| `altemail` | VARCHAR2 | 254 |  |  |  |  | `purpose`: `alt_email` |
| `altphone` | VARCHAR2 | 100 |  |  |  |  | `purpose`: `alt_phone` |
| `assigntasks` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `can_be_assigned_tasks` |
| `city` | VARCHAR2 | 50 |  |  |  |  | `purpose`: `city` |
| `code` | VARCHAR2 | 200 |  |  |  |  | `purpose`: `promotion_code` |
| `comments` | VARCHAR2 | 4000 |  |  |  |  | `purpose`: `comments` |
| `companyname` | VARCHAR2 | 83 |  |  |  |  | `purpose`: `company_name` |
| `country` | VARCHAR2 | 50 |  |  |  |  | `purpose`: `country` |
| `create_date` | TIMESTAMP |  |  |  |  |  | `purpose`: `create_date`; `time_zone`: `gmt` |
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `date_last_modified` |
| `email` | VARCHAR2 | 254 |  |  |  |  | `purpose`: `email` |
| `fax` | VARCHAR2 | 100 |  |  |  |  | `purpose`: `fax` |
| `full_name` | VARCHAR2 | 1800 |  |  |  |  | `purpose`: `full_name` |
| `home_phone` | VARCHAR2 | 100 |  |  |  |  | `purpose`: `home_phone` |
| `is_person` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `type_company_or_individual` |
| `isinactive` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `partner_is_inactive` |
| `last_modified_date` | TIMESTAMP |  |  |  |  |  | `purpose`: `last_modified_date`; `time_zone`: `gmt` |
| `loginaccess` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `give_access` |
| `mobile_phone` | VARCHAR2 | 100 |  |  |  |  | `purpose`: `mobile_phone` |
| `name` | VARCHAR2 | 83 |  |  |  |  | `purpose`: `name` |
| `partner_extid` | VARCHAR2 | 255 |  |  |  |  | `purpose`: `partner_external_id` |
| `partner_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `partner_id` |
| `phone` | VARCHAR2 | 100 |  |  |  |  | `purpose`: `phone` |
| `print_on_check_as` | VARCHAR2 | 83 |  |  |  |  | `purpose`: `print_on_check_as` |
| `shipping_address` | VARCHAR2 | 999 |  |  |  |  | `purpose`: `shipping_address` |
| `state` | VARCHAR2 | 50 |  |  |  |  | `purpose`: `state` |
| `subsidiary` | NUMBER |  | 39 | 0 | `subsidiaries`.`subsidiary_id` | foreign_key | `purpose`: `subsidiary` |
| `tax_number` | VARCHAR2 | 15 |  |  |  |  | `purpose`: `tax_identity_number` |
| `url` | VARCHAR2 | 100 |  |  |  |  | `purpose`: `url` |
| `zipcode` | VARCHAR2 | 36 |  |  |  |  | `purpose`: `zip_code` |

## Foreign keys

| Key | Columns | References table | Referenced columns |
|---|---|---|---|
| `partners_subsidiaries_fk` | `subsidiary` | `subsidiaries` | `subsidiary_id` |

## Referenced by

| Key | Table | Columns | Columns here |
|---|---|---|---|
| `Customers_partners_fk` | `customers` | `partner_id` | `partner_id` |
| `Opportunities_partners_fk` | `opportunities` | `partner_id` | `partner_id` |
| `Posting_account_activity_partners_fk` | `posting_account_activity` | `partner_id` | `partner_id` |
| `Posting_account_activity_pe_partners_fk` | `posting_account_activity_pe` | `partner_id` | `partner_id` |
