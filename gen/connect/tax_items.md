# Tax_items (`tax_items`)
<!-- id: connect:tax_items -->

| Fact | Value |
|---|---|
| Table | `tax_items` |
| Primary key | `item_id` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `date_last_modified`; `time_zone`: `gmt` |
| `description` | VARCHAR2 | 4000 |  |  |  |  | `purpose`: `description` |
| `full_name` | VARCHAR2 | 550 |  |  |  |  | `purpose`: `tax_code_full_name`; `available_only_when`: `suitetax_disabled` |
| `income_account_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `tax_code_sales_tax_account`; `available_only_when`: `suitetax_disabled` |
| `isinactive` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `tax_item_is_inactive` |
| `item_extid` | VARCHAR2 | 255 |  |  |  |  | `purpose`: `item_external_id` |
| `item_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `item_id` |
| `name` | VARCHAR2 | 250 |  |  |  |  | `purpose`: `tax_code_name`; `requires_feature`: `SuiteTax` [`real_name`] |
| `parent_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `parent_tax_item`; `available_only_when`: `suitetax_disabled` |
| `rate` | VARCHAR2 | 42 |  |  |  |  | `purpose`: `tax_rate`; `available_only_when`: `suitetax_disabled` |
| `tax_city` | VARCHAR2 | 4000 |  |  |  |  | `purpose`: `tax_city`; `available_only_when`: `suitetax_disabled` |
| `tax_county` | VARCHAR2 | 41 |  |  |  |  | `purpose`: `tax_county`; `available_only_when`: `suitetax_disabled` |
| `tax_state` | VARCHAR2 | 2 |  |  |  |  | `purpose`: `tax_state`; `available_only_when`: `suitetax_disabled` |
| `tax_type_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `tax_type` |
| `tax_zipcode` | VARCHAR2 | 4000 |  |  |  |  | `purpose`: `tax_postal_code`; `available_only_when`: `suitetax_disabled` |
| `vendor_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `unsupported_column`; `not_supported`: `column_not_supported` |
| `vendorname` | VARCHAR2 | 250 |  |  |  |  | `purpose`: `unsupported_column`; `not_supported`: `column_not_supported` |

## Referenced by

| Key | Table | Columns | Columns here |
|---|---|---|---|
| `Transaction_tax_detail_tax_items_fk` | `transaction_tax_detail` | `tax_item_id` | `item_id` |
