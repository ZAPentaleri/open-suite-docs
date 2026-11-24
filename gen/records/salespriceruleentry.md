# Sales Price Rule Entry (`salespriceruleentry`)
<!-- id: record:salespriceruleentry -->

| Fact | Value |
|---|---|
| Internal ID | `salespriceruleentry` |

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `datecreated` | datetime | Date Created | no |  | marked_new |  |
| `externalid` | text | External ID | no |  | marked_new |  |
| `id` | integer | Internal ID | no |  | marked_new |  |
| `item` | select | Item | yes |  | marked_new |  |
| `lastmodifieddate` | datetime | Last Modified Date | no |  | marked_new |  |
| `pricerule` | select | Sales Price Rule | yes |  | marked_new |  |
