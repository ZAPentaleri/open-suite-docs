# Units for Pick Decomposition (`pickdecomposition`)
<!-- id: record:pickdecomposition -->

| Fact | Value |
|---|---|
| Internal ID | `pickdecomposition` |

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `externalid` | text | External ID | no |  |  |  |
| `id` | integer | Internal ID | no |  |  |  |
| `inactive` | checkbox | Inactive | no |  |  |  |
| `unitstype` | select | Units Type | yes |  |  |  |

## Sublist `units` (PickDecompositionLine)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `id` | integer | ID | no |  |  |
| `owner` | select |  | no |  |  |
| `pickperquantity` | checkbox | Pick per quantity | no |  |  |
| `unit` | select | Unit | no |  |  |
