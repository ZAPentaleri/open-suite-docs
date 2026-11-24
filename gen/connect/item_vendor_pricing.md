# Item_vendor_pricing (`item_vendor_pricing`)
<!-- id: connect:item_vendor_pricing -->

| Fact | Value |
|---|---|
| Table | `item_vendor_pricing` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `cost_0` | NUMBER |  | 30 | 15 |  |  | `purpose`: `vendor_pricing_cost` |
| `cost_is_percentage` | VARCHAR2 | 1 |  |  |  |  | `purpose`: `vendor_cost_is_percentage` |
| `currency_id` | NUMBER |  | 39 | 0 | `currencies`.`currency_id` | foreign_key | `purpose`: `vendor_pricing_currency` |
| `item_id` | NUMBER |  | 39 | 0 | `items`.`item_id` | foreign_key | `purpose`: `vendor_pricing_item` |
| `subsidiary_id` | NUMBER |  | 39 | 0 | `subsidiaries`.`subsidiary_id` | foreign_key | `purpose`: `vendor_pricing_subsidiary` |
| `vendor_id` | NUMBER |  | 39 | 0 | `vendors`.`vendor_id` | foreign_key | `purpose`: `vendor_pricing_vendor` |

## Foreign keys

| Key | Columns | References table | Referenced columns |
|---|---|---|---|
| `item_vendor_pricing_currencies_fk` | `currency_id` | `currencies` | `currency_id` |
| `item_vendor_pricing_items_fk` | `item_id` | `items` | `item_id` |
| `item_vendor_pricing_subsidiaries_fk` | `subsidiary_id` | `subsidiaries` | `subsidiary_id` |
| `item_vendor_pricing_vendors_fk` | `vendor_id` | `vendors` | `vendor_id` |
