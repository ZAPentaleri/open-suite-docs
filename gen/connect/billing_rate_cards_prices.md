# Billing_rate_cards_prices (`billing_rate_cards_prices`)
<!-- id: connect:billing_rate_cards_prices -->

| Fact | Value |
|---|---|
| Table | `billing_rate_cards_prices` |
| Primary key | `billing_rate_card_id`, `billing_class_id`, `currency_id`, `version_0` |
| Tags | `composite_primary_key` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `billing_class_id` | NUMBER |  | 39 | 0 | `billing_classes`.`billing_class_id` | primary_key | `purpose`: `billing_class_id` |
| `billing_rate_card_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `billing_rate_card_id` |
| `currency_id` | NUMBER |  | 39 | 0 | `currencies`.`currency_id` | primary_key | `purpose`: `currency_id` |
| `sale_unit_id` | NUMBER |  | 39 | 0 | `uom`.`uom_id` | foreign_key | `purpose`: `sale_unit_id` |
| `service_item_id` | NUMBER |  | 39 | 0 | `items`.`item_id` | foreign_key | `purpose`: `service_item_id` |
| `unit_price` | NUMBER |  | 39 | 0 |  |  | `purpose`: `unit_price` |
| `units_type_id` | NUMBER |  | 39 | 0 | `units_type`.`units_type_id` | foreign_key | `purpose`: `units_type_id` |
| `version_0` | NUMBER |  | 39 | 0 | `billing_rate_cards_versions`.`version_0` | primary_key | `purpose`: `version_0` |

## Foreign keys

| Key | Columns | References table | Referenced columns |
|---|---|---|---|
| `billing_rate_cards_prices_billing_classes_fk` | `billing_class_id` | `billing_classes` | `billing_class_id` |
| `billing_rate_cards_prices_billing_rate_cards_fk` | `billing_rate_card_id` | `billing_rate_cards` | `billing_rate_card_id` |
| `billing_rate_cards_prices_billing_rate_cards_versions_fk` | `billing_rate_card_id`, `version_0` | `billing_rate_cards_versions` | `billing_rate_card_id`, `version_0` |
| `billing_rate_cards_prices_currencies_fk` | `currency_id` | `currencies` | `currency_id` |
| `billing_rate_cards_prices_items_fk` | `service_item_id` | `items` | `item_id` |
| `billing_rate_cards_prices_units_type_fk` | `units_type_id` | `units_type` | `units_type_id` |
| `billing_rate_cards_prices_uom_fk` | `sale_unit_id` | `uom` | `uom_id` |
