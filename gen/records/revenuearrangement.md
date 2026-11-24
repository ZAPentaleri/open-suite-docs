# Revenue Arrangement (`revenuearrangement`)
<!-- id: record:revenuearrangement -->

| Fact | Value |
|---|---|
| Internal ID | `revenuearrangement` |
| Category | transaction |
| Client scripts | no |
| Server scripts | yes |
| Searchable | yes |
| Search data via | Transaction |
| Tags | `custom_fields_supported`, `scripting_full` |

## Record facts

- `record_actions`: `allocate:redistribute_amount_by_fair_value`
- `governance_note`: `allocate:10`
- `returns_detail`: `notifications_and_response`
- `parameters`: `record_id_only`
- `ui_location`: `allocate_button`
- `use_when`: `arrangement_not_compliant_allocation_failed_or_flag_false`
- `script_types`: `server`
- `script_functions`: `create`, `update`, `copy`, `delete`, `search`
- `requires_feature`: `Accounting Periods`, `Advanced Revenue Management` [`real_name`]
- `record_actions`: `allocate`
- `related` (refs `N/action`)
- `purpose`: `performance_obligations_for_revenue_allocation`

## Body fields

| Field ID | Type | Label | Required | submitField | Read-only | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `accountingbook` | select | Accounting Book | no | no |  |  | `purpose`: `segment_from_source_documents`; `value_source`: `source_documents`; `help_text_shared_with`: `revenuearrangement.class`, `revenuearrangement.department`, `revenuearrangement.location`, `revenuearrangement.subsidiary` |
| `class` | select | Class | no | no |  |  | `purpose`: `segment_from_source_documents`; `value_source`: `source_documents`; `help_text_shared_with`: `revenuearrangement.accountingbook`, `revenuearrangement.department`, `revenuearrangement.location`, `revenuearrangement.subsidiary` |
| `compliant` | checkbox | Compliant | no | no |  |  | `purpose`: `allocation_compliant`; `true_means`: `allocation_succeeded_or_not_needed`; `false_means`: `system_allocation_failed`; `can`: `clear_to_enable_reallocation` (when `transaction_is_allocation_bundle`) |
| `department` | select | Department | no | no |  |  | `purpose`: `segment_from_source_documents`; `value_source`: `source_documents`; `help_text_shared_with`: `revenuearrangement.accountingbook`, `revenuearrangement.class`, `revenuearrangement.location`, `revenuearrangement.subsidiary` |
| `location` | select | Location | no | no |  |  | `purpose`: `segment_from_source_documents`; `value_source`: `source_documents`; `help_text_shared_with`: `revenuearrangement.accountingbook`, `revenuearrangement.class`, `revenuearrangement.department`, `revenuearrangement.subsidiary` |
| `memo` | text | Memo | no | yes |  |  | `purpose`: `memo`; `displayed_on`: `default_list_view` |
| `subsidiary` | select | Subsidiary | no | no |  |  | `purpose`: `segment_from_source_documents`; `value_source`: `source_documents`; `help_text_shared_with`: `revenuearrangement.accountingbook`, `revenuearrangement.class`, `revenuearrangement.department`, `revenuearrangement.location` |
| `totalallocationamount` | currency | Total Carve-out | no | no | yes |  | `purpose`: `total_allocation_amount`; `value_kind`: `always_positive` |
| `totalrevenueamount` | currency | Total Revenue Amount | no | no | yes |  | `purpose`: `total_revenue_amount`; `value_source`: `sum_of_element_revenue_amounts` |
| `totalsellingamount` | currency | Transaction Total | no | no | yes |  | `purpose`: `total_selling_amount`; `value_source`: `sum_of_element_discounted_sales` |
| `trandate` | date | Date | yes | no |  |  | `purpose`: `arrangement_date`; `default_behavior`: `creation_date`; `effect`: `updates_plan_start_dates_before_recognition` |
| `tranisvsoebundle` | checkbox | Transaction Is Allocation Bundle | no | no |  |  | `purpose`: `allocation_bundle`; `true_means`: `revenue_allocated_between_elements` |
| `contrevhandlingtriggered` | checkbox | Contingent Revenue Handling Triggered | no | no |  |  |  |
| `transactionnumber` | text | Transaction Number | no | no |  |  |  |
