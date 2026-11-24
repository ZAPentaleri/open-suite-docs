# CMS Page (`cmspage`)
<!-- id: record:cmspage -->

| Fact | Value |
|---|---|
| Internal ID | `cmspage` |
| Category | website |
| Client scripts | no |
| Server scripts | yes |
| Searchable | yes |
| Tags | `no_attach`, `no_copy`, `no_transform`, `supports_create`, `supports_search`, `supports_update` |

## Record facts

- `script_types`: `server`
- `script_functions`: `create`, `update`, `search`
- `requires_feature`: `Site Management Tools` [`real_name`]
- `script_functions`: `create`, `update`, `search`
- `not_supported`: `copy`, `attach`, `transform`
- `landing_page_type`: `1` [`pagetype`]
- `purpose`: `website_content_pages`
- `configured_by`: `enable_features_web_presence_subtab` [`site_management_tools`]
- `value_examples`: `search_pages_by_sca_site_name_site_pagetype_url`
- `script_types`: `server`
- `script_functions`: `create`, `update`, `search`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `addtohead` | textarea | Addition to <head> | no |  |  | `purpose`: `html_added_to_head` |
| `enddate` | datetime | End Date | no |  |  | `purpose`: `visibility_end`; `effect`: `content_hidden_after_end`; `default_behavior`: `never_expires` (when `blank`) |
| `internalid` | integer | Internal ID | no |  |  | `purpose`: `internal_id`; `value_source`: `system_assigned` |
| `metadescription` | textarea | Meta Description | no |  |  | `purpose`: `meta_description`; `not_displayed_on`: `page_body` |
| `metakeywords` | textarea | Meta Keywords | no |  |  | `purpose`: `meta_keywords`; `not_displayed_on`: `page_body` |
| `name` | text | Name | yes |  |  | `purpose`: `record_name` |
| `pageheader` | textarea | Page Heading | no |  |  | `purpose`: `page_heading`; `requires`: `template_supports_heading` |
| `pagetitle` | textarea | Page Title | no |  |  | `purpose`: `page_title`; `displayed_on`: `browser_title_bar` |
| `pagetype` | select | Type | no |  |  | `purpose`: `page_type_scope`; `options`: `Product Details`, `facet-browse`, `cms-landing-page`; `option_meanings`: `product_details:product_detail_page`, `facet_browse:facet_or_category_page`, `cms_landing:landing_page_type`; `comparable_to`: `smt_this_page_area_scope` |
| `site` | select | Site | yes |  |  | `purpose`: `site` |
| `startdate` | datetime | Start Date | no |  |  | `purpose`: `visibility_start`; `effect`: `content_hidden_until_start` |
| `template` | text | Template | yes |  |  | `purpose`: `render_template`; `recommendation`: `set_to_default` |
| `url` | text | URL | yes |  |  | `purpose`: `page_url` |
| `createdby` | select | Created By | no |  |  |  |
| `createddate` | date | Date Created | no |  |  |  |
| `externalid` | text | External Id | no |  |  |  |
| `id` | integer | Internal ID | no |  |  |  |
| `lastmodifiedby` | select | Modified By | no |  |  |  |
| `lastmodifieddate` | date | Last Modified | no |  |  |  |

## Sublist `tags` (CMS Page Tag)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `name` | text | Name | no |  |  |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `addtohead` | textarea | Addition to <head> |  |
| `createdby` | select | Created By |  |
| `createddate` | date | Date Created |  |
| `deletedasofversion` | integer | Deleted As Of Version |  |
| `enddate` | datetime | End Date |  |
| `externalid` | select | External ID |  |
| `externalidstring` | text | External ID (Text) |  |
| `id` | integer | Internal ID |  |
| `internalid` | select | Internal ID |  |
| `internalidnumber` | integer | Internal ID (Number) |  |
| `lastmodifiedby` | select | Modified By |  |
| `lastmodifieddate` | date | Last Modified |  |
| `metadescription` | text | Meta Description |  |
| `metakeywords` | text | Meta Keywords |  |
| `name` | text | Name |  |
| `pageheader` | textarea | Page Heading |  |
| `pagetitle` | textarea | Page Title |  |
| `pagetype` | select | Type |  |
| `site` | select | Site |  |
| `startdate` | datetime | Start Date |  |
| `template` | text | Template |  |
| `url` | text | URL |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `addtohead` | textarea | Addition to <head> |  |
| `createdby` | select | Created By |  |
| `createddate` | date | Date Created |  |
| `deletedasofversion` | integer | Deleted As Of Version |  |
| `enddate` | datetime | End Date |  |
| `externalid` | select | External ID |  |
| `id` | integer | Internal ID |  |
| `internalid` | select | Internal ID |  |
| `lastmodifiedby` | select | Modified By |  |
| `lastmodifieddate` | date | Last Modified |  |
| `metadescription` | text | Meta Description |  |
| `metakeywords` | text | Meta Keywords |  |
| `name` | text | Name |  |
| `pageheader` | textarea | Page Heading |  |
| `pagetitle` | textarea | Page Title |  |
| `pagetype` | select | Type |  |
| `site` | select | Site |  |
| `startdate` | datetime | Start Date |  |
| `template` | text | Template |  |
| `url` | text | URL |  |
