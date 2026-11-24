# Payment_terms (`payment_terms`)
<!-- id: connect:payment_terms -->

| Fact | Value |
|---|---|
| Table | `payment_terms` |
| Primary key | `payment_terms_id` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `date_driven` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `date_driven` |
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `date_last_modified` |
| `days_till_1st_payment` | NUMBER |  | 39 | 0 |  |  | `purpose`: `days_until_first_payment` |
| `days_until_due` | NUMBER |  | 39 | 0 |  |  | `purpose`: `days_until_net_due` |
| `discount_days` | NUMBER |  | 39 | 0 |  |  | `purpose`: `days_until_discount_expires` |
| `frequency_id` | VARCHAR2 | 20 |  |  |  |  | `purpose`: `frequency_id` |
| `installment_count` | NUMBER |  | 39 | 0 |  |  | `purpose`: `installment_count` |
| `is_installment` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `is_installment` |
| `is_pay_tax_upfront` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `tax_paid_with_first_installment` |
| `is_preferred` | VARCHAR2 | 1 |  |  |  |  | `purpose`: `preferred` |
| `is_split_evenly` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `installments_split_evenly` |
| `isinactive` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `item_is_inactive` |
| `minimum_days` | NUMBER |  | 39 | 0 |  |  | `purpose`: `minimum_days` |
| `name` | VARCHAR2 | 31 |  |  |  |  | `purpose`: `name` |
| `payment_terms_extid` | VARCHAR2 | 255 |  |  |  |  | `purpose`: `payment_terms_external_id` |
| `payment_terms_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `payment_terms_id` |
| `percentage_discount` | NUMBER |  | 10 | 5 |  |  | `purpose`: `percentage_discount` |
| `repeat_every` | NUMBER |  | 39 | 0 |  |  | `purpose`: `repeat_every` |

## Referenced by

| Key | Table | Columns | Columns here |
|---|---|---|---|
| `Customers_payment_terms_fk` | `customers` | `payment_terms_id` | `payment_terms_id` |
| `Opportunities_payment_terms_fk` | `opportunities` | `payment_terms_id` | `payment_terms_id` |
