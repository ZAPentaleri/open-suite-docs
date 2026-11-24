# Markup Item (`markupitem`)
<!-- id: record:markupitem -->

| Fact | Value |
|---|---|
| Internal ID | `markupitem` |
| Category | item |
| Client scripts | yes |
| Server scripts | yes |
| Searchable | yes |
| Search data via | Item |
| Tags | `custom_fields_supported`, `scripting_full` |

## Record facts

- `script_types`: `client`, `server`
- `script_functions`: `create`, `update`, `copy`, `delete`, `search`
- `purpose`: `additional_charge_without_affecting_inventory_valuation`
- `value_examples`: `rush_fee`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `account` | select |  | yes | no |  | `purpose`: `deposit_to_account` |
| `availabletopartners` | checkbox | Available to Adv. Partners | no | no |  | `purpose`: `visible_to_advanced_partner_center`; `applies_to`: `advanced_partner_center_roles`, `custom_roles_with_item_access` |
| `class` | select | Class | no | yes |  | `purpose`: `item_class`; `allowed_values_listed_in`: `class_records`; `ui_effect`: `new_option_creates_record` |
| `customform` | select | Custom Form | no | no |  | `purpose`: `entry_form`; `allowed_values_listed_in`: `standard_and_custom_entry_forms`; `ui_effect`: `new_option_creates_record` |
| `department` | select | Department | no | yes |  | `purpose`: `item_department`; `allowed_values_listed_in`: `department_records`; `ui_effect`: `new_option_creates_record` |
| `description` | textarea | Description | no | no |  | `purpose`: `item_description`; `displayed_on`: `sales_forms_description_column`; `ui_effect`: `text_enhance_button_may_appear` |
| `displayname` | text | Display Name/Code | no | no |  | `purpose`: `display_name`; `max_length`: `250` characters; `displayed_on`: `sales_forms_item_column` (when `basic_printing`); `default_behavior`: `item_name_on_printed_forms` (when `blank`) |
| `includechildren` | checkbox | Include Children | no | no |  | `purpose`: `share_item_with_child_subsidiaries`; `constraint`: `item_settings_compatible_across_subsidiaries`; `value_examples`: `shared_items_need_shared_income_and_asset_accounts` |
| `isinactive` | checkbox | Inactive | no | no |  | `purpose`: `inactivate_item`; `ui_effect`: `hidden_from_lists_unless_show_all` |
| `ispretax` | checkbox | Apply Before Sales Tax | no | no |  | `purpose`: `apply_before_tax`; `applies_to`: `sales_tax_countries`, `brazil`, `china`, `india`; `caveat`: `no_effect_for_vat_countries` (when `oneworld`) |
| `issueproduct` | select | Product | no | no |  | `purpose`: `issue_management_product`; `allowed_values_listed_in`: `issue_product_records`; `value_kind`: `product_with_versions_and_builds` |
| `itemid` | text | Item Name/Number | yes | no |  | `purpose`: `item_name`; `max_length`: `250` characters; `displayed_on`: `transaction_lists`; `displayed_on`: `sales_forms_item_column` (when `no_display_name`); `displayed_on`: `purchase_forms_item_column` (when `no_vendor_name`) |
| `itemtype` | text |  | no | no |  | `purpose`: `item_type_for_label_printing`; `special_values`: `All`; `doc_inconsistency`: `item_record_help_describes_label_printing` |
| `location` | select | Location | no | yes |  | `purpose`: `item_location`; `requires_feature`: `Multi-Location Inventory` (when `tracking_inventory_by_location`) [`real_name`]; `effect`: `limits_role_access_to_item`; `value_examples`: `role_limited_to_location_one_items`; `use_instead`: `locations_sublist` (when `tracking_inventory_per_location`); `ui_effect`: `new_option_creates_record` |
| `nonposting` | radio | Non-posting | no | no |  | `purpose`: `non_posting_discount`; `true_means`: `line_posts_net_of_discount`; `constraint`: `applies_to_previous_line_only`; `false_means`: `gross_revenue_plus_offsetting_discount_debit`; `not_supported_for`: `discount_for_purchase_items`; `value_examples`: `10_percent_non_posting_on_100_posts_90`, `10_percent_posting_on_100_posts_100_and_10_debit`; `requires`: `account_for_discount` (when `non_posting`) |
| `parent` | select | Subitem of | no | no |  | `purpose`: `parent_item_of_subitem` |
| `rate` | rate | Rate | yes | no |  | `purpose`: `discount_or_markup_rate`; `accepts_forms`: `percentage_with_sign`, `positive_amount` |
| `subsidiary` | select | Subsidiary | no | no |  | `purpose`: `item_subsidiaries`; `value_kind`: `multi_select`; `constraint`: `item_usable_only_on_its_subsidiaries_transactions`; `value_examples`: `only_wolfe_us_items_on_wolfe_us_orders` |
| `taxschedule` | select | Tax Schedule | no | no |  | `purpose`: `tax_schedule`; `allowed_values_listed_in`: `tax_schedule_records` |
| `upccode` | text | UPC Code | no | no |  | `purpose`: `upc_code`; `max_length`: `999` characters; `ui_effect`: `item_labels_print_upc_barcode`; `default_behavior`: `labels_print_item_name_code128` (when `blank`) |
| `vendorname` | text | Vendor Name/Code | no | no |  | `purpose`: `vendor_item_name`; `max_length`: `60` characters; `displayed_on`: `purchase_forms_item_column` |
| `createddate` | datetime | Date Created | no | no |  |  |
| `externalid` | text | ExternalId | no | no |  |  |
| `lastmodifieddate` | datetime | Last Modified | no | no |  |  |

## Sublist `translations` (Translation)

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
| `workflow` | Workflow |

## Suspected source errors

> [!SOURCE-ERROR] `item_record_help_describes_label_printing` (on `itemtype`)
<!-- source-error: record:markupitem | itemtype | item_record_help_describes_label_printing -->
