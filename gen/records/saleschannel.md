# Sales Channel (`saleschannel`)
<!-- id: record:saleschannel -->

| Fact | Value |
|---|---|
| Internal ID | `saleschannel` |
| Category | transaction |
| Client scripts | yes |
| Server scripts | yes |
| Searchable | yes |
| Tags | `no_transform`, `supports_copy`, `supports_create`, `supports_delete`, `supports_read`, `supports_search`, `supports_update` |

## Record facts

- `requires_feature`: `Sales Channel Allocation` [`real_name`]
- `script_functions`: `create`, `read`, `update`, `delete`, `copy`, `search`
- `not_supported`: `transform`
- `not_exposed`: `description`
- `read_only_sublists`: `supply_allocation`, `fulfillment`
- `purpose`: `sales_category_by_customer_type_and_distribution_channel`
- `value_examples`: `web_shop`, `retail_outlet`
- `doc_inconsistency`: `internal_id_printed_with_capitals`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `description` | richtext | Description | no |  |  | `purpose`: `description` |
| `isinactive` | checkbox | Inactive | no |  |  | `purpose`: `inactivate_sales_channel`; `not_displayed_on`: `saved_searches` |
| `name` | text | Name | yes |  |  | `purpose`: `sales_channel_name` |
| `onlyusereservationassupply` | checkbox | Can only use dedicated order reservations as supply | no |  |  | `purpose`: `only_reservations_as_supply` |
| `orderpriority` | float | Order Priority | no |  |  | `purpose`: `channel_order_priority` |
| `customform` | select | Custom Form | no |  |  |  |
| `externalid` | text | External ID | no |  |  |  |
| `id` | integer | Internal ID | no |  |  |  |
| `templatestored` | checkbox |  | no |  |  |  |

## Suspected source errors

> [!SOURCE-ERROR] `internal_id_printed_with_capitals`
<!-- source-error: record:saleschannel | - | internal_id_printed_with_capitals -->
