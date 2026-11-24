# Landed Cost (`landedcost`)
<!-- id: record:landedcost -->

| Fact | Value |
|---|---|
| Internal ID | `landedcost` |
| Category | transaction |
| Client scripts | no |
| Server scripts | yes |
| Searchable | yes |
| Subrecord | yes |
| Tags | `deleted_record_search_supported`, `scripting_full` |

## Record facts

- `script_types`: `server`
- `requires_feature`: `Landed Cost` [`real_name`]
- `is_subrecord`: `true`
- `user_events_supported`: `false`
- `requires`: `parent_landed_cost_per_line_checked`, `item_tracks_landed_cost`
- `purpose`: `landed_costs_for_one_item_line`
- `value_examples`: `customs_duties`, `freight_fees`
- `applies_to`: `check`, `credit_card_charge`, `item_receipt`, `vendor_bill`
- `configured_by`: `enable_features_items_and_inventory_subtab` [`landed_cost`]

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `item` | select | Item | no | no |  | `purpose`: `assembly_filter_for_work_orders` |
| `externalid` | text | ExternalId | no | no |  |  |
| `total` | currency2 | Total | no | no |  |  |

## Sublist `landedcostdata` (Landed Cost)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `amount` | poscurrency | Amount | yes |  |  |
| `costcategory` | select | Cost Category | yes |  |  |
