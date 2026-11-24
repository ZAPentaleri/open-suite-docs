# Item Group (`itemgroup`)
<!-- id: record:itemgroup -->

| Fact | Value |
|---|---|
| Internal ID | `itemgroup` |
| Category | item |
| Client scripts | yes |
| Server scripts | yes |
| Searchable | yes |
| Search data via | Item |
| Tags | `custom_fields_supported`, `scripting_full` |

## Record facts

- `script_types`: `client`, `server`
- `script_functions`: `create`, `update`, `copy`, `delete`, `search`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `availabletopartners` | checkbox | Available to Adv. Partners | no | no |  | `purpose`: `visible_to_advanced_partner_center`; `applies_to`: `advanced_partner_center_roles`, `custom_roles_with_item_access` |
| `class` | select | Class | no | yes |  | `purpose`: `item_class`; `allowed_values_listed_in`: `class_records`; `ui_effect`: `new_option_creates_record` |
| `customform` | select | Custom Form | no | no |  | `purpose`: `entry_form`; `allowed_values_listed_in`: `standard_and_custom_entry_forms`; `ui_effect`: `new_option_creates_record` |
| `department` | select | Department | no | yes |  | `purpose`: `item_department`; `allowed_values_listed_in`: `department_records`; `ui_effect`: `new_option_creates_record` |
| `description` | textarea | Description | no | no |  | `purpose`: `item_description`; `displayed_on`: `sales_forms_description_column`; `ui_effect`: `text_enhance_button_may_appear` |
| `displayname` | text | Display Name/Code | no | no |  | `purpose`: `display_name`; `max_length`: `250` characters; `displayed_on`: `sales_forms_item_column` (when `basic_printing`); `default_behavior`: `item_name_on_printed_forms` (when `blank`) |
| `includechildren` | checkbox | Include Children | no | no |  | `purpose`: `share_item_with_child_subsidiaries`; `constraint`: `item_settings_compatible_across_subsidiaries`; `value_examples`: `shared_items_need_shared_income_and_asset_accounts` |
| `includestartendlines` | checkbox | Include Start/End Lines | no | no |  | `purpose`: `include_group_start_end_lines`; `true_means`: `group_lines_kept_on_picking_tickets`; `false_means`: `components_sorted_with_other_items` |
| `isinactive` | checkbox | Inactive | no | no |  | `purpose`: `inactivate_item`; `ui_effect`: `hidden_from_lists_unless_show_all` |
| `issueproduct` | select | Product | no | no |  | `purpose`: `issue_management_product`; `allowed_values_listed_in`: `issue_product_records`; `value_kind`: `product_with_versions_and_builds` |
| `isvsoebundle` | checkbox | Is VSOE Bundle | no | no |  | `purpose`: `vsoe_bundle_group`; `effect`: `vsoe_prices_allocate_group_revenue` |
| `itemid` | text | Item Name/Number | yes | no |  | `purpose`: `item_name`; `max_length`: `250` characters; `displayed_on`: `transaction_lists`; `displayed_on`: `sales_forms_item_column` (when `no_display_name`); `displayed_on`: `purchase_forms_item_column` (when `no_vendor_name`) |
| `itemtype` | text |  | no | no |  | `purpose`: `item_type_for_label_printing`; `special_values`: `All`; `doc_inconsistency`: `item_record_help_describes_label_printing` |
| `location` | select | Location | no | yes |  | `purpose`: `item_location`; `requires_feature`: `Multi-Location Inventory` (when `tracking_inventory_by_location`) [`real_name`]; `effect`: `limits_role_access_to_item`; `value_examples`: `role_limited_to_location_one_items`; `use_instead`: `locations_sublist` (when `tracking_inventory_per_location`); `ui_effect`: `new_option_creates_record` |
| `parent` | select | Subitem of | no | no |  | `purpose`: `parent_item_of_subitem` |
| `printitems` | checkbox | Print Items | no | no |  | `purpose`: `print_member_items`; `true_means`: `member_lines_on_sales_and_purchase_forms`; `false_means`: `group_description_and_amount_only`; `caveat`: `assemblies_never_on_purchase_forms`; `doc_inconsistency`: `says_members_never_shown_on_printed_forms` |
| `subsidiary` | select | Subsidiary | no | no |  | `purpose`: `item_subsidiaries`; `value_kind`: `multi_select`; `constraint`: `item_usable_only_on_its_subsidiaries_transactions`; `value_examples`: `only_wolfe_us_items_on_wolfe_us_orders` |
| `upccode` | text | UPC Code | no | no |  | `purpose`: `upc_code`; `max_length`: `999` characters; `ui_effect`: `item_labels_print_upc_barcode`; `default_behavior`: `labels_print_item_name_code128` (when `blank`) |
| `vendorname` | text | Vendor Name/Code | no | no |  | `purpose`: `vendor_item_name`; `max_length`: `60` characters; `displayed_on`: `purchase_forms_item_column` |
| `createddate` | datetime | Date Created | no | no |  |  |
| `externalid` | text | ExternalId | no | no |  |  |
| `lastmodifieddate` | datetime | Last Modified | no | no |  |  |

## Sublist `hierarchyversions` (Hierarchy Versions)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `hierarchynode` | select | Hierarchy Node | no |  |  |
| `isincluded` | checkbox | Included In Version | no |  |  |

## Sublist `member` (Members)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `item` | select | Item | yes |  |  |
| `memberdescr` | textarea | Description | no |  |  |
| `memberunit` | text | Units | no |  |  |
| `quantity` | float | Quantity | no |  |  |
| `taxschedule` | select | Schedule | no |  |  |
| `vsoedeferral` | select | Deferral | no |  |  |
| `vsoedelivered` | checkbox | Default as Delivered | no |  |  |
| `vsoepermitdiscount` | select | Permit Discount | no |  |  |
| `weight` | text |  | no |  |  |

## Sublist `translations` (Item Translation)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `displayname` | text | Display Name | no |  |  |
| `featureddescription` | textarea | Featured Description | no |  |  |
| `language` | text | Language | no |  |  |
| `locale` | text |  | no |  |  |
| `nopricemessage` | textarea | No Price Message | no |  |  |
| `outofstockmessage` | textarea | Out Of Stock Message | no |  |  |
| `pagetitle` | textarea | Page Title | no |  |  |
| `salesdescription` | textarea | Sales Description | no |  |  |
| `storedescription` | textarea | Store Description | no |  |  |
| `storedetaileddescription` | textarea | Detailed Description | no |  |  |
| `storedisplayname` | text | Store Display Name | no |  |  |

## Tabs

| Tab ID | Label |
|---|---|
| `basic` | Basic |
| `custom` | Custom |
| `history` | History |
| `inventory` | Inventory |
| `inventorynumbers` | Inventory Numbers |
| `members` | Members |
| `pricing` | Pricing |
| `relitems` | Related Items |
| `specials` | Specials |
| `store` | Store |
| `translation` | Translation |
| `vsoepricing` | Rev Rec / Amort |
| `workflow` | Workflow |

## Suspected source errors

> [!SOURCE-ERROR] `item_record_help_describes_label_printing` (on `itemtype`)
<!-- source-error: record:itemgroup | itemtype | item_record_help_describes_label_printing -->

> [!SOURCE-ERROR] `says_members_never_shown_on_printed_forms` (on `printitems`)
<!-- source-error: record:itemgroup | printitems | says_members_never_shown_on_printed_forms -->
