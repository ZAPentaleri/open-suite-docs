# Location (`location`)
<!-- id: record:location -->

| Fact | Value |
|---|---|
| Internal ID | `location` |
| Category | list |
| Client scripts | no |
| Server scripts | yes |
| Tags | `custom_fields_supported` |

## Record facts

- `script_types`: `server`
- `client_supported`: `false`
- `purpose`: `offices_or_warehouses_for_employees_and_transactions`
- `script_functions`: `create`, `update`, `copy`, `delete`, `search`
- `can`: `filter_report_data_by_location`
- `doc_inconsistency`: `records_browser_omits_mainaddress` [`needs_human_review`]

## Body fields

| Field ID | Type | Label | Required | submitField | Read-only | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `addrtext` | address | Address | no | no |  |  | `purpose`: `formatted_address_text`; `value_source`: `built_from_address_fields` |
| `allowstorepickup` | checkbox | Allow Store Pickup | no | no |  |  | `purpose`: `allow_store_pickup`; `effect`: `selectable_for_store_pickup_lines`; `requires`: `line_items_allow_store_pickup` |
| `autoassignmentregionsetting` | select | Auto-Assigning This Location | no | no |  |  | `purpose`: `auto_assignment_region_setting`; `options`: `disallow`, `allow_worldwide`, `allow_worldwide_except_excluded_regions`, `allow_specified_regions_only` |
| `bufferstock` | integer | Buffer Stock | no | no |  |  | `purpose`: `buffer_stock`; `effect`: `auto_assignment_skips_if_below_buffer`; `value_examples`: `stock_6_buffer_5_order_2_not_assigned` |
| `dailyshippingcapacity` | integer | Daily Shipping Capacity | no | no |  |  | `purpose`: `daily_shipping_capacity`; `requires_feature`: `Fulfillment Request` [`real_name`]; `effect`: `auto_assignment_stops_at_capacity`; `caveat`: `manual_line_location_can_exceed` |
| `geolocationmethod` | radio | Derive From Postal Code | no | no |  |  | `purpose`: `geolocation_method`; `options`: `Derive From Postal Code`, `Use Latitude/Longitude Coordinates`; `default_value`: `Derive From Postal Code`; `value_source`: `postal_code_centroid` (when `derive_from_postal_code`) |
| `includeincontroltower` | checkbox | Include In Control Tower | no |  |  |  | `purpose`: `include_in_control_tower_snapshots` |
| `includeinsupplyplanning` | checkbox | Include in Supply Planning | no | no |  |  | `purpose`: `include_in_supply_planning`; `applies_when`: `plan_through_distribution_network_preference` |
| `isinactive` | checkbox | Location is Inactive | no | no |  |  | `purpose`: `inactivate_location`; `ui_effect`: `hidden_from_location_lists` |
| `latitude` | float | Latitude | no | no |  |  | `purpose`: `latitude`; `allowed_range`: `-90`, `90`; `value_examples`: `37.5367`; `format`: `decimal_degrees`; `recommendation`: `at_least_four_decimal_places`; `caveat`: `decimal_symbol_per_number_format_preference` |
| `locationtype` | select | Location Type | no | no |  |  | `purpose`: `location_type`; `allowed_values`: `Store`, `Warehouse`, `Undefined`; `effect`: `used_by_automatic_location_assignment` |
| `logo` | select | Logo | no | no |  |  | `purpose`: `location_logo`; `displayed_on`: `printed_forms`, `emailed_forms`; `allowed_values`: `JPG`, `GIF`; `constraint`: `max_200x60_pixels_on_standard_forms`; `requires`: `image_available_without_login` |
| `longitude` | float | Longitude | no | no |  |  | `purpose`: `longitude`; `allowed_range`: `-180`, `180`; `value_examples`: `-122.3285`; `format`: `decimal_degrees`; `recommendation`: `at_least_four_decimal_places`; `caveat`: `decimal_symbol_per_number_format_preference` |
| `makeinventoryavailable` | checkbox | Make Inventory Available | no | no |  |  | `purpose`: `inventory_available_to_orders`; `false_means`: `excluded_from_available_count` |
| `makeinventoryavailablestore` | checkbox | Make Inventory Available in Web Store | no | no |  |  | `purpose`: `inventory_available_in_web_store`; `false_means`: `excluded_from_web_store_available` |
| `name` | text | Name | yes | no |  |  | `purpose`: `location_name`; `format`: `alphanumeric`; `max_length`: `60` characters; `displayed_on`: `location_lists` |
| `nextpickupcutofftime` | datetime | Next Pickup Cut-Off Time | no | no | yes |  | `purpose`: `next_pickup_cutoff_time`; `value_source`: `business_hours_same_day_cutoffs` |
| `override` | checkbox | Override | no | no |  |  | `purpose`: `override_free_form_address_text`; `true_means`: `addrtext_not_rebuilt_from_address_fields` |
| `parent` | select | Sublocation of | no | no |  |  | `purpose`: `parent_location` |
| `sopredconfidence` | percent | Sales Order Predicted Risk Confidence | no | no |  |  | `purpose`: `late_order_prediction_confidence` |
| `sopredicteddays` | integer | Sales Order Predicted Days Late/Early | no | no |  |  | `purpose`: `predicted_order_days_late_or_early` |
| `storepickupbufferstock` | float | Store Pickup Buffer Stock | no |  |  |  | `purpose`: `store_pickup_buffer_stock`; `effect`: `reduces_quantity_available_for_pickup`; `constraint`: `pickup_orders_need_available_above_zero`; `applies_to`: `all_items_at_location`; `help_text_shared_with`: `itemlocationconfiguration.locationstorepickupbufferstock` |
| `subsidiary` | select | Subsidiary | no | no |  |  | `purpose`: `location_subsidiaries`; `requires_feature`: `OneWorld` [`real_name`]; `constraint`: `one_subsidiary_when_inventory_feature`; `value_kind`: `multi_select` (when `no_inventory_feature`) |
| `timezone` | select | Time Zone | yes | no |  |  | `purpose`: `location_time_zone` |
| `totalshippingcapacity` | integer | Total Shipping Capacity | no | no |  |  | `purpose`: `total_shipping_capacity`; `requires_feature`: `Fulfillment Request` [`real_name`]; `value_source`: `open_fulfillment_requests` [`backlog`] |
| `tranprefix` | text | Transaction Prefix | no | no |  |  | `purpose`: `transaction_number_prefix`; `max_length`: `8` characters; `format`: `alphanumeric`; `configured_by`: `auto_generated_numbers_use_location` |
| `usebins` | checkbox | Use Bins | no | no |  |  | `purpose`: `use_bins_at_location` |
| `addrphone` | phone | Phone | no | no |  |  |  |
| `attention` | text | Attention | no | no |  |  |  |
| `city` | text | City | no | no |  |  |  |
| `country` | select | Country | no | no |  |  |  |
| `externalid` | text | ExternalId | no | no |  |  |  |
| `returnaddr` | address | Address | no | no |  |  |  |
| `returnaddress1` | text | Address1 | no | no |  |  |  |
| `returnaddress2` | text | Address2 | no | no |  |  |  |
| `returncity` | text | City | no | no |  |  |  |
| `returncountry` | select | Country | no | no |  |  |  |
| `returnstate` | select | State | no | no |  |  |  |
| `returnzip` | text | Zip | no | no |  |  |  |
| `state` | text | State/Province | no | no |  |  |  |
| `zip` | text | Zip | no | no |  |  |  |

