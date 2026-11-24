# Financial Institution (`financialinstitution`)
<!-- id: record:financialinstitution -->

| Fact | Value |
|---|---|
| Internal ID | `financialinstitution` |
| Category | list |
| Client scripts | no |
| Server scripts | yes |
| Searchable | yes |
| Tags | `supports_create`, `supports_delete`, `supports_read`, `supports_search`, `supports_update` |

## Record facts

- `script_types`: `server`
- `not_supported`: `copy`, `transform`
- `requires_permission`: `financial_institution_records_full`
- `script_types`: `server`
- `script_functions`: `read`, `create`, `update`, `delete`, `search`
- `not_supported`: `copy`, `transform`
- `not_scriptable`: `format_profile_table`
- `purpose`: `holds_format_profiles_for_bank_and_card_imports`
- `ui_location`: `setup_accounting_financial_institution`
- `ui_effect`: `add_opens_format_profile_page`
- `use_instead`: `bank_feeds_suiteapp`, `amex_corporate_card_suiteapp` (when `creating_by_script`)

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `createdby` | select | Created By | no |  |  | `purpose`: `created_by`; `value_source`: `system_displayed` |
| `createddate` | date | Created Date | no |  |  | `purpose`: `date_created`; `value_source`: `system_displayed` |
| `description` | text | Description | no |  |  | `purpose`: `description` |
| `financialinstitution` | text | Financial Institution | yes |  |  | `purpose`: `financial_institution_name` |
| `isinactive` | checkbox | Inactive | no |  |  | `purpose`: `inactivate_institution`; `cannot`: `import_with_inactive_institution` |
| `lastmodifieddate` | date | Last Modified Date | no |  |  | `purpose`: `date_last_modified`; `value_source`: `system_displayed` |
| `modifiedby` | select | Modified By | no |  |  | `purpose`: `last_modified_by`; `value_source`: `system_displayed` |
| `externalid` | text | External ID | no |  |  |  |
| `id` | integer | Internal ID | no |  |  |  |
| `internalid` | integer | Internal ID | no |  |  |  |
