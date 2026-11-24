# Bonus (`bonus`)
<!-- id: record:bonus -->

| Fact | Value |
|---|---|
| Internal ID | `bonus` |
| Category | entity |
| Client scripts | yes |
| Server scripts | yes |
| Searchable | yes |
| Tags | `supports_copy`, `supports_create`, `supports_delete`, `supports_read`, `supports_search`, `supports_update` |

## Record facts

- `record_actions`: `cancel:exclude_from_payroll`
- `not_supported`: `action_all_qualified_instances` [`cancel_action`]
- `script_types`: `client`, `server`
- `script_functions`: `read`, `create`, `update`, `copy`, `delete`, `search`
- `requires_feature`: `Compensation Tracking` [`real_name`]
- `record_actions` (refs `N/action`)
- `not_supported`: `update` (when `bonus_type_has_payroll_item`)
- `avoid_when`: `status_change_workflow_triggers` (when `bonus_type_has_payroll_item`)
- `purpose`: `award_bonuses_to_employees`
- `effect`: `suitepeople_payroll_sets_status_when_payroll_item`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `bonusamountabsolute` | posfloat | Amount | yes |  |  |  |
| `bonusamountpercentage` | posfloat | Percentage | no |  |  |  |
| `bonusawarddate` | date | Award Date | yes |  |  |  |
| `bonuscomment` | textarea | Comment | no |  |  |  |
| `bonusemployee` | select | Employee | yes |  |  |  |
| `bonusstatus` | select | Payment Status | yes |  |  |  |
| `bonustype` | select | Bonus Type | yes |  |  |  |
| `externalid` | text | External ID | no |  |  |  |
| `id` | integer | Internal ID | no |  |  |  |
| `internalid` | integer | Internal ID | no |  |  |  |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `bonusamountabsolute` | float | Amount |  |
| `bonusamountpercentage` | float | Percentage |  |
| `bonusawarddate` | date | Award Date |  |
| `bonuscomment` | text | Comment |  |
| `bonuscurrency` | select | Currency |  |
| `bonusemployee` | select | Employee |  |
| `bonusstatus` | select | Status |  |
| `bonustype` | select | BonusType |  |
| `externalid` | select | External ID |  |
| `externalidstring` | text | External ID (Text) |  |
| `internalid` | select | Internal ID |  |
| `internalidnumber` | integer | Internal ID (Number) |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `bonusamountabsolute` | float | Amount |  |
| `bonusamountpercentage` | float | Percentage |  |
| `bonusawarddate` | date | Award Date |  |
| `bonuscomment` | text | Comments |  |
| `bonuscurrency` | select | Currency |  |
| `bonusemployee` | select | Employee |  |
| `bonusstatus` | select | Status |  |
| `bonustype` | select | Bonus Type |  |
| `externalid` | select | External ID |  |
| `internalid` | select | Internal ID |  |
