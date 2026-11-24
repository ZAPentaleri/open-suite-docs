# Case Priority (`supportcasepriority`)
<!-- id: record:supportcasepriority -->

| Fact | Value |
|---|---|
| Internal ID | `supportcasepriority` |

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `description` | textarea | Description | no |  | marked_new | `purpose`: `priority_description` |
| `insertbefore` | select | Insert Before | no |  | marked_new | `purpose`: `insert_before_priority`; `effect`: `orders_dropdown_entries` |
| `isinactive` | checkbox | Inactive | no |  | marked_new | `purpose`: `inactivate_priority` |
| `name` | text | Case Priority | yes |  | marked_new | `purpose`: `priority_name` |
| `externalid` | text | External ID | no |  | marked_new |  |
| `id` | integer | Internal ID | no |  | marked_new |  |
