# Entity Account Mapping (`entityaccountmapping`)
<!-- id: record:entityaccountmapping -->

| Fact | Value |
|---|---|
| Internal ID | `entityaccountmapping` |
| Tags | `custom_fields_supported` |

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `accountingbook` | select | Accounting Book | yes | no |  |  |
| `class` | select | Class | no | no |  |  |
| `customform` | select | Custom Form | no | no |  |  |
| `department` | select | Department | no | no |  |  |
| `destinationaccount` | select | Destination Account | yes | no |  |  |
| `effectivedate` | date | Effective Date | yes | no |  |  |
| `enddate` | date | End Date | no | no |  |  |
| `entityaccount` | select | Entity Account | yes | no |  |  |
| `externalid` | text | ExternalId | no | no |  |  |
| `location` | select | Location | no | no |  |  |
| `sourceaccount` | select | Source Account | no | no |  |  |
| `subsidiary` | select | Subsidiary | no | no |  |  |

## Tabs

| Tab ID | Label |
|---|---|
| `workflow` | Workflow |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `accountingbook` | select | Accounting Book |  |
| `class` | select | Class |  |
| `customdimension` | select | Custom Dimension |  |
| `department` | select | Department |  |
| `destinationaccount` | select | Destination Account |  |
| `effectivedate` | date | Effective Date |  |
| `enddate` | date | End Date |  |
| `entityaccount` | select | Entity Account |  |
| `externalid` | select | External ID |  |
| `externalidstring` | text | External ID (Text) |  |
| `formuladate` | date | Formula (Date) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulatext` | text | Formula (Text) |  |
| `internalid` | select | Internal ID |  |
| `internalidnumber` | integer | Internal ID (Number) |  |
| `location` | select | Location |  |
| `sourceaccount` | select | Source Account |  |
| `subsidiary` | select | Subsidiary |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `accountingbook` | select | Accounting Book |  |
| `class` | select | Class |  |
| `customdimension` | select | Custom Dimension |  |
| `department` | select | Department |  |
| `destinationaccount` | select | Destination Account |  |
| `effectivedate` | date | Effective Date |  |
| `enddate` | date | End Date |  |
| `entityaccount` | select | Entity Account |  |
| `externalid` | select | External ID |  |
| `formulacurrency` | currency | Formula (Currency) |  |
| `formuladate` | date | Formula (Date) |  |
| `formuladatetime` | datetime | Formula (Date/Time) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulapercent` | percent | Formula (Percent) |  |
| `formulatext` | text | Formula (Text) |  |
| `internalid` | select | Internal ID |  |
| `location` | select | Location |  |
| `sourceaccount` | select | Source Account |  |
| `subsidiary` | select | Subsidiary |  |

## Search joins

| Join ID | Label | Joined record |
|---|---|---|
| `accountingbook` | Accounting Book | Accounting Book |
| `department` | Department | Department |
| `destinationaccount` | Destination Account | Account |
| `location` | Location | Location |
| `sourceaccount` | Source Account | Account |
| `subsidiary` | Subsidiary | Subsidiary |
