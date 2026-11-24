# Case Status (`supportcasestatus`)
<!-- id: record:supportcasestatus -->

| Fact | Value |
|---|---|
| Internal ID | `supportcasestatus` |

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `autoclosecase` | checkbox | Auto Close With Issues | no |  | marked_new | `purpose`: `auto_close_case`; `effect`: `closes_when_all_issues_closed`; `requires`: `automatically_close_linked_cases_preference` |
| `caseonhold` | checkbox | On Hold | no |  | marked_new | `purpose`: `case_on_hold`; `effect`: `stops_case_clock` |
| `description` | textarea | Description | no |  | marked_new | `purpose`: `status_description` |
| `insertbefore` | select | Insert Before | no |  | marked_new | `purpose`: `insert_before_status`; `effect`: `orders_dropdown_entries` |
| `isinactive` | checkbox | Inactive | no |  | marked_new | `purpose`: `inactivate_status` |
| `name` | text | Case Status | yes |  | marked_new | `purpose`: `status_name` |
| `stage` | select | Stage | yes |  | marked_new | `purpose`: `status_stage`; `allowed_values`: `Open`, `Escalated`, `Closed` |
| `externalid` | text | External ID | no |  | marked_new |  |
| `id` | integer | Internal ID | no |  | marked_new |  |
