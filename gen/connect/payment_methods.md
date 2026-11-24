# Payment_methods (`payment_methods`)
<!-- id: connect:payment_methods -->

| Fact | Value |
|---|---|
| Table | `payment_methods` |
| Record | `paymentmethod` |
| Primary key | `payment_method_id` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `date_last_modified` |
| `default_account_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `default_account_id` |
| `external_0` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `external` |
| `isinactive` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `item_is_inactive` |
| `name` | VARCHAR2 | 31 |  |  |  |  | `purpose`: `name` |
| `payment_method_extid` | VARCHAR2 | 255 |  |  |  |  | `purpose`: `payment_method_external_id` |
| `payment_method_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `payment_method_id` |
| `payment_method_tags` | VARCHAR2 | 256 |  |  |  |  | `purpose`: `payment_method_tags` |

## Referenced by

| Key | Table | Columns | Columns here |
|---|---|---|---|
| `Opportunity_lines_payment_methods_fk` | `opportunity_lines` | `payment_method_id` | `payment_method_id` |
