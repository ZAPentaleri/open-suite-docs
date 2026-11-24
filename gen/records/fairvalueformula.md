# Fair Value Formula (`fairvalueformula`)
<!-- id: record:fairvalueformula -->

| Fact | Value |
|---|---|
| Internal ID | `fairvalueformula` |

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `description` | text | Description | no |  | marked_new | `purpose`: `formula_description` |
| `fairvalueformula` | textarea | Fair Value Formula | yes |  | marked_new | `purpose`: `fair_value_formula_expression` |
| `inactive` | checkbox | Inactive | no |  | marked_new | `purpose`: `inactivate_formula`; `ui_effect`: `hidden_from_lists` |
| `name` | text | Name | yes |  | marked_new | `purpose`: `formula_name`; `displayed_on`: `dropdowns` |
| `customform` | select | Custom Form | no |  | marked_new |  |
| `externalid` | text | External ID | no |  | marked_new |  |
| `id` | integer | Internal ID | no |  | marked_new |  |
