# CMS Content (`cmscontent`)
<!-- id: record:cmscontent -->

| Fact | Value |
|---|---|
| Internal ID | `cmscontent` |
| Category | website |
| Client scripts | no |
| Server scripts | yes |
| Searchable | yes |
| Tags | `custom_fields_supported`, `no_attach`, `no_copy`, `no_transform`, `supports_create`, `supports_search`, `supports_update` |

## Record facts

- `script_types`: `server`
- `script_functions`: `create`, `update`, `search`
- `requires_feature`: `Site Management Tools` [`real_name`]
- `script_functions`: `create`, `update`, `search`
- `not_supported`: `copy`, `attach`, `transform`
- `purpose`: `website_text_and_images`
- `configured_by`: `enable_features_web_presence_subtab` [`site_management_tools`]

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `areaname` | text | Area Name | yes |  |  | `purpose`: `page_area_name`; `value_source`: `template_data_cms_area_attribute` |
| `changeurl` | text | Change URL | no |  |  | `purpose`: `url_where_content_last_changed`; `caveat`: `content_may_display_on_other_urls`; `value_examples`: `page_type_global_content_on_many_pages` |
| `cmscontenttype` | select | Content Type | yes |  |  | `purpose`: `cms_content_type`; `allowed_values`: `CMS_IMAGE`, `CMS_HTML`, `CMS_MERCHZONE`, `CMS_TEXT` [`core_types`]; `caveat`: `custom_content_types_also_possible`; `allowed_values_listed_in`: `cms_content_type_records` |
| `description` | textarea | Description | no |  |  | `purpose`: `description` |
| `enddate` | datetime | End Date | no |  |  | `purpose`: `visibility_end`; `effect`: `content_hidden_after_end`; `default_behavior`: `never_expires` (when `blank`) |
| `global` | text | Global Context | no |  |  | `purpose`: `all_pages_scope`; `value_examples`: `*` |
| `internalid` | integer | Internal ID | no |  |  | `purpose`: `internal_id`; `value_source`: `system_assigned` |
| `matchtype` | text | Match Type | yes |  |  | `purpose`: `match_type`; `caveat`: `unused_by_smt_version_3` |
| `name` | text | Name | no |  |  | `purpose`: `record_name` |
| `pagetype` | text | Page Type Context | no |  |  | `purpose`: `page_type_scope`; `options`: `Product Details`, `facet-browse`, `cms-landing-page`; `option_meanings`: `product_details:product_detail_page`, `facet_browse:facet_or_category_page`, `cms_landing:landing_page_type`; `comparable_to`: `smt_this_page_area_scope` |
| `path` | text | Path Context | no |  |  | `purpose`: `page_url_for_this_page_scope` |
| `settings` | integer | Settings | no |  |  | `purpose`: `content_custom_record_id`; `effect`: `links_content_instance_custom_record` |
| `site` | select | Site | yes |  |  | `purpose`: `site` |
| `startdate` | datetime | Start Date | no |  |  | `purpose`: `visibility_start`; `effect`: `content_hidden_until_start` |
| `template` | text | Template | yes |  |  | `purpose`: `render_template`; `recommendation`: `set_to_default` |
| `createdby` | select | Created By | no |  |  |  |
| `createddate` | date | Date Created | no |  |  |  |
| `externalid` | text | ExternalId | no |  |  |  |
| `hidden` | checkbox | Hidden | no |  |  |  |
| `id` | integer | Internal ID | no |  |  |  |
| `lastmodifiedby` | select | Modified By | no |  |  |  |
| `lastmodifieddate` | date | Last Modified | no |  |  |  |
| `matchcount` | integer | Match Count | no |  |  |  |
| `sequencenumber` | integer | Sequence Number | no |  |  |  |

## Sublist `tags` (CMS Content Tag)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `name` | text | Name | no |  |  |
