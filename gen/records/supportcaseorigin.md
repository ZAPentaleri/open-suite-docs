# Case Origin (`supportcaseorigin`)
<!-- id: record:supportcaseorigin -->

| Fact | Value |
|---|---|
| Internal ID | `supportcaseorigin` |

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `description` | textarea | Description | no |  | marked_new | `purpose`: `description`; `max_length`: `199` characters |
| `insertbefore` | select | Insert Before | no |  | marked_new | `purpose`: `insert_before_origin`; `effect`: `orders_dropdown_entries` |
| `isinactive` | checkbox | Inactive | no |  | marked_new | `purpose`: `inactivate_origin`; `cannot`: `select_inactive_origin` |
| `name` | text | Case Origin | yes |  | marked_new | `purpose`: `origin_name`; `displayed_on`: `case_origin_field`, `online_case_form` |
| `externalid` | text | External ID | no |  | marked_new |  |
| `id` | integer | Internal ID | no |  | marked_new |  |
| `lastmodifieddate` | datetime | Last Modified Date | no |  | marked_new |  |
| `origbinactive` | checkbox | Is Inactive (original) | no |  | marked_new |  |
