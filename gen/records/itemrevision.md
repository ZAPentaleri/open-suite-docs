# Item Revision (`itemrevision`)
<!-- id: record:itemrevision -->

| Fact | Value |
|---|---|
| Internal ID | `itemrevision` |
| Category | list |
| Client scripts | no |
| Server scripts | yes |
| Searchable | yes |
| Tags | `custom_fields_supported`, `scripting_full` |

## Record facts

- `script_types`: `server`
- `client_supported`: `false`
- `script_functions`: `create`, `update`, `copy`, `delete`, `search`

## Body fields

| Field ID | Type | Label | Required | submitField | Read-only | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `effectivedate` | date | Effective Date | yes | no |  |  | `purpose`: `revision_effective_date` |
| `inactive` | checkbox | Inactive | no | no |  |  | `purpose`: `inactivate_revision`; `ui_effect`: `hidden_from_lists` |
| `item` | select | Item | yes | no |  |  | `purpose`: `revision_item` |
| `memo` | textarea | Memo | no | no |  |  | `purpose`: `memo`; `supported_in`: `search` |
| `name` | text | Name | yes | no |  |  | `purpose`: `revision_name`; `value_examples`: `Version One` |
| `obsoletedate` | date | Obsolete Date | no | no | yes |  | `purpose`: `obsolete_date`; `value_source`: `system_avoids_gaps_and_overlaps` |
| `externalid` | text | ExternalId | no | no |  |  |  |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `effectivedate` | date | Effective Date |  |
| `externalid` | select | External ID |  |
| `externalidstring` | text | External ID (Text) |  |
| `formuladate` | date | Formula (Date) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulatext` | text | Formula (Text) |  |
| `internalid` | select | Internal ID |  |
| `internalidnumber` | integer | Internal ID (Number) |  |
| `item` | select | Item |  |
| `name` | text | Name |  |
| `obsoletedate` | date | Obsolete Date |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `effectivedate` | date | Effective Date |  |
| `externalid` | select | External ID |  |
| `formulacurrency` | currency | Formula (Currency) |  |
| `formuladate` | date | Formula (Date) |  |
| `formuladatetime` | datetime | Formula (Date/Time) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulapercent` | percent | Formula (Percent) |  |
| `formulatext` | text | Formula (Text) |  |
| `internalid` | select | Internal ID |  |
| `isinactive` | checkbox | inactive |  |
| `item` | select | Item |  |
| `memo` | text | Memo |  |
| `name` | text | Name |  |
| `obsoletedate` | date | Obsolete Date |  |
