# Vendor_subsidiary_map (`vendor_subsidiary_map`)
<!-- id: connect:vendor_subsidiary_map -->

| Fact | Value |
|---|---|
| Table | `vendor_subsidiary_map` |
| Primary key | `vendor_id`, `subsidiary_id` |
| Tags | `composite_primary_key` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `balance` | NUMBER |  | 39 | 0 |  |  | `purpose`: `vendor_payable_balance_due` |
| `balance_base` | NUMBER |  | 20 | 2 |  |  | `purpose`: `vendor_payable_balance_due`; `value_unit`: `base_currency` |
| `credit_limit` | NUMBER |  | 20 | 2 |  |  | `purpose`: `vendor_purchase_credit_limit`; `caveat`: `optional_value` |
| `subsidiary_id` | NUMBER |  | 39 | 0 | `subsidiaries`.`subsidiary_id` | primary_key | `purpose`: `customer_id`; `caveat`: `backward_compatibility_only`; `use_instead`: `customer_subsidiary_map_id`, `customer_subsidiary_map_extid`; `doc_inconsistency`: `names_column_customer_subsidary_map_id_misspelled` |
| `unbilled_orders` | NUMBER |  | 39 | 0 |  |  | `purpose`: `vendor_unbilled_orders_total` |
| `unbilled_orders_base` | NUMBER |  | 20 | 2 |  |  | `purpose`: `vendor_unbilled_orders_total`; `value_unit`: `base_currency` |
| `vendor_id` | NUMBER |  | 39 | 0 | `vendors`.`vendor_id` | primary_key | `purpose`: `customer_id`; `caveat`: `backward_compatibility_only`; `use_instead`: `customer_subsidiary_map_id`, `customer_subsidiary_map_extid`; `doc_inconsistency`: `names_column_customer_subsidary_map_id_misspelled` |
| `vendor_subsidiary_map_extid` | VARCHAR2 | 255 |  |  |  |  | `purpose`: `vendor_subsidiary_map_external_id`; `caveat`: `optional_value`; `value_source`: `set_by_customer` |
| `vendor_subsidiary_map_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `vendor_subsidiary_map_id`; `constraint`: `unique` |

## Foreign keys

| Key | Columns | References table | Referenced columns |
|---|---|---|---|
| `vendor_subsidiary_map_subsidiaries_fk` | `subsidiary_id` | `subsidiaries` | `subsidiary_id` |
| `vendor_subsidiary_map_vendors_fk` | `vendor_id` | `vendors` | `vendor_id` |

## Suspected source errors

> [!SOURCE-ERROR] `names_column_customer_subsidary_map_id_misspelled` (on `subsidiary_id`)
<!-- source-error: connect:vendor_subsidiary_map | subsidiary_id | names_column_customer_subsidary_map_id_misspelled -->

> [!SOURCE-ERROR] `names_column_customer_subsidary_map_id_misspelled` (on `vendor_id`)
<!-- source-error: connect:vendor_subsidiary_map | vendor_id | names_column_customer_subsidary_map_id_misspelled -->
