# Vendor Category (`vendorcategory`)
<!-- id: record:vendorcategory -->

| Fact | Value |
|---|---|
| Internal ID | `vendorcategory` |
| Category | list |
| Client scripts | yes |
| Server scripts | yes |
| Searchable | no |
| Tags | `custom_fields_supported`, `no_search` |

## Record facts

- `script_types`: `client`, `server`
- `doc_inconsistency`: `says_search_unavailable_and_fully_scriptable`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `isinactive` | checkbox | Inactive | no | no |  | `purpose`: `inactivate_record`; `can`: `reactivate_any_time`; `doc_inconsistency`: `says_removes_all_references` |
| `istaxagency` | checkbox | Tax Agency | no | no |  | `purpose`: `tax_agency_category`; `true_means`: `vendor_is_tax_agency` |
| `name` | text | Vendor | yes | no |  | `purpose`: `record_name`; `displayed_on`: `lists` |
| `externalid` | text | ExternalId | no | no |  |  |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `externalid` | select | External ID |  |
| `externalidstring` | text | External ID (Text) |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `externalid` | select | External ID |  |

## Suspected source errors

> [!SOURCE-ERROR] `says_search_unavailable_and_fully_scriptable`
<!-- source-error: record:vendorcategory | - | says_search_unavailable_and_fully_scriptable -->

> [!SOURCE-ERROR] `says_removes_all_references` (on `isinactive`)
<!-- source-error: record:vendorcategory | isinactive | says_removes_all_references -->
