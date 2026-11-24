# Commerce Category (`commercecategory`)
<!-- id: record:commercecategory -->

| Fact | Value |
|---|---|
| Internal ID | `commercecategory` |
| Category | website |
| Client scripts | no |
| Server scripts | yes |
| Tags | `custom_fields_supported`, `no_attach`, `no_copy`, `no_transform` |

## Record facts

- `script_types`: `server`
- `script_functions`: `create`, `update`, `search`
- `requires_feature`: `Commerce Category` [`real_name`]
- `script_functions`: `create`, `update`, `search`
- `not_supported`: `copy`, `attach`, `transform`
- `sublist_fields`: `customersegment`, `customersegmentsize`
- `purpose`: `hierarchy_of_categories_subcategories_products`
- `ui_location`: `commerce_content_management_catalog_and_categories`
- `configured_by`: `enable_features_web_presence_subtab` [`commerce_category`]
- `cannot`: `create_edit_or_delete_commerce_category_sublists`
- `value_examples`: `search_by_fullurl_and_siteid`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `addtohead` | richtext | Addition to <head> | no | no |  | `purpose`: `code_added_to_head`; `accepts_forms`: `html`, `css`, `javascript`; `displayed_on`: `category_page_head` |
| `catalog` | select | Catalog | yes | no |  | `purpose`: `catalog` |
| `description` | textarea | Description | no | no |  | `purpose`: `description` |
| `displayinsite` | select | Display in Web Site | no | no |  | `purpose`: `displayed_in_site` |
| `enddate` | datetime | End Date | no |  |  | `purpose`: `visibility_end`; `format`: `user_preference_date_time`; `effect`: `content_hidden_after_end`; `default_behavior`: `never_expires` (when `blank`) |
| `isinactive` | checkbox | Inactive | no | no |  | `purpose`: `inactivate_category`; `effect`: `hidden_and_unusable`; `cannot`: `inactivate_with_subcategories`; `cannot`: `assign_inactive_as_subcategory`; `ui_effect`: `omitted_from_categories_list_by_default`; `ui_effect`: `show_inactives_includes_in_list` |
| `metadescription` | text | Meta Description | no | no |  | `purpose`: `meta_description`; `displayed_on`: `search_engine_results` |
| `metakeywords` | text | Meta Keywords | no | no |  | `purpose`: `meta_keywords` |
| `name` | text | Name | yes | no |  | `purpose`: `record_name` |
| `pagebanner` | select | Page Banner | no | no |  | `purpose`: `category_banner_image`; `requires`: `template_displays_banner` |
| `pageheading` | text | Heading | no | no |  | `purpose`: `category_page_heading`; `requires`: `template_displays_heading` |
| `pagetitle` | text | Page Title | no | no |  | `purpose`: `page_title`; `effect`: `fills_title_element` |
| `primaryparent` | select | Primary Parent | no | no |  | `purpose`: `primary_parent_category`; `effect`: `builds_canonical_link` |
| `sequencenumber` | integer | Sequence Number | no | no |  | `purpose`: `display_order_in_smt`; `default_behavior`: `assigned_from_smt_manual_order` (when `blank`) |
| `sitemappriority` | select | Sitemap Priority | no | no |  | `purpose`: `sitemap_priority`; `allowed_range`: `0.0`, `1.0`; `default_value`: `0.5` (when `not_set`); `effect`: `fills_sitemap_priority_element` |
| `startdate` | datetime | Start Date | no |  |  | `purpose`: `visibility_start`; `format`: `user_preference_date_time`; `effect`: `content_hidden_until_start` |
| `thumbnail` | select | Thumbnail Image | no | no |  | `purpose`: `category_thumbnail_image` |
| `urlfragment` | text | URL Fragment | yes | no |  | `purpose`: `url_fragment`; `constraint`: `no_slashes` |
| `created` | date | Creation Date/Time | no | no |  |  |
| `externalid` | text | ExternalId | no | no |  |  |
| `lastmodified` | date | Last Modified | no | no |  |  |
| `lastmodifiedby` | select | Modified By | no | no |  |  |
| `origbinactive` | checkbox |  | no | no |  |  |
| `version` | integer | Version | no | no |  |  |

## Sublist `customersegments` (Customer Segment)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `customersegment` | select | Customer Segment | no |  |  |

## Sublist `items` (Items)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `item` | select | Items | no |  |  |
| `primarycategory` | checkbox | Primary Category | no |  |  |
| `sequencenumber` | integer | Sequence Number | no |  |  |

## Sublist `subcategories` (Subcategories)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `addtoheadoverride` | richtext | Addition to <head> Override | no |  |  |
| `descriptionoverride` | text | Description Override | no |  |  |
| `displayinsiteoverride` | select | Display in Web Site Override | no |  |  |
| `metadescriptionoverride` | text | Meta Description Override | no |  |  |
| `metakeywordsoverride` | text | Meta Keywords Override | no |  |  |
| `nameoverride` | text | Name Override | no |  |  |
| `pagebanneroverride` | select | Page Banner Override | no |  |  |
| `pageheadingoverride` | text | Heading Override | no |  |  |
| `pagetitleoverride` | text | Title Override | no |  |  |
| `sequencenumber` | integer | Sequence Number | no |  |  |
| `sitemappriorityoverride` | select | Sitemap Priority Override | no |  |  |
| `subcategory` | select | Subcategory | yes |  |  |
| `thumbnailoverride` | select | Thumbnail Image Override | no |  |  |
| `urlfragmentoverride` | text | URL Fragment Override | no |  |  |

