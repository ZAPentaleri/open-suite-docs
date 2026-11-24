# Sales Price Rule (`salespricerule`)
<!-- id: record:salespricerule -->

| Fact | Value |
|---|---|
| Internal ID | `salespricerule` |

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `datecreated` | datetime | Date Created | no |  | marked_new |  |
| `description` | textarea | Description | no |  | marked_new |  |
| `enddate` | date | End Date | no |  | marked_new |  |
| `externalid` | text | External ID | no |  | marked_new |  |
| `id` | integer | Internal ID | no |  | marked_new |  |
| `isappliedtoallitems` | checkbox | Apply Selected Price Level To All Items Assigned To This Level | no |  | marked_new |  |
| `isinactive` | checkbox | Inactive | no |  | marked_new |  |
| `lastmodifieddate` | datetime | Last Modified Date | no |  | marked_new |  |
| `name` | text | Name | yes |  | marked_new |  |
| `pricelevel` | select | Price Level | yes |  | marked_new |  |
| `startdate` | date | Start Date | no |  | marked_new |  |

## Sublist `customerdimension` (Customer Dimension)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `customer` | select | Customer | yes | marked_new |  |
| `datecreated` | datetime | Date Created | no | marked_new |  |
| `id` | integer | Internal ID | no | marked_new |  |
| `lastmodifieddate` | datetime | Last Modified Date | no | marked_new |  |
| `parentrecord` | select | Sales Price Rule | no | marked_new |  |

## Sublist `customergroups` (Customer Groups)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `customergroup` | select | Customer Group | yes | marked_new |  |
| `datecreated` | datetime | Date Created | no | marked_new |  |
| `id` | integer | Internal ID | no | marked_new |  |
| `lastmodifieddate` | datetime | Last Modified Date | no | marked_new |  |
| `parentrecord` | select | Sales Price Rule | no | marked_new |  |
