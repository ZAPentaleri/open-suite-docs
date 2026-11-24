# Sales Role (`salesrole`)
<!-- id: record:salesrole -->

| Fact | Value |
|---|---|
| Internal ID | `salesrole` |
| Category | list |
| Client scripts | yes |
| Server scripts | yes |
| Searchable | yes |
| Tags | `custom_fields_supported`, `scripting_full` |

## Record facts

- `script_types`: `client`, `server`
- `requires_feature`: `Team Selling` [`real_name`]
- `script_types`: `client`, `server`
- `purpose`: `role_of_sales_team_member`
- `configured_by`: `enable_features_crm_subtab` [`team_selling`]
- `script_functions`: `create`, `update`, `copy`, `delete`, `search`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `description` | textarea | Description | no | no |  | `purpose`: `description` |
| `isinactive` | checkbox | Inactive | no | no |  | `purpose`: `inactivate_sales_role` |
| `issalesrep` | checkbox | Sales Rep | no | no |  | `purpose`: `sales_rep_role`; `effect`: `included_in_sales_reports_and_kpis` |
| `name` | text | Role | yes | no |  | `purpose`: `sales_role_name` |
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

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `description` | text | Description |  |
| `externalid` | select | External ID |  |
| `internalid` | select | Internal ID |  |
| `isinactive` | checkbox | Inactive |  |
| `name` | text | Name |  |

## Search joins

| Join ID | Label | Joined record |
|---|---|---|
| `user` | User | Employee |
