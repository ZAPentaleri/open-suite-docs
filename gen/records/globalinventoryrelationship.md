# Global Inventory Relationship (`globalinventoryrelationship`)
<!-- id: record:globalinventoryrelationship -->

| Fact | Value |
|---|---|
| Internal ID | `globalinventoryrelationship` |
| Category | list |
| Client scripts | yes |
| Server scripts | yes |
| Searchable | yes |
| Tags | `custom_fields_supported`, `no_copy`, `supports_create`, `supports_delete`, `supports_read`, `supports_search`, `supports_update` |

## Record facts

- `script_types`: `client`, `server`
- `not_supported`: `copy`
- `script_functions`: `create`, `read`, `edit`, `delete`, `search`
- `requires_feature`: `Multi-Location Inventory`, `Advanced Inventory Management`, `Intercompany Cross-Subsidiary Fulfillment` [`real_name`]
- `script_functions`: `create`, `read`, `edit`, `delete`, `search`
- `not_supported`: `copy`
- `search_only_sublists`: `systemnotes`
- `not_updatable_fields`: `originatingsubsidiary`, `inventorysubsidiary`
- `hidden_by_default_fields`: `allowcrosssubfulfillment`, `allowcrosssubcustomerreturn`
- `purpose`: `fulfill_and_receive_returns_across_subsidiaries`
- `can`: `expose_hidden_cross_sub_boxes_on_custom_form`
- `constraint`: `originating_and_inventory_subsidiary_differ_and_pair_unique`
- `constraint`: `at_least_one_of_fulfillment_or_return_box_checked`
- `cannot`: `delete_when_used_by_sales_order_or_return_authorization`
- `editable_only_when`: `no_open_sales_orders_use_it` [`allowcrosssubfulfillment`]
- `editable_only_when`: `no_open_return_authorizations_use_it` [`allowcrosssubcustomerreturn`]
- `cannot`: `clear_cross_sub_box_while_locations_listed`
- `cannot`: `remove_location_used_by_open_cross_sub_transaction`
- `required_fields`: `originatingsubsidiary`, `inventorysubsidiary`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `alllocationscustomerreturn` | checkbox | All Customer Return Locations | no | no |  | `purpose`: `all_customer_return_locations`; `default_value`: `true`; `effect`: `location_list_synced_with_inventory_subsidiary`; `value_examples`: `three_new_locations_added_automatically` |
| `alllocationsfulfillment` | checkbox | All Fulfillment Locations | no | no |  | `purpose`: `all_fulfillment_locations`; `default_value`: `true`; `effect`: `location_list_synced_with_inventory_subsidiary`; `value_examples`: `three_new_locations_added_automatically` |
| `allowcrosssubfulfillment` | checkbox | Allow Cross-Subsidiary Fulfillment | no | no |  | `purpose`: `allow_cross_subsidiary_fulfillment`; `doc_inconsistency`: `help_says_receive_only_in_listed_locations` |
| `customform` | select | Custom Form | no | no |  | `purpose`: `custom_form` |
| `inventorysubsidiary` | select | Inventory Subsidiary | yes | no |  | `purpose`: `inventory_subsidiary`; `effect`: `its_locations_fulfill_or_receive` |
| `originatingsubsidiary` | select | Originating Subsidiary | yes | no |  | `purpose`: `originating_subsidiary`; `applies_to`: `sales_orders`, `return_authorizations` |
| `allowcrosssubcustomerreturn` | checkbox | Allow Cross-Subsidiary Customer Return | no | no |  |  |
| `externalid` | text | ExternalId | no | no |  |  |

## Sublist `customerreturn` (Customer Return)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `city` | text | City | no |  |  |
| `country` | text | Country | no |  |  |
| `inventorylocation` | select | Inventory Location | no |  |  |
| `state` | text | State | no |  |  |

## Sublist `fulfillment` (Fulfillment)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `city` | text | City | no |  |  |
| `country` | text | Country | no |  |  |
| `inventorylocation` | select | Inventory Location | no |  |  |
| `state` | text | State | no |  |  |

## Sublist `systemnotes` (System Notes)

## Tabs

| Tab ID | Label |
|---|---|
| `invtloccustreturn` | Customer Return |
| `invtlocfulfillment` | Fulfillment |
| `s_sysinfo` | System Information |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `alllocationscustomerreturn` | checkbox | All Customer Return Locations |  |
| `alllocationsfulfillment` | checkbox | All Fulfillment Locations |  |
| `allowcrosssubcustomerreturn` | checkbox | Allow Cross-Subsidiary Customer Return |  |
| `allowcrosssubfulfillment` | checkbox | Allow Cross-Subsidiary Fulfillment |  |
| `externalid` | select | External ID |  |
| `externalidstring` | text | External ID (Text) |  |
| `formuladate` | date | Formula (Date) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulatext` | text | Formula (Text) |  |
| `intercompanyactivitytype` | select | Intercompany Activity Type |  |
| `internalid` | select | Internal ID |  |
| `internalidnumber` | integer | Internal ID (Number) |  |
| `isinactive` | checkbox | Is Inactive |  |
| `location` | select | Inventory Location |  |
| `subsidiary` | select | Inventory Subsidiary |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `alllocationscustomerreturn` | checkbox | All Customer Return Locations |  |
| `alllocationsfulfillment` | checkbox | All Fulfillment Locations |  |
| `allowcrosssubcustomerreturn` | checkbox | Allow Cross-Subsidiary Customer Return |  |
| `allowcrosssubfulfillment` | checkbox | Allow Cross-Subsidiary Fulfillment |  |
| `externalid` | select | External ID |  |
| `formulacurrency` | currency | Formula (Currency) |  |
| `formuladate` | date | Formula (Date) |  |
| `formuladatetime` | datetime | Formula (Date/Time) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulapercent` | percent | Formula (Percent) |  |
| `formulatext` | text | Formula (Text) |  |
| `intercompanyactivitytype` | select | Intercompany Activity Type |  |
| `internalid` | integer | Internal ID |  |
| `inventorysubsidiary` | select | Inventory Subsidiary |  |
| `inventorysubsidiarynohierarchy` | select | Inventory Subsidiary (no hierarchy) |  |
| `isinactive` | checkbox | Inactive |  |
| `location` | select | Location |  |
| `origsubsidiarynohierarchy` | select | Originating Subsidiary (no hierarchy) |  |
| `subsidiary` | select | Originating Subsidiary |  |

## Search joins

| Join ID | Label | Joined record |
|---|---|---|
| `inventorysubsidiary` | Inventory Subsidiary | Subsidiary |
| `location` | Inventory Location | Location |
| `originatingsubsidiary` | Originating Subsidiary | Subsidiary |

## Suspected source errors

> [!SOURCE-ERROR] `help_says_receive_only_in_listed_locations` (on `allowcrosssubfulfillment`)
<!-- source-error: record:globalinventoryrelationship | allowcrosssubfulfillment | help_says_receive_only_in_listed_locations -->
