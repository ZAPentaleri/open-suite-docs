# Automated Clearing House (`automatedclearinghouse`)
<!-- id: record:automatedclearinghouse -->

| Fact | Value |
|---|---|
| Internal ID | `automatedclearinghouse` |
| Category | list |
| Client scripts | yes |
| Server scripts | yes |
| Tags | `no_copy`, `no_transform` |

## Record facts

- `script_types`: `client`, `server`
- `requires_feature`: `Payment Instruments` [`real_name`]
- `requires`: `ach_payment_method`
- `script_functions`: `read`, `create`, `update`, `delete`, `search`
- `search_only_sublists`: `systemnotes`
- `not_updatable_fields`: `entity`, `mask`, `bankaccountnumber`, `routingnumber`, `accountownername`, `accounttype`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `accountownername` | text | Account owner name | no |  |  |  |
| `accounttype` | select | Account Type | no |  |  |  |
| `bankaccountnumber` | text | Bank account number | no |  |  |  |
| `bankname` | text | Bank Name | no |  |  |  |
| `customerconsent` | textarea | Customer Consent | no |  |  |  |
| `entity` | select | Customer | yes |  |  |  |
| `externalid` | text | External ID | no |  |  |  |
| `id` | integer | Internal ID | no |  |  |  |
| `instrumenttype` | select | Type | yes |  |  |  |
| `isdefault` | checkbox | Default | no |  |  |  |
| `isinactive` | checkbox | Inactive | no |  |  |  |
| `limit` | poscurrency | Limit | no |  |  |  |
| `mask` | text | Mask | yes |  |  |  |
| `memo` | text | Memo | no |  |  |  |
| `paymentmethod` | select | Payment Method | yes |  |  |  |
| `preserveonfile` | checkbox | Preserve on File | no |  |  |  |
| `routingnumber` | text | Routing Number | no |  |  |  |
| `state` | select | State | no |  |  |  |
| `supportedoperations` | multiselect | Supported Operations | no |  |  |  |
