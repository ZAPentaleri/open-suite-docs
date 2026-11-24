# Payment Item (`paymentitem`)
<!-- id: record:paymentitem -->

| Fact | Value |
|---|---|
| Internal ID | `paymentitem` |
| Category | item |
| Client scripts | yes |
| Server scripts | yes |
| Searchable | yes |
| Search data via | Item |
| Tags | `custom_fields_supported`, `scripting_full` |

## Record facts

- `script_types`: `client`, `server`
- `script_functions`: `create`, `update`, `copy`, `delete`, `search`
- `purpose`: `payment_types_shown_separately_on_invoices`
- `value_examples`: `down_payment`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `account` | select |  | no | no |  | `purpose`: `deposit_to_account` |
| `availabletopartners` | checkbox | Available to Adv. Partners | no | no |  | `purpose`: `visible_to_advanced_partner_center`; `applies_to`: `advanced_partner_center_roles`, `custom_roles_with_item_access` |
| `class` | select | Class | no | yes |  | `purpose`: `item_class`; `allowed_values_listed_in`: `class_records`; `ui_effect`: `new_option_creates_record` |
| `customform` | select | Custom Form | no | no |  | `purpose`: `entry_form`; `allowed_values_listed_in`: `standard_and_custom_entry_forms`; `ui_effect`: `new_option_creates_record` |
| `department` | select | Department | no | yes |  | `purpose`: `item_department`; `allowed_values_listed_in`: `department_records`; `ui_effect`: `new_option_creates_record` |
| `description` | textarea | Description | no | no |  | `purpose`: `item_description`; `displayed_on`: `sales_forms_description_column`; `ui_effect`: `text_enhance_button_may_appear` |
| `displayname` | text | Display Name/Code | no | no |  | `purpose`: `display_name`; `max_length`: `250` characters; `displayed_on`: `sales_forms_item_column` (when `basic_printing`); `default_behavior`: `item_name_on_printed_forms` (when `blank`) |
| `includechildren` | checkbox | Include Children | no | no |  | `purpose`: `share_item_with_child_subsidiaries`; `constraint`: `item_settings_compatible_across_subsidiaries`; `value_examples`: `shared_items_need_shared_income_and_asset_accounts` |
| `isinactive` | checkbox | Inactive | no | no |  | `purpose`: `inactivate_item`; `ui_effect`: `hidden_from_lists_unless_show_all` |
| `issueproduct` | select | Product Name | no | no |  | `purpose`: `issue_management_product`; `allowed_values_listed_in`: `issue_product_records`; `value_kind`: `product_with_versions_and_builds` |
| `itemid` | text | Item Name/Number | yes | no |  | `purpose`: `item_name`; `max_length`: `250` characters; `displayed_on`: `transaction_lists`; `displayed_on`: `sales_forms_item_column` (when `no_display_name`); `displayed_on`: `purchase_forms_item_column` (when `no_vendor_name`) |
| `itemtype` | text |  | no | no |  | `purpose`: `item_type_for_label_printing`; `special_values`: `All`; `doc_inconsistency`: `item_record_help_describes_label_printing` |
| `location` | select | Location | no | yes |  | `purpose`: `item_location`; `requires_feature`: `Multi-Location Inventory` (when `tracking_inventory_by_location`) [`real_name`]; `effect`: `limits_role_access_to_item`; `value_examples`: `role_limited_to_location_one_items`; `use_instead`: `locations_sublist` (when `tracking_inventory_per_location`); `ui_effect`: `new_option_creates_record` |
| `paymentmethod` | select | Payment Method | no | no |  | `purpose`: `payment_method_reference`; `caveat`: `reference_only_no_payment_initiated`; `allowed_values_listed_in`: `accounting_list_payment_method` |
| `subsidiary` | select | Subsidiary | no | no |  | `purpose`: `item_subsidiaries`; `value_kind`: `multi_select`; `constraint`: `item_usable_only_on_its_subsidiaries_transactions`; `value_examples`: `only_wolfe_us_items_on_wolfe_us_orders` |
| `undepfunds` | radio | Group with Undeposited Funds | no | no |  | `purpose`: `undeposited_funds_or_account`; `options`: `Group with Undeposited Funds`, `Deposit To` |
| `createddate` | datetime | Date Created | no | no |  |  |
| `externalid` | text | ExternalId | no | no |  |  |
| `lastmodifieddate` | datetime | Last Modified | no | no |  |  |

## Sublist `translations` (Translations)

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
| `translation` | Translation |
| `workflow` | Workflow |

## Suspected source errors

> [!SOURCE-ERROR] `item_record_help_describes_label_printing` (on `itemtype`)
<!-- source-error: record:paymentitem | itemtype | item_record_help_describes_label_printing -->
