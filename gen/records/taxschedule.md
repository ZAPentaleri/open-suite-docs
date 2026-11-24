# Tax Schedule (`taxschedule`)
<!-- id: record:taxschedule -->

| Fact | Value |
|---|---|
| Internal ID | `taxschedule` |
| Tags | `custom_fields_supported` |

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `description` | text | Description | yes | no |  |  |
| `externalid` | text | ExternalId | no | no |  |  |
| `id` | integer | Internal ID | no |  | marked_new |  |
| `isinactive` | checkbox | Inactive | no |  | marked_new |  |
| `name` | text | Product | yes | no |  |  |
| `pagehelpertext` | help |  | no |  | marked_new |  |

## Sublist `nexuses`

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `nexus` | select | Nexus | no | marked_new |  |
| `nexuscountry` | select | Country | no | marked_new |  |
| `purchasetaxcode` | select | Purchase Tax Code | no | marked_new |  |
| `salestaxcode` | select | Sales Tax Code | no | marked_new |  |
| `taxcodeinclude` | text | Tax Code Lists Include | no | marked_new |  |
| `taxschedule` | select | Tax Schedule | no | marked_new |  |

## Sublist `usnexuses` (United States)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `nexus` | select | Nexus | no | marked_new |  |
| `taxable` | checkbox | Taxable | no | marked_new |  |
| `taxschedule` | select | Tax Schedule | no | marked_new |  |

## Tabs

| Tab ID | Label |
|---|---|
| `non_us` | Non US Nexuses |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `description` | text | Description |  |
| `externalid` | select | External ID |  |
| `externalidstring` | text | External ID (Text) |  |
| `formuladate` | date | Formula (Date) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulatext` | text | Formula (Text) |  |
| `internalid` | select | Internal ID |  |
| `internalidnumber` | integer | Internal ID (Number) |  |
| `name` | text | Name |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `description` | select | Description |  |
| `externalid` | select | External ID |  |
| `formulacurrency` | currency | Formula (Currency) |  |
| `formuladate` | date | Formula (Date) |  |
| `formuladatetime` | datetime | Formula (Date/Time) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulapercent` | percent | Formula (Percent) |  |
| `formulatext` | text | Formula (Text) |  |
| `internalid` | select | Internal ID |  |
| `name` | text | Name |  |

## Search joins

| Join ID | Label | Joined record |
|---|---|---|
| `user` | User | Employee |
