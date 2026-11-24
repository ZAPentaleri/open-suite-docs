# Unlock Time Period (`unlockedtimeperiod`)
<!-- id: record:unlockedtimeperiod -->

| Fact | Value |
|---|---|
| Internal ID | `unlockedtimeperiod` |
| Category | transaction |
| Client scripts | yes |
| Server scripts | yes |
| Searchable | yes |
| Tags | `supports_copy`, `supports_create`, `supports_delete`, `supports_read`, `supports_search`, `supports_update` |

## Record facts

- `script_types`: `client`, `server`
- `not_supported`: `transform`
- `script_functions`: `create`, `read`, `update`, `delete`, `copy`, `search`
- `requires_feature`: `Weekly Timesheets` [`real_name`]
- `requires_preference`: `lock_timesheet_period`
- `script_functions`: `create`, `read`, `update`, `delete`, `copy`, `search`
- `not_supported`: `transform`
- `avoid_when`: `overlapping_active_period_same_employee`
- `search_only_sublists`: `systemnotes`
- `can`: `read_via_connect_unlocked_time_periods_table`
- `purpose`: `unlock_locked_timesheet_period_for_employee`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `enddate` | date | End Date | yes |  |  |  |
| `entity` | select | Employee | yes |  |  |  |
| `externalid` | text | External ID | no |  |  |  |
| `id` | integer | Internal ID | no |  |  |  |
| `internalid` | integer | Internal ID | no |  |  |  |
| `isinactive` | checkbox | Inactive | no |  |  |  |
| `startdate` | date | Start Date | yes |  |  |  |
| `validuntil` | date | Valid Until | yes |  |  |  |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `enddate` | date | End Date |  |
| `entity` | select | Employee |  |
| `externalid` | select | External ID |  |
| `externalidstring` | text | External ID (Text) |  |
| `internalid` | select | Internal ID |  |
| `internalidnumber` | integer | Internal ID (Number) |  |
| `isinactive` | checkbox | Inactive |  |
| `startdate` | date | Start Date |  |
| `validuntil` | date | Valid Until |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `enddate` | date | End Date |  |
| `entity` | select | Employee |  |
| `externalid` | select | External ID |  |
| `internalid` | select | Internal ID |  |
| `isinactive` | checkbox | Inactive |  |
| `startdate` | date | Start Date |  |
| `validuntil` | date | Valid Until |  |

## Search joins

| Join ID | Label | Joined record |
|---|---|---|
| `employee` | Employee | Employee |
| `vendor` | Vendor | Vendor |
