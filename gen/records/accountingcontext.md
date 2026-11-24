# Accounting Context (`accountingcontext`)
<!-- id: record:accountingcontext -->

| Fact | Value |
|---|---|
| Internal ID | `accountingcontext` |
| Category | list |
| Client scripts | yes |
| Server scripts | yes |
| Searchable | yes |
| Tags | `custom_fields_supported`, `supports_copy`, `supports_create`, `supports_delete`, `supports_read`, `supports_search`, `supports_update` |

## Record facts

- `script_types`: `client`, `server`
- `not_supported`: `transform`
- `requires`: `oneworld`
- `requires_permission`: `setup_company_full`
- `script_functions`: `read`, `create`, `delete`, `copy`, `search`, `edit`
- `not_supported`: `transform`
- `purpose`: `local_gaap_and_statutory_coa_context`
- `use_cases`: `work_in_local_gaap_context`, `secondary_book_reports`
- `ui_location`: `general_preferences_accounting_contexts_subtab`
- `shown_only_when`: `oneworld` [`accounting_contexts_subtab`]

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `name` | text | Name | yes | no |  | `purpose`: `region_name`; `value_kind`: `shipping_region_by_country_state_or_zip`; `doc_inconsistency`: `accounting_context_help_describes_shipping_region` |
| `externalid` | text | ExternalId | no | no |  |  |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `externalid` | select | External ID |  |
| `externalidstring` | text | External ID (Text) |  |
| `internalid` | select | Internal ID |  |
| `internalidnumber` | integer | Internal ID (Number) |  |
| `name` | text | Name |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `externalid` | select | External ID |  |
| `internalid` | select | Internal ID |  |
| `name` | text | Name |  |

## Suspected source errors

> [!SOURCE-ERROR] `accounting_context_help_describes_shipping_region` (on `name`)
<!-- source-error: record:accountingcontext | name | accounting_context_help_describes_shipping_region -->
