# Case Type (`supportcasetype`)
<!-- id: record:supportcasetype -->

| Fact | Value |
|---|---|
| Internal ID | `supportcasetype` |

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `description` | textarea | Description | no |  | marked_new | `purpose`: `description`; `max_length`: `199` characters |
| `insertbefore` | select | Insert Before | no |  | marked_new | `purpose`: `insert_before_type`; `effect`: `orders_dropdown_entries` |
| `isinactive` | checkbox | Inactive | no |  | marked_new | `purpose`: `inactivate_case_type`; `ui_effect`: `hidden_on_cases_and_online_forms` |
| `name` | text | Case Type | yes |  | marked_new | `purpose`: `case_type_name`; `displayed_on`: `case_type_field`, `online_case_form` |
| `externalid` | text | External ID | no |  | marked_new |  |
| `id` | integer | Internal ID | no |  | marked_new |  |
| `lastmodifieddate` | datetime | Last Modified Date | no |  | marked_new |  |
| `origbinactive` | checkbox | Is Inactive (original) | no |  | marked_new |  |
