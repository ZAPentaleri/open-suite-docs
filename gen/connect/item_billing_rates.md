# Item_billing_rates (`item_billing_rates`)
<!-- id: connect:item_billing_rates -->

| Fact | Value |
|---|---|
| Table | `item_billing_rates` |
| Primary key | `item_id`, `billing_class_id`, `item_price_id`, `currency_id` |
| Tags | `composite_primary_key` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `billing_class_id` | NUMBER |  | 39 | 0 | `billing_classes`.`billing_class_id` | primary_key | `purpose`: `billing_class_id` |
| `currency_id` | NUMBER |  | 39 | 0 | `currencies`.`currency_id` | primary_key | `purpose`: `currency_id` |
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `date_last_modified` |
| `discount_pct` | NUMBER |  | 5 | 2 |  |  | `purpose`: `discount_percentage` |
| `isinactive` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `is_inactive` |
| `isonline` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `is_online` |
| `item_id` | NUMBER |  | 39 | 0 | `service_items`.`item_id` | primary_key | `purpose`: `item_id` |
| `item_price_extid` | VARCHAR2 | 255 |  |  |  |  | `purpose`: `item_price_extension_id` |
| `item_price_id` | NUMBER |  | 39 | 0 | `price_types`.`price_type_id` | primary_key | `purpose`: `item_price_id` |
| `item_unit_price` | NUMBER |  | 30 | 15 |  |  | `purpose`: `item_unit_price` |
| `name` | VARCHAR2 | 60 |  |  |  |  | `purpose`: `name` |

## Foreign keys

| Key | Columns | References table | Referenced columns |
|---|---|---|---|
| `item_billing_rates_billing_classes_fk` | `billing_class_id` | `billing_classes` | `billing_class_id` |
| `item_billing_rates_currencies_fk` | `currency_id` | `currencies` | `currency_id` |
| `item_billing_rates_price_types_fk` | `item_price_id` | `price_types` | `price_type_id` |
| `item_billing_rates_service_items_fk` | `item_id` | `service_items` | `item_id` |
