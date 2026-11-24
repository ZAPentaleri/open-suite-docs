# Item Supply Plan (`itemsupplyplan`)
<!-- id: record:itemsupplyplan -->

| Fact | Value |
|---|---|
| Internal ID | `itemsupplyplan` |
| Category | transaction |
| Client scripts | yes |
| Server scripts | yes |
| Searchable | yes |
| Tags | `custom_fields_supported`, `scripting_full` |

## Record facts

- `script_types`: `client`, `server`
- `sublists`: `orders`
- `not_updatable_fields`: `item`, `location`, `units`
- `rule`: `receiptdate_not_before_orderdate`
- `read_only_fields`: `ordercreated`
- `script_functions`: `create`, `copy`, `update`, `delete`, `search`
- `effect`: `ordercreated_true_when_order_generated`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `item` | select | Item | yes | no |  | `purpose`: `planned_item` |
| `location` | select | Location | yes | no |  | `purpose`: `location`; `requires_feature`: `Multi-Location Inventory` [`real_name`]; `effect`: `filters_items_to_time_phased_at_location`; `doc_inconsistency`: `stray_test_text_in_help` |
| `memo` | text | Memo | no | no |  | `purpose`: `memo`; `supported_in`: `search` |
| `subsidiary` | select | Subsidiary | yes | no |  | `purpose`: `subsidiary`; `requires_feature`: `OneWorld` [`real_name`] |
| `units` | select | Unit of Measure | no | no |  | `purpose`: `plan_unit_of_measure` |
| `customform` | select | Custom Form | no | no |  |  |
| `externalid` | text | ExternalId | no | no |  |  |

## Sublist `order` (Orders)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `orderdate` | date | Order Date | yes |  |  |
| `ordertype` | select | Order Type | yes |  |  |
| `quantity` | posfloat | Quantity | yes |  |  |
| `receiptdate` | date | Receipt Date | yes |  |  |
| `sourcelocation` | select | Source Location | no |  |  |

## Tabs

| Tab ID | Label |
|---|---|
| `orders` | Orders |
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
| `item` | select | Item |  |
| `lastmodifieddate` | datetime | Last Plan Modified |  |
| `location` | select | Location |  |
| `memo` | text | Memo |  |
| `ordercreated` | checkbox | Order Created |  |
| `orderdate` | date | Order Date |  |
| `ordertype` | select | Order Type |  |
| `quantity` | float | Quantity |  |
| `receiptdate` | date | Receipt Date |  |
| `subsidiary` | select | Subsidiary |  |
| `units` | select | Units |  |

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
| `item` | select | Item |  |
| `lastmodifieddate` | datetime | Last Plan Modified |  |
| `location` | select | Location |  |
| `memo` | text | Memo |  |
| `ordercreated` | checkbox | Order Created |  |
| `orderdate` | date | Order Date |  |
| `ordertype` | select | Order Type |  |
| `quantity` | float | Quantity |  |
| `quantityuom` | float | Quantity in Supply Plan Units |  |
| `receiptdate` | date | Receipt Date |  |
| `subsidiary` | select | Subsidiary |  |
| `units` | select | Units |  |

## Search joins

| Join ID | Label | Joined record |
|---|---|---|
| `location` | Location | Location |

## Suspected source errors

> [!SOURCE-ERROR] `stray_test_text_in_help` (on `location`)
<!-- source-error: record:itemsupplyplan | location | stray_test_text_in_help -->
