# Contacts (`contacts`)
<!-- id: connect:contacts -->

| Fact | Value |
|---|---|
| Table | `contacts` |
| Record | `contact` |
| Primary key | `contact_id` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `address` | VARCHAR2 | 999 |  |  |  |  | `purpose`: `address` |
| `altemail` | VARCHAR2 | 254 |  |  |  |  | `purpose`: `alternate_email` |
| `assistant_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `assistant` |
| `assistantphone` | VARCHAR2 | 100 |  |  |  |  | `purpose`: `assistant_phone` |
| `city` | VARCHAR2 | 50 |  |  |  |  | `purpose`: `city` |
| `comments` | VARCHAR2 | 4000 |  |  |  |  | `purpose`: `comments` |
| `company_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `company_id` |
| `contact_extid` | VARCHAR2 | 255 |  |  |  |  | `purpose`: `contact_external_id` |
| `contact_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `contact_id` |
| `country` | VARCHAR2 | 50 |  |  |  |  | `purpose`: `country` |
| `create_date` | TIMESTAMP |  |  |  |  |  | `purpose`: `create_date`; `time_zone`: `gmt` |
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `date_last_modified` |
| `email` | VARCHAR2 | 254 |  |  |  |  | `purpose`: `email` |
| `fax` | VARCHAR2 | 100 |  |  |  |  | `purpose`: `fax` |
| `firstname` | VARCHAR2 | 32 |  |  |  |  | `purpose`: `first_name` |
| `full_name` | VARCHAR2 | 1800 |  |  |  |  | `purpose`: `full_name` |
| `homephone` | VARCHAR2 | 100 |  |  |  |  | `purpose`: `home_phone` |
| `is_private` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `private` |
| `isemailhtml` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `email_as_html` |
| `isemailpdf` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `email_as_pdf` |
| `isinactive` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `contact_is_inactive` |
| `last_modified_date` | TIMESTAMP |  |  |  |  |  | `purpose`: `date_last_modified` |
| `lastname` | VARCHAR2 | 32 |  |  |  |  | `purpose`: `last_name` |
| `line1` | VARCHAR2 | 150 |  |  |  |  | `purpose`: `address_line_1` |
| `line2` | VARCHAR2 | 150 |  |  |  |  | `purpose`: `address_line_2` |
| `line3` | VARCHAR2 | 150 |  |  |  |  | `purpose`: `address_line_3` |
| `loginaccess` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `contact_has_login_access` |
| `middlename` | VARCHAR2 | 32 |  |  |  |  | `purpose`: `middle_name` |
| `mobilephone` | VARCHAR2 | 100 |  |  |  |  | `purpose`: `mobile_phone` |
| `name` | VARCHAR2 | 83 |  |  |  |  | `purpose`: `name` |
| `officephone` | VARCHAR2 | 100 |  |  |  |  | `purpose`: `office_phone` |
| `phone` | VARCHAR2 | 100 |  |  |  |  | `purpose`: `phone` |
| `salutation` | VARCHAR2 | 30 |  |  |  |  | `purpose`: `salutation` |
| `state` | VARCHAR2 | 50 |  |  |  |  | `purpose`: `state` |
| `subsidiary` | NUMBER |  | 39 | 0 |  |  | `purpose`: `subsidiary` |
| `supervisior_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `supervisor_id` |
| `supervisorphone` | VARCHAR2 | 100 |  |  |  |  | `purpose`: `supervisor_phone` |
| `title` | VARCHAR2 | 99 |  |  |  |  | `purpose`: `job_title` |
| `zipcode` | VARCHAR2 | 36 |  |  |  |  | `purpose`: `zip_code` |

## Referenced by

| Key | Table | Columns | Columns here |
|---|---|---|---|
| `Campaignresponse_contacts_fk` | `campaignresponse` | `contact_id` | `contact_id` |
| `Customers_contacts_fk` | `customers` | `primary_contact_id` | `contact_id` |
| `Customers_contacts_fk_2` | `customers` | `alternate_contact_id` | `contact_id` |
| `Entity_role_map_contacts_fk` | `entity_role_map` | `contact_id` | `contact_id` |
| `Support_incidents_contacts_fk` | `support_incidents` | `contact_id` | `contact_id` |
