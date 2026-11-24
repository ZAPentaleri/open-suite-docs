# Expense Amortization Event (`expenseamortizationevent`)
<!-- id: record:expenseamortizationevent -->

| Fact | Value |
|---|---|
| Internal ID | `expenseamortizationevent` |

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `amount` | currency | Amount (Transaction Currency) | no | no |  |  |
| `cumulativepercentcomplete` | percent | Cumulative Percent Complete | no | no |  |  |
| `customform` | select | Custom Form | no | no |  |  |
| `eventdate` | date | Event Date | yes | no |  |  |
| `eventpurpose` | select | Event Purpose | yes | no |  |  |
| `eventtype` | select | Event Type | yes | no |  |  |
| `externalid` | text | ExternalId | no | no |  |  |
| `quantity` | float | Quantity | no | no |  |  |
| `transactionline` | select | Transaction Line | no | no |  |  |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `amount` | currency | Amount (Transaction Currency) |  |
| `cumulativepercentcomplete` | percent | Cumulative Percent Complete |  |
| `eventdate` | date | Event Date |  |
| `eventpurpose` | select | Event Purpose |  |
| `eventtype` | select | Event Type |  |
| `externalid` | select | External ID |  |
| `externalidstring` | text | External ID (Text) |  |
| `internalid` | select | Internal ID |  |
| `internalidnumber` | integer | Internal ID (Number) |  |
| `quantity` | float | Quantity |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `amount` | currency | Amount (Transaction Currency) |  |
| `cumulativepercentcomplete` | percent | Cumulative Percent Complete |  |
| `eventdate` | date | Event Date |  |
| `eventpurpose` | select | Event Purpose |  |
| `eventtype` | select | Event Type |  |
| `externalid` | select | External ID |  |
| `internalid` | select | Internal ID |  |
| `quantity` | float | Quantity |  |

## Search joins

| Join ID | Label | Joined record |
|---|---|---|
| `expenseamortizationeventtype` | Expense Amortization Event Type | Custom Recognition Event Type |
| `transaction` | Transaction | Transaction |
