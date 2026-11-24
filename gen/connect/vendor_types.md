# Vendor_types (`vendor_types`)
<!-- id: connect:vendor_types -->

| Fact | Value |
|---|---|
| Table | `vendor_types` |
| Primary key | `vendor_type_id` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `isinactive` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `item_is_inactive` |
| `name` | VARCHAR2 | 31 |  |  |  |  | `purpose`: `name` |
| `parent_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `child_of` |
| `vendor_type_extid` | VARCHAR2 | 255 |  |  |  |  | `purpose`: `vendor_type_external_id` |
| `vendor_type_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `vendor_type_id` |

## Referenced by

| Key | Table | Columns | Columns here |
|---|---|---|---|
| `Vendors_vendor_types_fk` | `vendors` | `vendor_type_id` | `vendor_type_id` |
