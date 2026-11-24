# Eventattendees (`eventattendees`)
<!-- id: connect:eventattendees -->

| Fact | Value |
|---|---|
| Table | `eventattendees` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `attendee_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `attendee_id` |
| `event_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `event_id` |
| `status` | VARCHAR2 | 12 |  |  |  |  | `purpose`: `status` |
