# Campaign Subscription (`campaignsubscription`)
<!-- id: record:campaignsubscription -->

| Fact | Value |
|---|---|
| Internal ID | `campaignsubscription` |
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
| `externaldescription` | textarea | External Description | no |  | marked_new | `purpose`: `subscription_center_description` |
| `externalname` | text | External Name | no |  | marked_new | `purpose`: `subscription_center_name` |
| `isinactive` | checkbox | Inactive | no |  | marked_new | `purpose`: `inactivate_record` |
| `name` | text | Name | yes |  | marked_new | `purpose`: `campaign_metric_name` |
| `subscribedbydefault` | checkbox | Subscribed by Default | no |  | marked_new | `purpose`: `subscribe_entities_by_default`; `effect`: `subscribes_new_and_existing_entities`; `requires`: `notice_and_consent`; `caveat`: `unsubscribed_entities_must_resubscribe_manually` |
| `externalid` | text | External ID | no |  | marked_new |  |
| `id` | integer | Internal ID | no |  | marked_new |  |
