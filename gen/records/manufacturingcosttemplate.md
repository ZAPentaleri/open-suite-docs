# Manufacturing Cost Template (`manufacturingcosttemplate`)
<!-- id: record:manufacturingcosttemplate -->

| Fact | Value |
|---|---|
| Internal ID | `manufacturingcosttemplate` |
| Category | list |
| Client scripts | no |
| Server scripts | yes |
| Searchable | yes |
| Tags | `custom_fields_supported`, `scripting_full` |

## Record facts

- `script_types`: `server`
- `requires_feature`: `Manufacturing Routing and Work Center` [`real_name`]
- `configured_by`: `enable_features_items_and_inventory_subtab` [`manufacturing_routing_and_work_center`]
- `ui_location`: `lists_supply_chain_manufacturing_cost_template`
- `script_functions`: `create`, `update`, `copy`, `delete`, `search`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `isinactive` | checkbox | Inactive | no | no |  | `purpose`: `inactivate_template`; `ui_effect`: `hidden_from_lists` |
| `memo` | textarea | Memo | no | no |  | `purpose`: `memo`; `supported_in`: `search` |
| `name` | text | Name | yes | no |  | `purpose`: `template_name` |
| `subsidiary` | select | Subsidiary | yes | no |  | `purpose`: `subsidiary`; `requires_feature`: `OneWorld` [`real_name`] |
| `customform` | select | Custom Form | no | no |  |  |
| `externalid` | text | ExternalId | no | no |  |  |

## Sublist `costdetail` (Costs)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `costcategory` | select | Cost Category | yes |  |  |
| `fixedrate` | currency2 | Fixed Rate | no |  |  |
| `item` | select | Item | yes |  |  |
| `runrate` | currency2 | Run Rate | no |  |  |

## Tabs

| Tab ID | Label |
|---|---|
| `costdetails` | Cost Details |
| `workflow` | Workflow |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `externalid` | select | External ID |  |
| `externalidstring` | text | External ID (Text) |  |
| `formuladate` | date | Formula (Date) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulatext` | text | Formula (Text) |  |
| `internalid` | select | Internal ID |  |
| `internalidnumber` | integer | Internal ID (Number) |  |
| `isinactive` | checkbox | Inactive |  |
| `item` | select | Item |  |
| `memo` | text | Memo |  |
| `name` | text | Name |  |
| `subsidiary` | select | Subsidiary |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `externalid` | select | External ID |  |
| `formulacurrency` | currency | Formula (Currency) |  |
| `formuladate` | date | Formula (Date) |  |
| `formuladatetime` | datetime | Formula (Date/Time) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulapercent` | percent | Formula (Percent) |  |
| `formulatext` | text | Formula (Text) |  |
| `internalid` | select | Internal ID |  |
| `isinactive` | checkbox | Inactive |  |
| `item` | select | Item |  |
| `memo` | text | Memo |  |
| `name` | text | Name |  |
| `subsidiary` | select | Subsidiary |  |

## Search joins

| Join ID | Label | Joined record |
|---|---|---|
| `user` | User | Employee |
