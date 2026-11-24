# Customer_types (`customer_types`)
<!-- id: connect:customer_types -->

| Fact | Value |
|---|---|
| Table | `customer_types` |
| Primary key | `customer_type_id` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `customer_type_extid` | VARCHAR2 | 255 |  |  |  |  | `purpose`: `customer_type_external_id` |
| `customer_type_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `customer_type_id` |
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `date_last_modified` |
| `isinactive` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `item_is_inactive` |
| `name` | VARCHAR2 | 31 |  |  |  |  | `purpose`: `name` |
| `parent_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `child_of` |

## Referenced by

| Key | Table | Columns | Columns here |
|---|---|---|---|
| `Customers_customer_types_fk` | `customers` | `customer_type_id` | `customer_type_id` |
