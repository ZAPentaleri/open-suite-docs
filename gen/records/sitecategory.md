# Site Category (`sitecategory`)
<!-- id: record:sitecategory -->

| Fact | Value |
|---|---|
| Internal ID | `sitecategory` |
| Category | website |
| Client scripts | yes |
| Server scripts | yes |
| Searchable | yes |
| Tags | `scripting_full` |

## Record facts

- `script_types`: `client`, `server`
- `script_functions`: `create`, `update`, `copy`, `delete`, `search`
- `requires_feature`: `Site Builder Web Store`, `Site Builder Website` [`real_name`]
- `not_supported`: `transform`
- `not_scriptable`: `tag_subs_machine_sublist`
- `read_only_fields`: `ctype`
- `format`: `html_paragraph_tags` [`description`, `storedetaileddescription`]
- `purpose`: `refine_website_site_category`
- `requires_feature`: `Advanced Site Customization`, `Multiple Web Sites`, `Upsell Manager`, `URL Component Aliases`, `Multi-Language`, `SuiteBundler`, `File Cabinet`, `SuiteCloud Development Framework`, `Document Publishing`, `Multi-Location Inventory` (when `using_all_fields`) [`real_name`]
- `not_scriptable`: `audvendor`, `audslctrole`, `audslctextrole`, `audpartner`, `audgroup`, `audemployee`, `auddepartment`, `audcustomer`, `allvendors`, `allroles`, `allpartners`, `allemployees`, `allcustomers` [`audience_sublist_fields`]
- `value_examples`: `description_p_tags`
- `allowed_values`: `INVTITEM` [`presentationitem_itemtype`]

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `categorylistlayout` | select | Category List Layout | no | no | marked_new | `purpose`: `subcategory_layout` |
| `correlateditemslistlayout` | select | Upsell Items List Layout | no | no | marked_new | `purpose`: `upsell_list_layout` |
| `description` | textarea | Brief Description | no | no | marked_new | `purpose`: `category_description`; `max_length`: `999` characters; `accepts_forms`: `text`, `basic_html` |
| `excludefromsitemap` | checkbox | Exclude from Sitemap | no | no | marked_new | `purpose`: `exclude_from_sitemap` |
| `isinactive` | checkbox | Inactive | no | no | marked_new | `purpose`: `inactivate_category`; `can`: `reactivate_any_time` |
| `isonline` | checkbox | Display in Website | no | no | marked_new | `purpose`: `display_in_web_site`; `configured_by`: `audience_subtab` [`intranet`] |
| `itemid` | text | Category | yes | no | marked_new | `purpose`: `category_name`; `displayed_on`: `web_store` (when `isonline`) |
| `itemlistlayout` | select | Item List Layout | no | no | marked_new | `purpose`: `item_list_layout` |
| `metataghtml` | textarea | Meta Tag HTML | no | no | marked_new | `purpose`: `meta_tag_html` |
| `pagetitle` | textarea | Page Title | no | no | marked_new | `purpose`: `page_title`; `displayed_on`: `browser_title` |
| `parentcategory` | select | Subcategory of | yes | no | marked_new | `purpose`: `parent_category`; `use_when`: `publishing_saved_search_results` |
| `relateditemslistlayout` | select | Related Items List Layout | no | no | marked_new | `purpose`: `related_items_list_layout` |
| `searchkeywords` | textarea | Search Keywords | no | no | marked_new | `purpose`: `search_keywords`; `caveat`: `weighted_equal_to_category_name` |
| `sitemappriority` | select | Sitemap Priority | no | no | marked_new | `purpose`: `sitemap_priority`; `allowed_range`: `0.0`, `1.0`; `default_value`: `Auto` |
| `storedetaileddescription` | textarea | Detailed Description | no | no | marked_new | `purpose`: `detailed_description`; `max_length`: `4000` characters; `accepts_forms`: `text`, `basic_html` |
| `storedisplayimage` | select | Image | no | no | marked_new | `purpose`: `category_image` |
| `storedisplaythumbnail` | select | Thumbnail | no | no | marked_new | `purpose`: `category_thumbnail` |
| `urlcomponent` | textarea | URL Component | no | no | marked_new | `purpose`: `url_component`; `default_behavior`: `system_identifiers_in_url` (when `blank`) |
| `website` | select | Site | yes | no | marked_new | `purpose`: `category_web_site`; `requires_feature`: `Multiple Sites` [`real_name`] |
| `ctype` | text | Category Type | no | no | marked_new |  |
| `externalid` | text | External ID | no | no | marked_new |  |
| `internalid` | integer | Internal ID | no | no | marked_new |  |

## Sublist `presentationitem` (Items)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `baseprice` | currency | Base Price | no | marked_new |  |
| `description` | textarea | Item Description | no | marked_new |  |
| `item` | select | Item | yes | marked_new |  |
| `itemtype` | select | Type | yes | marked_new |  |
| `onlineprice` | currency | Online Price | no | marked_new |  |
| `presentationitem` | select | Item | no | marked_new |  |

## Sublist `translations` (Translation)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `description` | textarea | Brief Description | no | marked_new |  |
| `displayname` | text | Name | no | marked_new |  |
| `language` | text | Language | no | marked_new |  |
| `locale` | text |  | no | marked_new |  |
| `pagetitle` | textarea | Page Title | no | marked_new |  |
| `storedetaileddescription` | textarea | Detailed Description | no | marked_new |  |
