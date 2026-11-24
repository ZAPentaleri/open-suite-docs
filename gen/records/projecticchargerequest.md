# Job Intercompany Cross Charge Request (`projecticchargerequest`)
<!-- id: record:projecticchargerequest -->

| Fact | Value |
|---|---|
| Internal ID | `projecticchargerequest` |

## Record facts

- `script_types`: `client`, `server`
- `script_functions`: `create`, `update`, `copy`, `delete`, `search`
- `requires_feature`: `Project Management`, `Intercompany Framework` [`real_name`]
- `script_types`: `client`, `server`
- `purpose`: `cross_charge_amounts_between_subsidiaries`
- `effect`: `shown_in_intercompany_cross_charges_list`
- `value_examples`: `project_completion_trigger_amount_200`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `amount` | poscurrency | Amount | yes |  |  | `purpose`: `charge_amount`; `caveat`: `amounts_converted_to_eur` |
| `chargetrigger` | select | Trigger | yes |  |  | `purpose`: `charge_trigger`; `options`: `Date`, `Project Completion`, `Project Tasks/Milestone Completion` |
| `currency` | select | Currency | yes |  |  | `purpose`: `currency` |
| `project` | select | Project | yes |  |  | `purpose`: `project` |
| `tosubsidiary` | select | To Subsidiary | yes |  |  | `purpose`: `buying_subsidiary` |
| `externalid` | text | External ID | no |  |  |  |
| `id` | integer | Internal ID | no |  |  |  |
| `projecttask` | select | Project Task | no |  |  |  |
| `triggerdate` | date | Date | no |  |  |  |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `amount` | currency | Amount |  |
| `chargetrigger` | text | Trigger |  |
| `currency` | select | Currency |  |
| `externalid` | select | External ID |  |
| `externalidstring` | text | External ID (Text) |  |
| `id` | select | Internal ID |  |
| `idnumber` | integer | Internal ID (Number) |  |
| `project` | select | Project |  |
| `projecttask` | select | Project Task |  |
| `tosubsidiary` | select | To Subsidiary |  |
| `triggerdate` | date | Date |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `amount` | currency | Amount |  |
| `chargetrigger` | text | Trigger |  |
| `currency` | select | Currency |  |
| `externalid` | select | External ID |  |
| `id` | select | Internal ID |  |
| `project` | select | Project |  |
| `projecttask` | select | Project Task |  |
| `tosubsidiary` | select | To Subsidiary |  |
| `triggerdate` | date | Date |  |
