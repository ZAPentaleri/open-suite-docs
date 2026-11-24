# Customer_subsidiary_map (`customer_subsidiary_map`)
<!-- id: connect:customer_subsidiary_map -->

| Fact | Value |
|---|---|
| Table | `customer_subsidiary_map` |
| Primary key | `customer_id`, `subsidiary_id` |
| Tags | `composite_primary_key` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `balance` | NUMBER |  | 39 | 0 |  |  | `purpose`: `customer_receivable_balance_due`; `value_unit`: `customer_currency` (when `multiple_currencies`) |
| `customer_id` | NUMBER |  | 39 | 0 | `customers`.`customer_id` | primary_key | `purpose`: `customer_id`; `caveat`: `backward_compatibility_only`; `use_instead`: `customer_subsidiary_map_id`, `customer_subsidiary_map_extid`; `doc_inconsistency`: `names_column_customer_subsidary_map_id_misspelled` |
| `customer_subsidiary_map_extid` | VARCHAR2 | 255 |  |  |  |  | `purpose`: `customer_subsidiary_map_external_id`; `caveat`: `optional_value`; `value_source`: `set_by_customer` |
| `customer_subsidiary_map_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `customer_subsidiary_map_id`; `constraint`: `unique` |
| `deposit_balance` | NUMBER |  | 39 | 0 |  |  | `purpose`: `unapplied_deposits_total`; `value_unit`: `customer_currency` (when `multiple_currencies`) |
| `subsidiary_id` | NUMBER |  | 39 | 0 | `subsidiaries`.`subsidiary_id` | primary_key | `purpose`: `customer_id`; `caveat`: `backward_compatibility_only`; `use_instead`: `customer_subsidiary_map_id`, `customer_subsidiary_map_extid`; `doc_inconsistency`: `names_column_customer_subsidary_map_id_misspelled` |
| `unbilled_orders` | NUMBER |  | 39 | 0 |  |  | `purpose`: `unbilled_orders_total`; `value_unit`: `customer_currency` (when `multiple_currencies`) |

## Foreign keys

| Key | Columns | References table | Referenced columns |
|---|---|---|---|
| `customer_subsidiary_map_customers_fk` | `customer_id` | `customers` | `customer_id` |
| `customer_subsidiary_map_subsidiaries_fk` | `subsidiary_id` | `subsidiaries` | `subsidiary_id` |

## Suspected source errors

> [!SOURCE-ERROR] `names_column_customer_subsidary_map_id_misspelled` (on `customer_id`)
<!-- source-error: connect:customer_subsidiary_map | customer_id | names_column_customer_subsidary_map_id_misspelled -->

> [!SOURCE-ERROR] `names_column_customer_subsidary_map_id_misspelled` (on `subsidiary_id`)
<!-- source-error: connect:customer_subsidiary_map | subsidiary_id | names_column_customer_subsidary_map_id_misspelled -->
