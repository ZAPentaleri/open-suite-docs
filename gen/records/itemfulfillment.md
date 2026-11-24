# Item Fulfillment (`itemfulfillment`)
<!-- id: record:itemfulfillment -->

| Fact | Value |
|---|---|
| Internal ID | `itemfulfillment` |
| Category | transaction |
| Client scripts | yes |
| Server scripts | yes |
| Search data via | Transaction |
| Tags | `custom_fields_supported`, `no_copy`, `no_create` |

## Record facts

- `script_types`: `client`, `server`
- `not_supported`: `create`, `copy`
- `script_functions`: `update`, `delete`, `search`
- `user_event_types`: `pick`, `pack`, `ship`
- `list_sublists`: `item`
- `sublist_missing_from_records_browser`: `shipping`
- `subrecords`: `returnshippingaddress`
- `deprecated_fields`: `returnshipcountry`, `returnshipaddr1`, `returnshipaddr2`, `returnshipcity`, `returnshipstate`, `returnshipzipcode`, `returnaddress`
- `purpose`: `records_shipment_of_order_items`
- `effect`: `fulfillment_also_creates_invoice` (when `advanced_shipping_disabled`)
- `effect`: `fulfillment_and_invoicing_independent` (when `advanced_shipping_enabled`)
- `workaround`: `read_shipping_sublist_ids_from_other_order_records`
- `field_requires`: `shippingtaxcode:per_line_taxes`
- `same_as`: `returnshipcountry:country`, `returnshipaddr1:addr1`, `returnshipaddr2:addr2`, `returnshipcity:city`, `returnshipstate:state`, `returnshipzipcode:zip`, `returnaddress:addrtext` [`deprecated_to_subrecord_field`]
- `doc_inconsistency`: `internal_id_sentence_code_markup_broken`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `ancillaryendorsementfedex` | select | Ancillary Endorsement | no | no |  | `purpose`: `smartpost_ancillary_endorsement`; `options`: `Address Correction`, `Return Service`; `applies_when`: `smartpost_returns` |
| `certifiedmailusps` | checkbox | Certified Mail | no | no |  | `purpose`: `usps_certified_mail` |
| `createdfrom` | select | Created From | no | no |  | `purpose`: `created_from_sales_order`; `value_source`: `system_displayed` |
| `entity` | select | Customer | no | no |  | `purpose`: `customer_or_job`; `value_source`: `system_displayed` |
| `handlingcost` | currency | Handling Cost | no | no |  | `purpose`: `handling_cost`; `value_source`: `calculated_from_ship_method`; `value_source`: `shipping_item_rates` |
| `insidedeliveryfedex` | checkbox | Inside Delivery | no | no |  | `purpose`: `fedex_inside_delivery` |
| `insidepickupfedex` | checkbox | Inside Pickup | no | no |  | `purpose`: `fedex_inside_pickup` |
| `memo` | text | Memo | no | yes |  | `purpose`: `memo`; `supported_in`: `search`; `displayed_on`: `reports` |
| `packeddate` | date | Packed Date | no | no |  | `purpose`: `pick_pack_ship_dates`; `effect`: `status_change_fills_empty_dates_and_trandate`; `effect`: `posting_period_required_from_ship_date` (when `shipped`); `help_text_shared_with`: `itemfulfillment.pickeddate`, `itemfulfillment.shippeddate` |
| `pickeddate` | date | Picked Date | no | no |  | `purpose`: `pick_pack_ship_dates`; `effect`: `status_change_fills_empty_dates_and_trandate`; `effect`: `posting_period_required_from_ship_date` (when `shipped`); `help_text_shared_with`: `itemfulfillment.packeddate`, `itemfulfillment.shippeddate` |
| `postingperiod` | select | Posting Period | no | no |  | `purpose`: `posting_period`; `cannot`: `post_to_closed_period` |
| `requestedby` | select | Requested By | no | no |  | `purpose`: `originating_fulfillment_request`; `value_source`: `system_displayed` |
| `shipaddress` | textarea | Address | no | no |  | `purpose`: `ship_to_address`; `options`: `New`, `Custom`; `option_meanings`: `new:saved_to_entity`, `custom:transaction_only` |
| `shipaddresslist` | select | Address Select | no | no |  | `purpose`: `ship_to_address_selector`; `ui_effect`: `fills_address_field`; `options`: `New`, `Custom`; `option_meanings`: `new:saved_to_entity`, `custom:transaction_only` |
| `shipisresidential` | checkbox | Residential Address | no | no |  | `purpose`: `residential_address`; `effect`: `improves_ups_and_fedex_rates` |
| `shipmentweightfedex` | posfloat | Total Package Weight | no | no |  | `purpose`: `fedex_total_weight`; `value_source`: `sum_of_package_weights`; `value_unit`: `pounds` |
| `shipmentweightups` | posfloat | Total Package Weight | no |  |  | `purpose`: `ups_total_weight`; `value_source`: `sum_of_package_weights` |
| `shipmethod` | select | Shipping Method | no | no |  | `purpose`: `ship_method`; `effect`: `fills_shipping_cost` (when `charge_for_shipping`); `allowed_values_listed_in`: `shipping_item_records`; `configured_by`: `charge_for_shipping_setting`; `requires`: `shipping_item_charge_details` (when `charge_for_shipping`) |
| `shippeddate` | date | Shipped Date | no | no |  | `purpose`: `pick_pack_ship_dates`; `effect`: `status_change_fills_empty_dates_and_trandate`; `effect`: `posting_period_required_from_ship_date` (when `shipped`); `help_text_shared_with`: `itemfulfillment.packeddate`, `itemfulfillment.pickeddate` |
| `shippingcost` | currency | Shipping Cost | no | no |  | `purpose`: `shipping_cost`; `applies_when`: `charging_for_shipping` |
| `shipstatus` | select | Status | no | no |  | `purpose`: `ship_status`; `options`: `Picked`, `Packed`, `Shipped`; `option_meanings`: `picked:separate_pick_pack_ship_steps`, `packed:pick_and_pack_then_ship`, `shipped:all_steps_then_bill` |
| `termsofsalefedex` | select | Terms of Sale | no | no |  | `purpose`: `fedex_terms_of_sale`; `required_when`: `dutiable_international_express`; `not_supported_for`: `international_document_express` |
| `trandate` | date | Date | yes | no |  | `purpose`: `fulfillment_date` |
| `tranid` | text | Ref. No. | no | no |  | `purpose`: `fulfillment_number`; `default_behavior`: `highest_number_plus_one`; `caveat`: `next_number_reverts_to_standard_pattern` |
| `accessibilitytypefedex` | select | Accessibility | no | no |  |  |
| `b13afilingoptionfedex` | select | B13A Filing Option | no | no |  |  |
| `b13astatementdatafedex` | text | B13A Statement Data | no | no |  |  |
| `backupemailaddressups` | text |  | no | no |  |  |
| `balance` | currency |  | no | no |  |  |
| `bookingconfirmationnumfedex` | text | Booking Confirmation Number | no | no |  |  |
| `consolidatebalance` | currency | Consolidated Balance | no | no |  |  |
| `createddate` | datetime |  | no | no |  |  |
| `createdfromshipgroup` | integer |  | no |  |  |  |
| `customform` | select | Custom Form | yes | no |  |  |
| `exchangerate` | posfloat |  | no | no |  |  |
| `externalid` | text | ExternalId | no | no |  |  |
| `haladdressfedex` | select | HAL FedEx Location | no | no |  |  |
| `halphonefedex` | phone | Contact Phone Number | no | no |  |  |
| `hazmattypefedex` | select | Type | no | no |  |  |
| `holdatlocationfedex` | checkbox | Hold At Location | no | no |  |  |
| `isbasecurrency` | checkbox | Base Currency | no | no |  |  |
| `iscargoaircraftonlyfedex` | checkbox | Cargo Aircraft Only | no | no |  |  |
| `lastmodifieddate` | datetime |  | no | no |  |  |
| `shipcompany` | text | Addressee | no | no |  |  |
| `shipcountry` | select | Country | no | no |  |  |
| `shipmentprocessed` | checkbox | Shipment Request Sent | no |  |  |  |
| `shipmentweightusps` | posfloat | Total Package Weight | no |  |  |  |
| `shipoverride` | checkbox | Override | no | no |  |  |
| `shipphone` | phone | Phone | no | no |  |  |
| `shippingaddress` | summary |  | no | no |  |  |
| `status` | text |  | no | no |  |  |
| `statusRef` | text |  | no | no |  |  |
| `subsidiary` | select |  | no | no |  |  |

