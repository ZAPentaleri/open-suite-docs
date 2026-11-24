# Pricing_groups (`pricing_groups`)
<!-- id: connect:pricing_groups -->

| Fact | Value |
|---|---|
| Table | `pricing_groups` |
| Record | `pricinggroup` |
| Primary key | `pricing_group_id` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `name` | VARCHAR2 | 60 |  |  |  |  | `purpose`: `name` |
| `pricing_group_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `pricing_group_id` |
