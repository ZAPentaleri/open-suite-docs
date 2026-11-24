# Order Schedule (`orderschedule`)
<!-- id: record:orderschedule -->

| Fact | Value |
|---|---|
| Internal ID | `orderschedule` |
| Category | transaction |
| Client scripts | yes |
| Subrecord | yes |
| Tags | `access_via_parent_record` |

## Record facts

- `is_subrecord`: `true`
- `server_access`: `through_parent_record`
- `scriptable_from`: `body_field`, `line_item`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `createpurchaseorder` | select | Create Purchase Orders | yes | no |  | `purpose`: `purchase_order_creation_method`; `options`: `Manually`, `At Lead Time`; `option_meanings`: `manually:release_orders_by_hand`, `at_lead_time:released_by_item_lead_time` |
| `createschedule` | select | Create Schedule | yes | no |  | `purpose`: `schedule_creation_method`; `options`: `Manually`, `Autogenerate` |
| `enddate` | date | End Date | no | no |  | `purpose`: `last_date_for_orders_in_schedule` |
| `releasefrequency` | select | Release Frequency | no | no |  | `purpose`: `release_frequency`; `options`: `Daily`, `Weekly`, `Monthly`, `Quarterly` |
| `startdate` | date | Start Date | no | no |  | `purpose`: `first_date_for_orders_in_schedule` |
| `total` | float | Total Quantity | no | no |  | `purpose`: `total_quantity`; `value_source`: `line_quantity` |
| `currencyprecision` | integer |  | no | no |  |  |
| `externalid` | text | ExternalId | no | no |  |  |
| `item` | integer |  | no | no |  |  |

## Sublist `schedule` (Schedule)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `amount` | poscurrency |  | no |  |  |
| `id` | integer |  | no |  |  |
| `memo` | text | Memo | no |  |  |
| `orderschedule` | integer |  | no |  |  |
| `purchaseorder` | select | Order | no |  |  |
| `quantity` | posfloat | Quantity | yes |  |  |
| `release` | checkbox | Release | no |  |  |
| `trandate` | date | Date | yes |  |  |
