# Events1 (`events1`)
<!-- id: connect:events1 -->

| Fact | Value |
|---|---|
| Table | `events1` |
| Record | `calendarevent` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `access_level` | VARCHAR2 | 10 |  |  |  |  | `purpose`: `access_level` |
| `company_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `company_id` |
| `contact_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `contact_id` |
| `date_0` | TIMESTAMP |  |  |  |  |  | `purpose`: `date` |
| `date_created` | TIMESTAMP |  |  |  |  |  | `purpose`: `date_created`; `time_zone`: `gmt` |
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `date_last_modified`; `time_zone`: `gmt` |
| `event_extid` | VARCHAR2 | 255 |  |  |  |  | `purpose`: `event_external_id` |
| `event_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `event_id` |
| `is_timed_event` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `is_timed_event` |
| `location_0` | VARCHAR2 | 200 |  |  |  |  | `purpose`: `location` |
| `message` | VARCHAR2 | 4000 |  |  |  |  | `purpose`: `message` |
| `opportunity_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `opportunity_id` |
| `organizer_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `organizer_id` |
| `status` | VARCHAR2 | 64 |  |  |  |  | `purpose`: `status` |
| `supportcase_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `support_case_id` |
| `title` | VARCHAR2 | 256 |  |  |  |  | `purpose`: `title` |
