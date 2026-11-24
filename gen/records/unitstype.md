# Units Type (`unitstype`)
<!-- id: record:unitstype -->

| Fact | Value |
|---|---|
| Internal ID | `unitstype` |
| Category | list |
| Client scripts | no |
| Server scripts | yes |
| Searchable | yes |
| Tags | `custom_fields_supported`, `scripting_full` |

## Record facts

- `script_types`: `server`
- `purpose`: `units_to_stock_purchase_and_sell_items`
- `script_functions`: `create`, `update`, `copy`, `delete`, `search`
- `requires_feature`: `Multiple Units of Measure` [`real_name`]

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `isinactive` | checkbox | Inactive | no | no |  | `purpose`: `inactivate_units_type`; `ui_effect`: `hidden_from_lists_and_not_selectable` |
| `name` | text | Type Name | yes | no |  | `purpose`: `units_type_name`; `value_examples`: `Length` |
| `externalid` | text | ExternalId | no | no |  |  |

## Sublist `uom` (Units)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `abbreviation` | text | Abbreviation | yes |  |  |
| `baseunit` | checkbox | Base Unit | no |  |  |
| `conversionrate` | posfloat | Conversion Rate (/base) | yes |  |  |
| `internalid` | text |  | no |  |  |
| `pluralabbreviation` | text | Plural Abbreviation | yes |  |  |
| `pluralname` | text | Plural Name | yes |  |  |
| `unitname` | text | Name | yes |  |  |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `abbreviation` | text | Abbreviation Name |  |
| `baseunit` | checkbox | Is Base Unit |  |
| `conversionrate` | text | Rate |  |
| `externalid` | select | External ID |  |
| `externalidstring` | text | External ID (Text) |  |
| `formuladate` | date | Formula (Date) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulatext` | text | Formula (Text) |  |
| `internalid` | select | Internal ID |  |
| `internalidnumber` | integer | Internal ID (Number) |  |
| `isinactive` | checkbox | Inactive |  |
| `name` | text | Name |  |
| `pluralabbreviation` | text | Abbreviation Name(Plural) |  |
| `pluralname` | text | Unit Name(Plural) |  |
| `unitname` | text | Unit Name |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `abbreviation` | text | Abbreviation Name |  |
| `baseunit` | checkbox | Is Base Unit |  |
| `conversionrate` | text | Rate |  |
| `externalid` | select | External ID |  |
| `formulacurrency` | currency | Formula (Currency) |  |
| `formuladate` | date | Formula (Date) |  |
| `formuladatetime` | datetime | Formula (Date/Time) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulapercent` | percent | Formula (Percent) |  |
| `formulatext` | text | Formula (Text) |  |
| `internalid` | select | Internal ID |  |
| `isinactive` | checkbox | Inactive |  |
| `name` | text | Name |  |
| `pluralabbreviation` | text | Abbreviation Name(Plural) |  |
| `pluralname` | text | Unit Name(Plural) |  |
| `unitname` | text | Unit Name |  |
