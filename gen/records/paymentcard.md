# Payment Card (`paymentcard`)
<!-- id: record:paymentcard -->

| Fact | Value |
|---|---|
| Internal ID | `paymentcard` |
| Category | list |
| Client scripts | yes |
| Server scripts | yes |
| Tags | `no_copy`, `no_transform` |

## Record facts

- `requires_feature`: `Payment Instruments` [`real_name`]
- `script_types`: `client`, `server`
- `script_functions`: `create`, `read`, `update`, `delete`, `search`
- `not_supported`: `copy`, `transform`
- `masked_after_save`: `cardnumber`
- `purpose`: `charge_shopper_without_shopper_action`
- `can`: `dynamic_mode_sets_other_required_fields`
- `effect`: `preserve_on_file_reuses_instrument_on_transactions`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `cardbrand` | select | Card Brand | yes |  |  |  |
| `cardnumber` | ccnumber | Payment Card Number | no |  |  |  |
| `cardtype` | select | Card Type | no |  |  |  |
| `entity` | select | Customer | yes |  |  |  |
| `expirationdate` | mmyydate | Expiration Date | yes |  |  |  |
| `externalid` | text | External Id | no |  |  |  |
| `id` | integer | Internal ID | no |  |  |  |
| `instrumenttype` | select | Type | yes |  |  |  |
| `isdefault` | checkbox | Default | no |  |  |  |
| `isinactive` | checkbox | Inactive | no |  |  |  |
| `issuenumber` | text | Issue No. | no |  |  |  |
| `mask` | text | Mask | yes |  |  |  |
| `memo` | text | Memo | no |  |  |  |
| `nameoncard` | text | Cardholder Name | no |  |  |  |
| `paymentmethod` | select | Payment Method | yes |  |  |  |
| `preserveonfile` | checkbox | Preserve on File | no |  |  |  |
| `state` | select | State | yes |  |  |  |
| `street` | text | Card Street | no |  |  |  |
| `validfromdate` | mmyydate | Valid From / Start Date (MM/YYYY) | no |  |  |  |
| `zipcode` | text | Card Zip Code | no |  |  |  |