## Sublist `businesshours` (Business Hours)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `endtime` | timeofday | End time | yes |  |  |
| `isfriday` | checkbox | Friday | no |  |  |
| `ismonday` | checkbox | Monday | no |  |  |
| `issaturday` | checkbox | Saturday | no |  |  |
| `issunday` | checkbox | Sunday | no |  |  |
| `isthursday` | checkbox | Thursday | no |  |  |
| `istuesday` | checkbox | Tuesday | no |  |  |
| `iswednesday` | checkbox | Wednesday | no |  |  |
| `samedaypickupcutofftime` | timeofday | Same Day Pickup Cut-Off Time | no |  |  |
| `starttime` | timeofday | Start Time | yes |  |  |

## Sublist `excludelocationregions` (Exclude These Regions)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `name` | text | Region | no |  |  |
| `ranking` | integer | None | no |  |  |
| `region` | select | Region | yes |  |  |

## Sublist `includelocationregions` (Include These Regions)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `name` | text | Region | no |  |  |
| `ranking` | integer | None | no |  |  |
| `region` | select | Region | yes |  |  |

## Tabs

| Tab ID | Label |
|---|---|
| `addressbook` | Address |
| `history` | System Notes |
| `returnsaddrtab` | Return Address |
| `translation` | Translation |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `address` | textarea | Address |  |
| `allowstorepickup` | checkbox | Allow Store Pickup |  |
| `autoassignmentregionsetting` | select | Auto-Assigning This Location |  |
| `bufferstock` | integer | Buffer Stock |  |
| `city` | text | City |  |
| `country` | select | Country |  |
| `county` | text | County/State |  |
| `dailyshippingcapacity` | integer | Daily Shipping Capacity |  |
| `endtime` | timeofday | End Time |  |
| `externalid` | select | External ID |  |
| `externalidstring` | text | External ID (Text) |  |
| `formuladate` | date | Formula (Date) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulatext` | text | Formula (Text) |  |
| `geolocationmethod` | select | Geolocation Method |  |
| `includeinsupplyplanning` | checkbox | Include in Supply Planning |  |
| `internalid` | select | Internal ID |  |
| `internalidnumber` | integer | Internal ID (Number) |  |
| `isfriday` | checkbox | Friday |  |
| `isinactive` | checkbox | Inactive |  |
| `ismonday` | checkbox | Monday |  |
| `isoffice` | checkbox | Staffed Location |  |
| `issaturday` | checkbox | Saturday |  |
| `issunday` | checkbox | Sunday |  |
| `isthursday` | checkbox | Thursday |  |
| `istuesday` | checkbox | Tuesday |  |
| `iswednesday` | checkbox | Wednesday |  |
| `locationtype` | select | Location Type |  |
| `makeinventoryavailable` | checkbox | Make Inventory Available |  |
| `makeinventoryavailablestore` | checkbox | Make Inventory Available in Web Store |  |
| `name` | text | Name |  |
| `nextpickupcutofftime` | datetime | Next Pickup Cut-Off Time |  |
| `phone` | phone | Phone |  |
| `samedaypickupcutofftime` | timeofday | Same Day Pickup Cut-Off Time |  |
| `sopredconfidence` | percent | Sales Order Predicted Risk Confidence |  |
| `sopredicteddays` | integer | Sales Order Predicted Days Late/Early |  |
| `starttime` | timeofday | Start Time |  |
| `state` | select | State/Province |  |
| `storepickupbufferstock` | float | Store Pickup Buffer Stock |  |
| `subsidiary` | multiselect | Subsidiary |  |
| `totalshippingcapacity` | integer | Total Shipping Capacity |  |
| `tranprefix` | text | Transaction Prefix |  |
| `usesbins` | checkbox | Uses Bins |  |
| `zip` | text | Zip |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `allowstorepickup` | checkbox | Allow Store Pickup |  |
| `autoassignmentregionsetting` | text | Auto-Assigning This Location |  |
| `bufferstock` | integer | Buffer Stock |  |
| `city` | text | City |  |
| `country` | text | Country |  |
| `dailyshippingcapacity` | integer | Daily Shipping Capacity |  |
| `endtime` | timeofday | End Time |  |
| `externalid` | select | External ID |  |
| `formulacurrency` | currency | Formula (Currency) |  |
| `formuladate` | date | Formula (Date) |  |
| `formuladatetime` | datetime | Formula (Date/Time) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulapercent` | percent | Formula (Percent) |  |
| `formulatext` | text | Formula (Text) |  |
| `geolocationmethod` | select | Geolocation Method |  |
| `includeinsupplyplanning` | checkbox | Include in Supply Planning |  |
| `internalid` | select | Internal ID |  |
| `isfriday` | checkbox | Friday |  |
| `isinactive` | checkbox | Inactive |  |
| `ismonday` | checkbox | Monday |  |
| `isoffice` | checkbox | Staffed Location |  |
| `issaturday` | checkbox | Saturday |  |
| `issunday` | checkbox | Sunday |  |
| `isthursday` | checkbox | Thursday |  |
| `istuesday` | checkbox | Tuesday |  |
| `iswednesday` | checkbox | Wednesday |  |
| `locationtype` | text | Location Type |  |
| `makeinventoryavailable` | checkbox | Make Inventory Available |  |
| `makeinventoryavailablestore` | checkbox | Make Inventory Available in Web Store |  |
| `name` | text | Name |  |
| `namenohierarchy` | text | Name (no hierarchy) |  |
| `nextpickupcutofftime` | datetime | Next Pickup Cut-Off Time |  |
| `phone` | phone | Phone |  |
| `samedaypickupcutofftime` | timeofday | Same Day Pickup Cut-Off Time |  |
| `sopredconfidence` | percent | Sales Order Predicted Risk Confidence |  |
| `sopredicteddays` | integer | Sales Order Predicted Days Late/Early |  |
| `starttime` | timeofday | Start Time |  |
| `state` | text | State/Province |  |
| `storepickupbufferstock` | float | Store Pickup Buffer Stock |  |
| `subsidiary` | multiselect | Subsidiary |  |
| `totalshippingcapacity` | integer | Total Shipping Capacity |  |
| `tranprefix` | text | Transaction Prefix |  |
| `usesbins` | checkbox | Use Bins |  |
| `zip` | text | Zip |  |

## Search joins

| Join ID | Label | Joined record |
|---|---|---|
| `address` | Address | Address |
| `returnaddress` | Return Address | Address |

## Suspected source errors

> [!SOURCE-ERROR] `records_browser_omits_mainaddress`
<!-- source-error: record:location | - | records_browser_omits_mainaddress -->
