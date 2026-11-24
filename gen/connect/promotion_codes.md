# Promotion_codes (`promotion_codes`)
<!-- id: connect:promotion_codes -->

| Fact | Value |
|---|---|
| Table | `promotion_codes` |
| Record | `promotioncode` |
| Primary key | `promotion_code_id` |
| Domains | Campaignevents |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `apply_discount` | VARCHAR2 | 20 |  |  |  |  | `purpose`: `promotion_discount_applies_to`; `options`: `first_sale`, `all_sales` |
| `code` | VARCHAR2 | 120 |  |  |  |  | `purpose`: `name_or_code` |
| `code_pattern` | VARCHAR2 | 120 |  |  |  |  | `purpose`: `code_pattern` |
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `date_last_modified`; `time_zone`: `gmt` |
| `descr` | VARCHAR2 | 199 |  |  |  |  | `purpose`: `description` |
| `discount` | VARCHAR2 | 42 |  |  |  |  | `purpose`: `discount_item_name` |
| `discount_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `discount_item_id` |
| `display_line_discounts` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `display_line_discounts` |
| `end_date` | TIMESTAMP |  |  |  |  |  | `purpose`: `end_date` |
| `name` | VARCHAR2 | 120 |  |  |  |  | `purpose`: `promotion_name` |
| `plugin_implementation_id` | VARCHAR2 | 200 |  |  |  |  | `purpose`: `plugin_implementation_id` |
| `promotion_code_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `promotion_code_id` |
| `use_type` | VARCHAR2 | 12 |  |  |  |  | `purpose`: `use_type` |

## Referenced by

| Key | Table | Columns | Columns here |
|---|---|---|---|
| `Campaignevent_promotion_codes_fk` | `campaignevent` | `promotion_code_id` | `promotion_code_id` |
| `Posting_account_activity_pe_promotion_codes_fk` | `posting_account_activity_pe` | `promotion_code_id` | `promotion_code_id` |
| `Posting_account_activity_promotion_codes_fk` | `posting_account_activity` | `promotion_code_id` | `promotion_code_id` |
