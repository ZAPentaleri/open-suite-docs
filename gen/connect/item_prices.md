# Item_prices (`item_prices`)
<!-- id: connect:item_prices -->

| Fact | Value |
|---|---|
| Table | `item_prices` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `currency_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `currency_id` |
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `price_type_last_modified_date`; `time_zone`: `gmt` |
| `discount_pct` | NUMBER |  | 5 | 2 |  |  | `purpose`: `default_discount` |
| `isinactive` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `item_is_inactive` |
| `isonline` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `display_in_web_site` |
| `item_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `item_id` |
| `item_price_extid` | VARCHAR2 | 255 |  |  |  |  | `purpose`: `item_price_external_id` |
| `item_price_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `item_price_id` |
| `item_quantity_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `item_quantity_id` |
| `item_unit_price` | NUMBER |  | 30 | 15 |  |  | `purpose`: `item_unit_price` |
| `name` | VARCHAR2 | 60 |  |  |  |  | `purpose`: `name` |
