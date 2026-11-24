# Sales_channels (`sales_channels`)
<!-- id: connect:sales_channels -->

| Fact | Value |
|---|---|
| Table | `sales_channels` |
| Record | `saleschannel` |
| Primary key | `sales_channel_id` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `date_created` | TIMESTAMP |  |  |  |  |  | `purpose`: `date_created` |
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `date_last_modified` |
| `description` | VARCHAR2 | 2000 |  |  |  |  | `purpose`: `description` |
| `is_inactive` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `is_inactive` |
| `name` | VARCHAR2 | 400 |  |  |  |  | `purpose`: `name` |
| `order_priority` | NUMBER |  | 39 | 0 |  |  | `purpose`: `order_priority` |
| `sales_channel_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `sales_channel_id` |
| `use_reservations_as_supply` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `only_dedicated_reservations_as_supply` |

## Referenced by

| Key | Table | Columns | Columns here |
|---|---|---|---|
| `Transactions_sales_channels_fk` | `transactions` | `sales_channel_id` | `sales_channel_id` |
