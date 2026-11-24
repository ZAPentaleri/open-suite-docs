# Tax Account (`taxacct`)
<!-- id: record:taxacct -->

| Fact | Value |
|---|---|
| Internal ID | `taxacct` |
| Category | list |
| Client scripts | yes |
| Server scripts | yes |
| Searchable | no |
| Tags | `custom_fields_supported`, `no_delete`, `no_search` |

## Record facts

- `script_types`: `client`, `server`
- `user_events_supported`: `false`
- `script_functions`: `create`, `copy`, `update`
- `not_supported`: `delete`
- `updatable_fields`: `description`, `externalid`, `isinactive`, `name`
- `recommendation`: `set_isinactive_instead_of_delete`
- `purpose`: `account_receiving_computed_indirect_tax`
- `ui_location`: `setup_accounting_tax_control_accounts`
- `displayed_on`: `accounts_list`
- `workaround`: `delete_associated_nexus` (when `removing_with_advanced_taxes`)
- `cannot`: `remove_without_advanced_taxes`
- `value_source`: `derived_from_nexus` [`country`, `read_only`]
- `caveat`: `same_country_for_all_without_advanced_taxes` [`country`]
- `max_length`: `50` characters [`description`]
- `raises_error`: `description_over_max_characters` (when `description_over_50`)
- `constraint`: `name_unique`
- `raises_error`: `record_already_exists` (when `duplicate_name_even_with_unique_externalid`)
- `editable_only_when`: `advanced_taxes` [`nexus`]
- `required_when`: `advanced_taxes` [`nexus`]
- `cannot`: `change_nexus_after_create`
- `value_source`: `set_automatically_without_advanced_taxes` [`nexus`]
- `caveat`: `advanced_taxes_always_on_in_oneworld`
- `required_when`: `country_allows_sales_and_purchase` [`taxaccttype`]
- `value_examples`: `uk:sales_or_purchase`, `us:sales_only`
- `cannot`: `change_taxaccttype_after_create`
- `value_source`: `set_automatically_when_one_type_allowed` [`taxaccttype`]
- `workaround`: `find_internal_id_in_accounts_list_with_show_internal_ids`
- `caveat`: `tax_control_accounts_page_does_not_show_internal_id`
- `displayed_on`: `accounts_list_as_other_current_liability` (when `sale_type`)
- `displayed_on`: `accounts_list_as_other_current_asset` (when `purchase_type`)
- `can`: `update_more_fields_when_handled_as_account` (refs `record:account`)

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `description` | text | Description | no | no |  | `purpose`: `description` |
| `isinactive` | checkbox | Inactive | no | no |  | `purpose`: `inactivate_account`; `ui_effect`: `hidden_from_lists_on_transactions_and_records` |
| `name` | text | Name | yes | no |  | `purpose`: `tax_account_name` |
| `taxaccttype` | select | Tax Account Type | yes | no |  | `purpose`: `tax_control_account_type` |
| `country` | text |  | no | no |  |  |
| `externalid` | text | ExternalId | no | no |  |  |
| `nexus` | text |  | no | no |  |  |