## Sublist `accountingbookdetail` (Accounting Books)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `accountingbook` | select | Secondary Book | no |  |  |
| `exchangerate` | currency2 | Exchange Rate | yes |  |  |

## Sublist `item` (Items)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `class` | text | Class | no |  |  |
| `countryofmanufacture` | text |  | no |  |  |
| `createdpo` | text |  | no |  |  |
| `department` | text | Department | no |  |  |
| `excludefromraterequest` | checkbox | Exclude Item from Rate Request | no |  |  |
| `inventorydetail` | summary | Inventory Detail | no |  |  |
| `item` | text |  | no |  |  |
| `itemdescription` | text | Description | no |  |  |
| `itemkey` | text | Item | no |  |  |
| `itemname` | text | Item | no |  |  |
| `itemreceive` | checkbox | Fulfill | no |  |  |
| `itemtype` | text |  | no |  |  |
| `line` | text |  | no |  |  |
| `location` | select | Location | yes |  |  |
| `manufactureraddr1` | text |  | no |  |  |
| `manufacturercity` | text |  | no |  |  |
| `manufacturerstate` | text |  | no |  |  |
| `manufacturertariff` | text |  | no |  |  |
| `manufacturertaxid` | text |  | no |  |  |
| `manufacturerzip` | text |  | no |  |  |
| `multmanufactureaddr` | text |  | no |  |  |
| `onhand` | float | On Hand | no |  |  |
| `options` | namevaluelist | Options | no |  |  |
| `preferencecriterion` | text |  | no |  |  |
| `producer` | text |  | no |  |  |
| `quantity` | posfloat | Quantity | no |  |  |
| `quantityremaining` | float |  | no |  |  |
| `schedulebcode` | text |  | no |  |  |
| `schedulebnumber` | text |  | no |  |  |
| `schedulebquantity` | text |  | no |  |  |
| `sitemname` | text | Item | no |  |  |
| `units` | text |  | no |  |  |

