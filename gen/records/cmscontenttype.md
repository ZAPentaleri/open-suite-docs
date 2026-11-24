# CMS Content Type (`cmscontenttype`)
<!-- id: record:cmscontenttype -->

| Fact | Value |
|---|---|
| Internal ID | `cmscontenttype` |
| Category | website |
| Client scripts | no |
| Server scripts | yes |
| Searchable | yes |
| Tags | `custom_fields_supported`, `no_attach`, `no_copy`, `no_transform`, `supports_create`, `supports_search`, `supports_update` |

## Record facts

- `requires_feature`: `Site Management Tools` [`real_name`]
- `script_types`: `server`
- `script_functions`: `create`, `update`, `search`
- `not_supported`: `copy`, `attach`, `transform`
- `associated_with`: `custom_record`
- `doc_inconsistency`: `page_gives_internal_id_cmspage`
- `purpose`: `website_content_types`
- `configured_by`: `enable_features_web_presence_subtab` [`site_management_tools`]
- `doc_inconsistency`: `intro_and_search_sample_describe_cms_page`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `customrecordid` | select | Custom Record | yes |  |  | `purpose`: `associated_custom_record` |
| `description` | textarea | Description | no |  |  | `purpose`: `description` |
| `iconimagepath` | richtext | Icon Image Path | no |  |  | `purpose`: `icon_image_path`; `format`: `svg_relative_path` |
| `internalid` | integer | Internal ID | no |  |  | `purpose`: `internal_id`; `value_source`: `system_assigned` |
| `label` | richtext | Label | yes |  |  | `purpose`: `content_type_label`; `displayed_on`: `site_management_tools` |
| `name` | richtext | Name | yes |  |  | `purpose`: `record_name` |
| `customrecordscriptid` | richtext | Custom Record Script Id | no |  |  |  |
| `externalid` | text | ExternalId | no |  |  |  |
| `id` | integer | Internal ID | no |  |  |  |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `customrecordid` | integer | Custom Record Id |  |
| `customrecordscriptid` | richtext | Custom Record Script Id |  |
| `description` | textarea | Description |  |
| `externalid` | select | External ID |  |
| `externalidstring` | text | External ID (Text) |  |
| `iconimagepath` | richtext | Icon Image Path |  |
| `id` | integer | Internal ID |  |
| `internalid` | select | Internal ID |  |
| `internalidnumber` | integer | Internal ID (Number) |  |
| `label` | richtext | Label |  |
| `name` | richtext | Name |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `customrecordid` | integer | Custom Record Id |  |
| `customrecordscriptid` | richtext | Custom Record Script Id |  |
| `description` | textarea | Description |  |
| `externalid` | select | External ID |  |
| `iconimagepath` | richtext | Icon Image Path |  |
| `id` | integer | Internal ID |  |
| `internalid` | integer | Internal ID |  |
| `label` | richtext | Label |  |
| `name` | richtext | Name |  |

## Suspected source errors

> [!SOURCE-ERROR] `page_gives_internal_id_cmspage`
<!-- source-error: record:cmscontenttype | - | page_gives_internal_id_cmspage -->

> [!SOURCE-ERROR] `intro_and_search_sample_describe_cms_page`
<!-- source-error: record:cmscontenttype | - | intro_and_search_sample_describe_cms_page -->
