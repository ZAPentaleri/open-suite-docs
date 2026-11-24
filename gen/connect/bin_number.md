# Bin_number (`bin_number`)
<!-- id: connect:bin_number -->

| Fact | Value |
|---|---|
| Table | `bin_number` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `bin_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `bin_number_id` |
| `bin_number` | VARCHAR2 | 100 |  |  |  |  | `purpose`: `bin_number` |
| `bin_number_extid` | VARCHAR2 | 255 |  |  |  |  | `purpose`: `bin_number_external_id` |
| `isinactive` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `inactive` |
| `ispreferred` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `preferred` |
| `item_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `item_id` |
| `location_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `location_id` |
| `memo` | VARCHAR2 | 4000 |  |  |  |  | `purpose`: `memo` |
| `on_hand_count` | NUMBER |  | 39 | 0 |  |  | `purpose`: `on_hand_count` |
