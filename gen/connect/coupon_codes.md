# Coupon_codes (`coupon_codes`)
<!-- id: connect:coupon_codes -->

| Fact | Value |
|---|---|
| Table | `coupon_codes` |
| Record | `couponcode` |
| Primary key | `coupon_code_id` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `code` | VARCHAR2 | 120 |  |  |  |  | `purpose`: `coupon_code` |
| `coupon_code_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `coupon_code_id` |
| `date_sent` | TIMESTAMP |  |  |  |  |  | `purpose`: `coupon_code_sent_date` |
| `promotion_code_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `promotion_id` |
| `recipient_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `coupon_code_recipient` |

## Referenced by

| Key | Table | Columns | Columns here |
|---|---|---|---|
| `Opportunities_coupon_codes_fk` | `opportunities` | `promotion_code_instance_id` | `coupon_code_id` |
