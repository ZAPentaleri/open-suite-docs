# File (`file`)
<!-- id: record:file -->

| Fact | Value |
|---|---|
| Internal ID | `file` |
| Category | file_cabinet |
| Client scripts | no |
| Server scripts | yes |
| Searchable | yes |
| Tags | `custom_fields_supported`, `deleted_record_search_supported`, `search_only`, `supports_search` |

## Record facts

- `script_types`: `server`
- `script_functions`: `search`
- `own_api` (refs `N/file`)
- `client_supported`: `false`

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `addtimestamptourl` | checkbox | Generate URL Timestamp |  |
| `availablewithoutlogin` | checkbox | Available Without Login |  |
| `created` | datetime | Date Created |  |
| `dateviewed` | datetime | Date Viewed |  |
| `description` | text | Description |  |
| `documentsize` | integer | Size (KB) |  |
| `externalid` | select | External ID |  |
| `externalidstring` | text | External ID (Text) |  |
| `filetype` | select | File Type |  |
| `folder` | select | Folder |  |
| `internalid` | select | Internal ID |  |
| `internalidnumber` | integer | Internal ID (Number) |  |
| `isavailable` | checkbox | Available |  |
| `islink` | checkbox | External URL |  |
| `modified` | datetime | Last Modified |  |
| `name` | text | Name |  |
| `owner` | select | Owner |  |
| `url` | text | URL |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `availablewithoutlogin` | checkbox | Available Without Login |  |
| `created` | datetime | Date Created |  |
| `dateviewed` | datetime | Date Viewed |  |
| `description` | text | Description |  |
| `documentsize` | integer | Size (KB) |  |
| `externalid` | select | External ID |  |
| `filetype` | select | Type |  |
| `folder` | select | Folder |  |
| `hits` | integer | # Times Viewed |  |
| `hostedpath` | text | Hosted Path |  |
| `internalid` | select | Internal ID |  |
| `isavailable` | checkbox | Available |  |
| `lastviewed` | datetime | Last Viewed |  |
| `modified` | datetime | Last Modified |  |
| `name` | text | Name |  |
| `owner` | select | Owner |  |
| `url` | text | URL |  |

## Search joins

| Join ID | Label | Joined record |
|---|---|---|
| `shopper` | Shopper | Customer |
| `user` | User | Employee |
