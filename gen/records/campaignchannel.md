# Campaign Channel (`campaignchannel`)
<!-- id: record:campaignchannel -->

| Fact | Value |
|---|---|
| Internal ID | `campaignchannel` |
| Category | marketing |
| Client scripts | yes |
| Server scripts | yes |
| Tags | `supports_create`, `supports_delete`, `supports_read`, `supports_update` |

## Record facts

- `script_types`: `client`, `server`
- `not_supported`: `copy`, `search`, `transform`
- `script_functions`: `create`, `read`, `update`, `delete`
- `requires_feature`: `Marketing Automation` [`real_name`]
- `subrecords`: `eventtype`
- `not_updatable_fields`: `eventType`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `description` | textarea | Description | no |  | marked_new | `purpose`: `description` |
| `eventtype` | select | Type | no |  | marked_new | `purpose`: `channel_event_type`; `effect`: `limits_event_types_using_channel`; `value_examples`: `email_channels_only_on_email_subtab_events` |
| `isinactive` | checkbox | Inactive | no |  | marked_new | `purpose`: `inactivate_record` |
| `name` | text | Name | yes |  | marked_new | `purpose`: `campaign_metric_name` |
| `externalid` | text | External ID | no |  | marked_new |  |
| `id` | integer | Internal ID | no |  | marked_new |  |
