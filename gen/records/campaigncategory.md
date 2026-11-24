# Campaign Category (`campaigncategory`)
<!-- id: record:campaigncategory -->

| Fact | Value |
|---|---|
| Internal ID | `campaigncategory` |
| Category | marketing |
| Client scripts | yes |
| Server scripts | yes |
| Tags | `supports_create`, `supports_delete`, `supports_read`, `supports_update` |

## Record facts

- `script_types`: `client`, `server`
- `not_supported`: `copy`, `search`, `transform`
- `script_functions`: `create`, `read`, `update`, `delete`
- `requires_feature`: `Marketing Automation` [`real_name`]
- `requires_feature`: `Customer Relationship Management` [`real_name`]
- `subrecords`: `leadsource`, `parent`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `description` | textarea | Description | no |  | marked_new | `purpose`: `description` |
| `isexternal` | checkbox | Available Externally | no |  | marked_new | `purpose`: `selectable_on_online_forms` |
| `isinactive` | checkbox | Inactive | no |  | marked_new | `purpose`: `inactivate_record` |
| `leadsource` | select | Default Lead Source | no |  | marked_new | `purpose`: `default_lead_source`; `effect`: `sets_lead_source_from_online_form` (when `no_other_lead_source`) |
| `name` | text | Name | yes |  | marked_new | `purpose`: `campaign_metric_name` |
| `parent` | select | Parent | no |  | marked_new | `purpose`: `parent_category` |
| `externalid` | text | External ID | no |  | marked_new |  |
| `id` | integer | Internal ID | no |  | marked_new |  |
