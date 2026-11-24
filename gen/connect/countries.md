# Countries (`countries`)
<!-- id: connect:countries -->

| Fact | Value |
|---|---|
| Table | `countries` |
| Primary key | `country_id` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `country_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `country` |
| `name` | VARCHAR2 | 50 |  |  |  |  | `purpose`: `name_of_country` |
| `short_name` | VARCHAR2 | 2 |  |  |  |  | `purpose`: `short_name_of_country` |

## Referenced by

| Key | Table | Columns | Columns here |
|---|---|---|---|
| `States_countries_fk` | `states` | `country_id` | `country_id` |
