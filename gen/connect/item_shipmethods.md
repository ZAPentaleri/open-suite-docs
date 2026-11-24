# Item_shipmethods (`item_shipmethods`)
<!-- id: connect:item_shipmethods -->

| Fact | Value |
|---|---|
| Table | `item_shipmethods` |
| Primary key | `shipmethod_id`, `item_id` |
| Tags | `composite_primary_key` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `is_default` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `is_default` |
| `item_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `item_id` |
| `shipmethod_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `ship_method_id` |
