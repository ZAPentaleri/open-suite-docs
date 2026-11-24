# As Charged Project Revenue Rule (`aschargedprojectrevenuerule`)
<!-- id: record:aschargedprojectrevenuerule -->

| Fact | Value |
|---|---|
| Internal ID | `aschargedprojectrevenuerule` |
| Category | entity |
| Client scripts | yes |
| Server scripts | yes |
| Searchable | yes |
| Tags | `scripting_full` |

## Record facts

- `script_types`: `client`, `server`
- `script_functions`: `create`, `update`, `copy`, `delete`, `search`
- `requires_feature`: `Project Management`, `Charge-Based Billing`, `Advanced Revenue Management` [`real_name`]
- `search_only_sublists`: `systemnotes`
- `editable_columns`: `chargerule:chargerule`
- `purpose`: `recognize_project_charge_revenue_when_created`
- `read_only_columns`: `amount`, `type`, `billingitem`, `description` [`chargerule_sublist`]

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `description` | textarea | Description | no | no |  | `purpose`: `rule_description` |
| `name` | text | Name | yes | no |  | `purpose`: `rule_name` |
| `totalamounttorecognize` | currency | Total Amount to Recognize | no | no |  | `purpose`: `total_charges_available_for_rule` |
| `customform` | select | Custom Form | yes | no |  |  |
| `externalid` | text | External ID | no | no |  |  |
| `isinactive` | checkbox | Inactive | no | no |  |  |
| `project` | select | Project | no | no |  |  |
| `recognizeoffactualcharges` | checkbox | Recognize Off Actual Charges | no | no |  |  |
| `revenuereconciled` | checkbox | Revenue Reconciled | no | no |  |  |
| `templatestored` | text | Store Form with Record | no | no |  |  |

## Sublist `chargerule` (Charge Rules)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `actualamount` | currency | Amount (Actual) | no |  |  |
| `billingitem` | select | Billing Item | no |  |  |
| `chargerule` | select | Name | yes |  |  |
| `description` | text | Description | no |  |  |
| `forecastamount` | currency | Amount | no |  |  |
| `ruletype` | select | Type | no |  |  |

## Tabs

| Tab ID | Label |
|---|---|
| `chargerules` | Charge Rules |
| `revenueelement` | Revenue Element |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `chargerule` | select | Charge Rule |  |
| `customform` | select | Custom Form |  |
| `description` | text | Description |  |
| `externalid` | select | External ID |  |
| `externalidstring` | text | External ID (Text) |  |
| `internalid` | select | Internal ID |  |
| `internalidnumber` | integer | Internal ID (Number) |  |
| `isinactive` | checkbox | Inactive |  |
| `name` | text | Name |  |
| `project` | select | Project |  |
| `recognizeoffactualcharges` | checkbox | Recognize Off Actual Charges |  |
| `revenuereconciled` | checkbox | Revenue Reconciled |  |
| `ruletype` | select | Rule Type |  |
| `totalamounttorecognize` | currency | Total Amount to Recognize |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `chargerule` | select | Charge Rule |  |
| `customform` | select | Custom Form |  |
| `description` | text | Description |  |
| `externalid` | select | External ID |  |
| `internalid` | select | Internal ID |  |
| `isinactive` | checkbox | Inactive |  |
| `name` | text | Name |  |
| `recognizeoffactualcharges` | checkbox | Recognize Off Actual Charges |  |
| `revenuereconciled` | checkbox | Revenue Reconciled |  |
| `totalamounttorecognize` | currency | Total Amount to Recognize |  |

## Search joins

| Join ID | Label | Joined record |
|---|---|---|
| `project` | Project | Job |
