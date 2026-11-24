# States (`states`)
<!-- id: connect:states -->

| Fact | Value |
|---|---|
| Table | `states` |
| Primary key | `state_id` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `country_id` | NUMBER |  | 39 | 0 | `countries`.`country_id` | foreign_key | `purpose`: `country_id` |
| `name` | VARCHAR2 | 50 |  |  |  |  | `purpose`: `state_name` |
| `short_name` | VARCHAR2 | 36 |  |  |  |  | `purpose`: `state_short_name` |
| `state_id` | NUMBER |  | 38 | 0 |  | primary_key | `purpose`: `state_id` |

## Foreign keys

| Key | Columns | References table | Referenced columns |
|---|---|---|---|
| `states_countries_fk` | `country_id` | `countries` | `country_id` |
