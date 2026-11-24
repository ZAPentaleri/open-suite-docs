# Gift_certificates (`gift_certificates`)
<!-- id: connect:gift_certificates -->

| Fact | Value |
|---|---|
| Table | `gift_certificates` |
| Record | `giftcertificate` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `amount` | NUMBER |  | 39 | 0 |  |  | `purpose`: `original_amount` |
| `amount_available` | NUMBER |  | 39 | 0 |  |  | `purpose`: `amount_remaining` |
| `amount_available_billed` | NUMBER |  | 39 | 0 |  |  | `purpose`: `amount_billed` |
| `create_date` | TIMESTAMP |  |  |  |  |  | `purpose`: `date_created` |
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `date_last_modified` |
| `expiration_date` | TIMESTAMP |  |  |  |  |  | `purpose`: `expiration_date` |
| `from_name` | VARCHAR2 | 600 |  |  |  |  | `purpose`: `from` |
| `gift_certificate_code` | VARCHAR2 | 50 |  |  |  |  | `purpose`: `code` |
| `gift_certificate_id` | VARCHAR2 | 50 |  |  |  |  | `purpose`: `gift_certificate_id` |
| `gift_message` | VARCHAR2 | 4000 |  |  |  |  | `purpose`: `gift_message` |
| `include_child_subsidiaries` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `include_child_subsidiaries` |
| `income_account_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `income_account` |
| `is_active` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `active` |
| `item_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `item_name_number` |
| `item_name` | VARCHAR2 | 250 |  |  |  |  | `purpose`: `display_name_code` |
| `liability_account_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `liability_account` |
| `purchase_date` | TIMESTAMP |  |  |  |  |  | `purpose`: `purchase_date` |
| `to_email` | VARCHAR2 | 300 |  |  |  |  | `purpose`: `recipient_email` |
| `to_name` | VARCHAR2 | 600 |  |  |  |  | `purpose`: `recipient_name` |
