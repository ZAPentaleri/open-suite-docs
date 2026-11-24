# Impact Subcategory (`impactsubcategory`)
<!-- id: record:impactsubcategory -->

| Fact | Value |
|---|---|
| Internal ID | `impactsubcategory` |
| Category | item |
| Client scripts | yes |
| Server scripts | yes |
| Tags | `supports_create`, `supports_delete`, `supports_read`, `supports_update` |

## Record facts

- `script_types`: `client`, `server`
- `requires_feature`: `SaaS Metrics` [`real_name`]
- `script_functions`: `read`, `create`, `update`, `delete`
- `configured_by`: `enable_features_analytics_subtab` [`saas_metrics`]
- `ui_location`: `setup_accounting_accounting_lists_impact_subcategory`
- `can`: `filter_accounting_list_to_impact_subcategories`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `externalid` | text | External ID | no | no |  |  |
| `isinactive` | checkbox | Inactive | no | no |  |  |
| `name` | text | Name | yes | no |  |  |
