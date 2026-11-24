# Customer Message (`customermessage`)
<!-- id: record:customermessage -->

| Fact | Value |
|---|---|
| Internal ID | `customermessage` |
| Category | list |
| Client scripts | yes |
| Server scripts | yes |
| Searchable | yes |
| Tags | `custom_fields_supported`, `scripting_full` |

## Record facts

- `script_types`: `client`, `server`
- `script_functions`: `create`, `update`, `copy`, `delete`, `search`
- `purpose`: `standard_messages_selectable_on_transactions`
- `value_examples`: `thank_you_for_your_order_on_invoice`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `description` | textarea | Long Text | no | no |  | `purpose`: `list_item_description` |
| `isinactive` | checkbox | Inactive | no | no |  | `purpose`: `inactivate_record`; `can`: `reactivate_any_time`; `doc_inconsistency`: `says_removes_all_references` |
| `name` | text | Customer Message | yes | no |  | `purpose`: `record_name`; `displayed_on`: `lists` |
| `preferred` | checkbox | Preferred | no | no |  | `purpose`: `preferred_term_or_message`; `true_means`: `default_on_sales_transactions`; `caveat`: `customer_terms_override_preferred` |
| `externalid` | text | ExternalId | no | no |  |  |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `description` | text | Description |  |
| `externalid` | select | External ID |  |
| `externalidstring` | text | External ID (Text) |  |
| `internalid` | select | Internal ID |  |
| `internalidnumber` | integer | Internal ID (Number) |  |
| `isinactive` | checkbox | Inactive |  |
| `name` | text | Name |  |
| `preferred` | checkbox | Preferred |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `description` | text | Description |  |
| `externalid` | select | External ID |  |
| `internalid` | select | Internal ID |  |
| `isinactive` | checkbox | Inactive |  |
| `name` | text | Name |  |
| `preferred` | text | Preferred |  |

## Search joins

| Join ID | Label | Joined record |
|---|---|---|
| `user` | User | Employee |

## Suspected source errors

> [!SOURCE-ERROR] `says_removes_all_references` (on `isinactive`)
<!-- source-error: record:customermessage | isinactive | says_removes_all_references -->
