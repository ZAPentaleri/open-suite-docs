# Campaign Offer (`campaignoffer`)
<!-- id: record:campaignoffer -->

| Fact | Value |
|---|---|
| Internal ID | `campaignoffer` |
| Category | marketing |
| Client scripts | yes |
| Server scripts | yes |
| Tags | `supports_create`, `supports_delete`, `supports_read`, `supports_update` |

## Record facts

- `script_types`: `client`, `server`
- `not_supported`: `copy`, `search`, `transform`
- `script_functions`: `create`, `read`, `update`, `delete`
- `requires_feature`: `Marketing Automation` [`real_name`]

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `description` | textarea | Description | no |  | marked_new | `purpose`: `description` |
| `isinactive` | checkbox | Inactive | no |  | marked_new | `purpose`: `inactivate_record` |
| `name` | text | Name | yes |  | marked_new | `purpose`: `campaign_metric_name` |
| `externalid` | text | External ID | no |  | marked_new |  |
| `id` | integer | Internal ID | no |  | marked_new |  |
