# Deleted Record (`deletedrecord`)
<!-- id: record:deletedrecord -->

| Fact | Value |
|---|---|
| Internal ID | `deletedrecord` |
| Tags | `custom_fields_supported`, `search_only` |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `context` | select | Context |  |
| `deletedby` | select | Deleted by |  |
| `deleteddate` | datetime | Date Deleted |  |
| `name` | text | Name |  |
| `recordtype` | select | Record Type |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `context` | text | Context |  |
| `deletedby` | select | Deleted By |  |
| `deleteddate` | datetime | Date Deleted |  |
| `externalid` | text | External Id |  |
| `name` | text | Name |  |

## Search joins

| Join ID | Label | Joined record |
|---|---|---|
| `user` | User | Employee |
