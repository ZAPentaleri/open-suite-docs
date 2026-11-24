# Price Level (`pricelevel`)
<!-- id: record:pricelevel -->

| Fact | Value |
|---|---|
| Internal ID | `pricelevel` |
| Category | list |
| Client scripts | yes |
| Server scripts | yes |
| Searchable | no |
| Tags | `custom_fields_supported`, `deleted_record_search_supported`, `no_search` |

## Record facts

- `script_types`: `client`, `server`
- `not_supported`: `search`
- `script_functions`: `create`, `update`, `copy`, `delete`
- `not_supported`: `search`
- `max_price_levels`: `1000` (when `multiple_pricing`)
- `purpose`: `price_level_values_for_items_and_opportunities`
- `value_examples`: `employee_price`, `corporate_discount_price`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `discountpct` | percent | Markup/Discount % | no | no |  | `purpose`: `markup_or_discount_percent`; `format`: `negative_for_discount` |
| `isinactive` | checkbox | Inactive | no | no |  | `purpose`: `inactivate_price_level`; `can`: `reactivate_any_time` |
| `isonline` | checkbox | Online Price Level | no | no |  | `purpose`: `available_in_web_store` |
| `name` | text | Price Level | yes | no |  | `purpose`: `price_level_name` |
| `updateexistingprices` | checkbox | Update Existing Prices | no | no |  | `purpose`: `update_existing_prices`; `effect`: `recalculates_item_prices_from_base_price`; `caveat`: `price_cleared_when_no_base_price` |
| `externalid` | text | ExternalId | no | no |  |  |
| `pricecalculationformula` | text | Price Calculation Formula | no | no | marked_new |  |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `externalid` | select | External ID |  |
| `externalidstring` | text | External ID (Text) |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `externalid` | select | External ID |  |
