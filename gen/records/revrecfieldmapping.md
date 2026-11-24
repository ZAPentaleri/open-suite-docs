# Revenue Recognition Field Map (`revrecfieldmapping`)
<!-- id: record:revrecfieldmapping -->

| Fact | Value |
|---|---|
| Internal ID | `revrecfieldmapping` |
| Category | list |
| Client scripts | yes |
| Server scripts | yes |
| Searchable | yes |
| Tags | `scripting_full` |

## Record facts

- `script_types`: `client`, `server`
- `requires_feature`: `Advanced Revenue Management Essentials` [`real_name`]
- `script_types`: `client`, `server`
- `script_functions`: `create`, `read`, `update`, `copy`, `delete`, `search`
- `purpose`: `map_source_fields_to_revenue_arrangement_columns`
- `requires`: `projects_project_management_charge_based_billing` (when `project_revenue_line_source_type`)
- `workflow`: `consolidate_jobs_on_sales_transactions`, `disable_employee_commissions`, `enable_subscription_billing` (when `subscription_line_source_type`)

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `autoupdatetargetfieldvalue` | checkbox | Automatically Update Target Field Value | no |  |  | `purpose`: `auto_update_target_field`; `true_means`: `target_updated_on_arrangement_update` |
| `customtxform` | select | Custom Form | no |  |  | `purpose`: `custom_form`; `value_source`: `source_record_and_transaction_type`; `read_only_when`: `source_subscription_line_or_project_rule` |
| `description` | textarea | Description | no |  |  | `purpose`: `description` |
| `enddate` | date | End Date | no |  |  | `purpose`: `map_end_date`; `requires`: `startdate` (when `set`) |
| `sourcefield` | select | Source Field | yes |  |  | `purpose`: `source_field`; `can`: `map_one_source_to_many_targets` |
| `sourcerecordtype` | select | Source Record Type | yes |  |  | `purpose`: `source_record_type`; `allowed_values`: `Project Revenue Rule`, `Subscription Line`, `Transaction Line` |
| `startdate` | date | Start Date | no |  |  | `purpose`: `map_start_date` |
| `targetfield` | select | Target Field | yes |  |  | `purpose`: `target_field`; `allowed_values`: `custom_and_standard_arrangement_line_fields` |
| `transactiontype` | select | Transaction Type | no |  |  | `purpose`: `transaction_type`; `effect`: `mapping_only_for_this_type`; `read_only_when`: `source_subscription_line_or_project_rule` |
| `externalid` | text | External ID | no |  |  |  |
| `id` | integer | Internal ID | no |  |  |  |
