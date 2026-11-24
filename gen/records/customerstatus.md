# Customer Status (`customerstatus`)
<!-- id: record:customerstatus -->

| Fact | Value |
|---|---|
| Internal ID | `customerstatus` |
| Category | entity |
| Client scripts | yes |
| Server scripts | yes |
| Searchable | yes |
| Tags | `custom_fields_supported`, `scripting_full` |

## Record facts

- `script_types`: `client`, `server`
- `script_functions`: `create`, `update`, `copy`, `delete`, `search`, `transform`
- `purpose`: `lead_prospect_or_customer_sales_stage`
- `ui_location`: `setup_sales_setup_tasks_customer_statuses_new`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `description` | textarea | Description | no | no |  | `purpose`: `status_description`; `max_length`: `199` characters |
| `includeinleadreports` | checkbox | Include In Lead Reports | no | no |  | `purpose`: `include_in_lead_reports` |
| `isinactive` | checkbox | Inactive | no | no |  | `purpose`: `inactivate_status` |
| `name` | text | Status | yes | no |  | `purpose`: `status_title`; `displayed_on`: `entity_status_field`; `displayed_on`: `estimate_status_field` (when `prospect_stage`) |
| `probability` | percent | Probability | yes | no |  | `purpose`: `purchase_probability` |
| `stage` | select | Stage | no | no |  | `purpose`: `sales_cycle_stage` |
| `externalid` | text | ExternalId | no | no |  |  |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `description` | text | Description |  |
| `externalid` | select | External ID |  |
| `externalidstring` | text | External ID (Text) |  |
| `includeinleadreports` | checkbox | Include in Lead Reports |  |
| `internalid` | select | Internal ID |  |
| `internalidnumber` | integer | Internal ID (Number) |  |
| `isinactive` | checkbox | Inactive |  |
| `name` | text | Name |  |
| `probability` | percent | Probability |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `description` | text | Description |  |
| `externalid` | select | External ID |  |
| `includeinleadreports` | checkbox | Include in Lead Reports |  |
| `internalid` | select | Internal ID |  |
| `isinactive` | checkbox | Inactive |  |
| `name` | text | Name |  |
| `probability` | text | Probability |  |

## Search joins

| Join ID | Label | Joined record |
|---|---|---|
| `user` | User | Employee |
