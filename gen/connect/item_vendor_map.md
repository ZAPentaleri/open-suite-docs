# Item_vendor_map (`item_vendor_map`)
<!-- id: connect:item_vendor_map -->

| Fact | Value |
|---|---|
| Table | `item_vendor_map` |
| Primary key | `item_id`, `vendor_id`, `subsidiary_id` |
| Tags | `composite_primary_key` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `is_preferred` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `whether_is_preferred` |
| `item_id` | NUMBER |  | 39 | 0 | `items`.`item_id` | primary_key | `purpose`: `item_id` |
| `subsidiary_id` | NUMBER |  | 39 | 0 | `subsidiaries`.`subsidiary_id` | primary_key | `purpose`: `subsidiary_id` |
| `vendor_code` | VARCHAR2 | 250 |  |  |  |  | `purpose`: `vendor_code` |
| `vendor_id` | NUMBER |  | 39 | 0 | `vendors`.`vendor_id` | primary_key | `purpose`: `vendor_id` |

## Foreign keys

| Key | Columns | References table | Referenced columns |
|---|---|---|---|
| `item_vendor_map_items_fk` | `item_id` | `items` | `item_id` |
| `item_vendor_map_subsidiaries_fk` | `subsidiary_id` | `subsidiaries` | `subsidiary_id` |
| `item_vendor_map_vendors_fk` | `vendor_id` | `vendors` | `vendor_id` |