## Sublist `tags` (Commerce Category Tag)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `name` | text | Name | no |  |  |

## Sublist `urls` (URLs)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `fullurl` | text | Full Url | no |  |  |
| `id` | integer | Internal ID | no |  |  |
| `idpath` | text | ID Path | no |  |  |
| `isprimary` | checkbox | Primary | no |  |  |
| `nestlevel` | integer | Level | no |  |  |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `addtohead` | text | Add to Head |  |
| `catalog` | select | Catalog |  |
| `created` | date | Creation Date/Time |  |
| `description` | textarea | Description |  |
| `displayinsite` | checkbox | Display in Web Site |  |
| `enddate` | datetime | End Date |  |
| `externalid` | select | External ID |  |
| `externalidstring` | text | External ID (Text) |  |
| `fullurl` | text | Full URL |  |
| `idpath` | text | ID Path |  |
| `internalid` | select | Internal ID |  |
| `internalidnumber` | integer | Internal ID (Number) |  |
| `isinactive` | checkbox | Inactive |  |
| `isprimaryurl` | checkbox | Is Primary URL |  |
| `item` | select | Item |  |
| `itemcategory` | select | Item Category ID |  |
| `itemdescription` | textarea | Item Description |  |
| `lastmodified` | date | Last Modified |  |
| `lastmodifiedby` | select | Modified By |  |
| `metadescription` | text | Meta Description |  |
| `metakeywords` | text | Meta Keywords |  |
| `name` | text | Name |  |
| `nestlevel` | integer | Nest Level |  |
| `pageheading` | text | Page Heading |  |
| `pagetitle` | text | Page Title |  |
| `primarycategory` | checkbox | Primary Category of Item |  |
| `primaryparent` | select | Primary Parent |  |
| `sequencenumber` | integer | Sequence Number |  |
| `site` | select | Site |  |
| `sitemappriority` | float | Sitemap Priority |  |
| `startdate` | datetime | Start Date |  |
| `subcatdisplayinsiteoverride` | checkbox | Subcategory Display In Site Override |  |
| `subcatid` | select | Subcategory ID |  |
| `subcatparent` | select | Subcategory Parent ID |  |
| `urlfragment` | text | URL |  |
| `version` | integer | Version |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `addtohead` | text | Add to Head |  |
| `catalog` | select | Catalog |  |
| `created` | date | Creation Date/Time |  |
| `description` | textarea | Description |  |
| `displayinsite` | checkbox | Display in Web Site |  |
| `enddate` | datetime | End Date |  |
| `externalid` | select | External ID |  |
| `fullurl` | text | Full URL |  |
| `idpath` | text | ID Path |  |
| `internalid` | select | Internal ID |  |
| `isinactive` | checkbox | Inactive |  |
| `isprimaryurl` | checkbox | Is Primary URL |  |
| `itemdescription` | textarea | Item Description |  |
| `itemid` | integer | Item ID |  |
| `itemsequencenumber` | integer | Item Sequence Number |  |
| `lastmodified` | date | Last Modified |  |
| `lastmodifiedby` | select | Modified By |  |
| `metadescription` | text | Meta Description |  |
| `metakeywords` | text | Meta Tag HTML |  |
| `name` | text | Name |  |
| `nestlevel` | integer | Nest Level |  |
| `pagebanner` | image | Category Page Banner |  |
| `pagebannerurl` | text | Category Page Banner URL |  |
| `pageheading` | text | Page Heading |  |
| `pagetitle` | text | Page Title |  |
| `primarycategory` | checkbox | Primary Category of Item |  |
| `primaryparent` | select | Primary Parent |  |
| `sequencenumber` | integer | Sequence Number |  |
| `sitemappriority` | float | Sitemap Priority |  |
| `startdate` | datetime | Start Date |  |
| `subcataddtoheadoverride` | text | Subcategory Add to Head Override |  |
| `subcatdescoverride` | text | Subcategory Description Override |  |
| `subcatdisplayinsiteoverride` | checkbox | Subcategory Display In Site Override |  |
| `subcatid` | integer | Subcategory ID |  |
| `subcatmetadescoverride` | text | Subcategory Meta Description Override |  |
| `subcatmetakeywordsoverride` | text | Subcategory Meta Keywords Override |  |
| `subcatnameoverride` | text | Subcategory Name Override |  |
| `subcatpagebanneroverride` | select | Subcategory Page Banner Override |  |
| `subcatpagebannerurloverride` | text | Subcategory Page Banner URL Override |  |
| `subcatpageheadingoverride` | text | Subcategory Page Heading Override |  |
| `subcatpagetitleoverride` | text | Subcategory Page Title Override |  |
| `subcatparent` | integer | Subcategory Parent ID |  |
| `subcatsequencenumber` | integer | Subcategory Sequence Number |  |
| `subcatthumbnailoverride` | select | Subcategory Thumbnail Override |  |
| `subcatthumbnailurloverride` | text | Subcategory Thumbnail URL Override |  |
| `subcaturlfragmentoverride` | text | Subcategory URL Fragment Override |  |
| `thumbnail` | image | Category Thumbnail |  |
| `thumbnailurl` | text | Category Thumbnail URL |  |
| `urlcommcat` | integer | URL Category |  |
| `urlcommcatassoc` | integer | URL Category Association |  |
| `urlfragment` | text | URL |  |
| `version` | integer | Version |  |
