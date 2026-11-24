# Activity (`activity`)
<!-- id: record:activity -->

| Fact | Value |
|---|---|
| Internal ID | `activity` |
| Category | activity |
| Client scripts | yes |
| Server scripts | yes |
| Searchable | yes |
| Tags | `custom_fields_supported`, `deleted_record_search_supported`, `search_only`, `supports_search` |

## Record facts

- `script_types`: `client`, `server`
- `script_functions`: `search`

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `assigned` | select | Assigned To |  |
| `company` | select | Company |  |
| `completeddate` | date | Date Completed |  |
| `contact` | select | Contact |  |
| `date` | date | Date |  |
| `externalid` | select | External ID |  |
| `externalidstring` | text | External ID (Text) |  |
| `formuladate` | date | Formula (Date) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulatext` | text | Formula (Text) |  |
| `internalid` | select | Internal ID |  |
| `internalidnumber` | integer | Internal ID (Number) |  |
| `memo` | textarea | Notes |  |
| `owner` | select | Created By |  |
| `priority` | select | Priority |  |
| `status` | select | Status |  |
| `title` | text | Title |  |
| `type` | select | Activity Type |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `assigned` | select | Assigned To |  |
| `company` | text | Company |  |
| `completeddate` | date | Date Completed |  |
| `contact` | select | Contact |  |
| `createddate` | date | Date Created |  |
| `externalid` | select | External ID |  |
| `formulacurrency` | currency | Formula (Currency) |  |
| `formuladate` | date | Formula (Date) |  |
| `formuladatetime` | datetime | Formula (Date/Time) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulapercent` | percent | Formula (Percent) |  |
| `formulatext` | text | Formula (Text) |  |
| `internalid` | select | Internal ID |  |
| `lastmodifieddate` | date | Last Modified |  |
| `markdone` | text | Mark |  |
| `message` | textarea | Notes |  |
| `owner` | select | Owner |  |
| `priority` | select | Priority |  |
| `startdate` | date | Date |  |
| `starttime` | timeofday | Time |  |
| `status` | text | Status |  |
| `title` | text | Title |  |
| `transaction` | select | Transaction |  |
| `type` | text | Type |  |

## Search joins

| Join ID | Label | Joined record |
|---|---|---|
| `contact` | Contact | Contact |
| `originatinglead` | Originating Lead | Lead |
| `user` | User | Employee |