## Sublist `packageFedex` (Packages)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `admpackagetypefedex` | select | Admissibility Package Type | no |  |  |
| `alcoholrecipienttypefedex` | select | Alcohol Recipient Type | no |  |  |
| `authorizationnumberfedex` | text | Authorization Number | no |  |  |
| `codfreighttypefedex` | select | Freight Charge Added to COD Amount | no |  |  |
| `codmethodfedex` | select | C.O.D. Method | no |  |  |
| `codotherchargefedex` | currency | C.O.D. Other Charge | no |  |  |
| `custompackagefedex` | select | Your Packaging | no |  |  |
| `dryiceweightfedex` | posfloat | Dry Ice Weight | no |  |  |
| `isalcoholfedex` | checkbox | Alcohol | no |  |  |
| `isnonhazlithiumfedex` | checkbox | Non-Hazardous Lithium Batteries | no |  |  |

## Tabs

| Tab ID | Label |
|---|---|
| `address` | Address |
| `custom` | Custom |
| `history` | History |
| `internationalfedex` | International |
| `internationalups` | International |
| `items` | Items |
| `packages` | Packages |
| `shipping` | Shipping |
| `workflow` | Workflow |

## Suspected source errors

> [!SOURCE-ERROR] `internal_id_sentence_code_markup_broken`
<!-- source-error: record:itemfulfillment | - | internal_id_sentence_code_markup_broken -->
