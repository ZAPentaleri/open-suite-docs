# Time-Off Request (`timeoffrequest`)
<!-- id: record:timeoffrequest -->

| Fact | Value |
|---|---|
| Internal ID | `timeoffrequest` |
| Category | entity |
| Client scripts | no |
| Server scripts | yes |
| User event scripts | no |
| Tags | `no_copy`, `no_transform` |

## Record facts

- `script_types`: `server`
- `user_events_supported`: `false`
- `not_supported`: `transform`, `copy`
- `requires`: `employee_start_date_and_time_off_plan`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `employee` | text | None | no | no |  | `purpose`: `employee` |
| `enddate` | date | End Date | yes | no |  | `purpose`: `end_date`; `format`: `dd.mm.yyyy` |
| `message` | textarea | Message To Manager | no | no |  | `purpose`: `recipient_message` |
| `startdate` | date | Start Date | yes | no |  | `purpose`: `start_date`; `format`: `dd.mm.yyyy` |
| `approvalstatus` | select | Approval Status | yes | no |  |  |
| `creationdate` | datetimetz | Creation Date | no |  |  |  |
| `externalid` | text | ExternalId | no | no |  |  |
| `id` | integer | Internal ID | no |  |  |  |
| `internalid` | integer | Internal ID | no |  |  |  |
| `isautodeletetimeentry` | checkbox | None | no | no |  |  |

## Sublist `detail` (Details)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `amountoftime` | nonnegfloat | Amount of Time | yes |  |  |
| `timeoffdate` | date | Date | yes |  |  |
| `timeoffrequest` | select | Time-Off Request | no |  |  |
| `timeofftype` | select | Type | yes |  |  |
| `timeunit` | select | Time Unit | yes |  |  |
