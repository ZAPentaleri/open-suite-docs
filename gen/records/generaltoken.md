# General Token (`generaltoken`)
<!-- id: record:generaltoken -->

| Fact | Value |
|---|---|
| Internal ID | `generaltoken` |
| Category | list |
| Client scripts | yes |
| Server scripts | yes |
| Tags | `no_copy`, `no_transform` |

## Record facts

- `requires_feature`: `Payment Instruments` [`real_name`]
- `script_types`: `client`, `server`
- `script_functions`: `create`, `read`, `update`, `delete`, `search`
- `not_supported`: `copy`, `transform`
- `masked_after_save`: `token`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `entity` | select | Customer | yes |  |  |  |
| `externalid` | text | External Id | no |  |  |  |
| `id` | integer | Internal ID | no |  |  |  |
| `instrumenttype` | select | Type | yes |  |  |  |
| `isdefault` | checkbox | Default | no |  |  |  |
| `isinactive` | checkbox | Inactive | no |  |  |  |
| `mask` | text | Mask | no |  |  |  |
| `memo` | text | Memo | no |  |  |  |
| `paymentmethod` | select | Payment Method | yes |  |  |  |
| `preserveonfile` | checkbox | Preserve on File | no |  |  |  |
| `state` | select | State | yes |  |  |  |
| `supportedoperations` | multiselect | Supported Operations | no |  |  |  |
| `token` | text | Token | no |  |  |  |
| `tokenexpirationdate` | mmyydate | Token Expiration Date | no |  |  |  |
| `tokenfamily` | select | Token Family | yes |  |  |  |
| `tokennamespace` | text | Token Namespace | no |  |  |  |
