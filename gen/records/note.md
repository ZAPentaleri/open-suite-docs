# Note (`note`)
<!-- id: record:note -->

| Fact | Value |
|---|---|
| Internal ID | `note` |
| Category | communication |
| Client scripts | no |
| Server scripts | yes |
| Searchable | yes |
| Tags | `custom_fields_supported`, `scripting_full` |

## Record facts

- `script_types`: `server`
- `script_functions`: `create`, `update`, `copy`, `delete`, `search`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `direction` | select | Direction | no | no |  | `purpose`: `note_direction`; `options`: `Incoming`, `Outgoing` |
| `note` | textarea | Memo | yes | no |  | `purpose`: `note_text`; `ui_effect`: `text_enhance_button_may_appear` |
| `notedate` | date | Date | no | no |  | `purpose`: `note_date` |
| `notetype` | select | Type | no | no |  | `purpose`: `note_type`; `allowed_values_listed_in`: `crm_list_note_type` |
| `time` | timeofday | Time | no | no |  | `purpose`: `note_time` |
| `title` | text | Title | no | no |  | `purpose`: `note_title` |
| `accountingperiod` | select | Accounting Period | no | no |  |  |
| `author` | select | Author | no | no |  |  |
| `entity` | select | Entity | no | no |  |  |
| `externalid` | text | ExternalId | no | no |  |  |
| `folder` | select | Folder | no | no |  |  |
| `item` | select | Item | no | no |  |  |
| `lastmodifieddate` | datetime | Modification Date | no | no |  |  |
| `media` | select | Media | no | no |  |  |
| `record` | select | Custom Record Key | no | no |  |  |
| `recordtype` | select | Custom Record Type | no | no |  |  |
| `topic` | select | Topic | no | no |  |  |
| `transaction` | select | Transaction | no | no |  |  |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `author` | select | Author |  |
| `direction` | checkbox | Direction |  |
| `externalid` | select | External ID |  |
| `externalidstring` | text | External ID (Text) |  |
| `formuladate` | date | Formula (Date) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulatext` | text | Formula (Text) |  |
| `internalid` | select | Internal ID |  |
| `internalidnumber` | integer | Internal ID (Number) |  |
| `note` | text | Memo |  |
| `notedate` | datetime | Date |  |
| `notetype` | select | Type |  |
| `title` | text | Title |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `author` | select | Author |  |
| `direction` | text | Direction |  |
| `externalid` | select | External ID |  |
| `formulacurrency` | currency | Formula (Currency) |  |
| `formuladate` | date | Formula (Date) |  |
| `formuladatetime` | datetime | Formula (Date/Time) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulapercent` | percent | Formula (Percent) |  |
| `formulatext` | text | Formula (Text) |  |
| `internalid` | select | Internal ID |  |
| `note` | text | Memo |  |
| `notedate` | datetime | Date |  |
| `notetype` | text | Type |  |
| `title` | text | Title |  |

## Search joins

| Join ID | Label | Joined record |
|---|---|---|
| `author` | Author | Employee |
| `campaign` | Campaign | Campaign |
| `contact` | Contact | Contact |
| `customer` | Customer | Customer |
| `employee` | Employee | Employee |
| `issue` | Issue | Issue |
| `originatinglead` | Originating Lead | Lead |
| `partner` | Partner | Partner |
| `solution` | Solution | Solution |
| `task` | Task | Task |
| `vendor` | Vendor | Vendor |
