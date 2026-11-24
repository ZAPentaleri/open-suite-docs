# Inventory Number (`inventorynumber`)
<!-- id: record:inventorynumber -->

| Fact | Value |
|---|---|
| Internal ID | `inventorynumber` |
| Category | list |
| Client scripts | no |
| Server scripts | yes |
| Tags | `custom_fields_supported`, `no_copy`, `no_create`, `no_delete` |

## Record facts

- `script_types`: `server`
- `script_functions`: `update`, `search`
- `script_functions`: `update`, `search`
- `not_supported`: `copy`, `create`, `delete`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `memo` | textarea | Memo | no | no |  | `purpose`: `memo`; `supported_in`: `search` |
| `expirationdate` | date | Expiration Date | no |  |  |  |
| `externalid` | text | ExternalId | no | no |  |  |
| `inventorynumber` | text | Serial/Lot Number | no | no |  |  |
| `status` | text | Status | no | no |  |  |
| `units` | select | Stock Unit | no |  |  |  |

## Sublist `locations`

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `location` | select | Location | no |  | `purpose`: `item_location`; `caveat`: `role_restriction_only_without_multi_location`; `allowed_values_listed_in`: `location_records`; `ui_effect`: `new_option_creates_record`; `help_text_shared_with`: `assemblyitem.location`, `inventoryitem.location`, `lotnumberedinventoryitem.location`, `serializedinventoryitem.location` |
| `quantityonhand` | float | On Hand | no |  | `purpose`: `quantity_on_hand`; `value_source`: `system_displayed` |
| `quantityonorder` | float | On Order | no |  | `purpose`: `quantity_on_order`; `value_source`: `system_displayed` |
| `quantityavailable` | float | Available | no |  |  |
| `quantityintranist` | float | In Transit | no |  |  |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `expirationdate` | date | Expiration Date |  |
| `externalid` | select | External ID |  |
| `externalidstring` | text | External ID (Text) |  |
| `formuladate` | date | Formula (Date) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulatext` | text | Formula (Text) |  |
| `internalid` | select | Internal ID |  |
| `internalidnumber` | integer | Internal ID (Number) |  |
| `inventorynumber` | text | Number |  |
| `isonhand` | checkbox | Is On Hand |  |
| `item` | select | Item |  |
| `location` | select | Location |  |
| `memo` | text | Memo |  |
| `quantityavailable` | float | Availabile |  |
| `quantityintransit` | float | In Transit |  |
| `quantityonhand` | float | On Hand |  |
| `quantityonorder` | float | On Order |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `expirationdate` | date | Expiration Date |  |
| `externalid` | select | External ID |  |
| `formulacurrency` | currency | Formula (Currency) |  |
| `formuladate` | date | Formula (Date) |  |
| `formuladatetime` | datetime | Formula (Date/Time) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulapercent` | percent | Formula (Percent) |  |
| `formulatext` | text | Formula (Text) |  |
| `internalid` | select | Internal ID |  |
| `inventorynumber` | select | Number |  |
| `isonhand` | checkbox | Is On Hand |  |
| `item` | select | Item |  |
| `location` | select | Location |  |
| `memo` | text | Memo |  |
| `quantityavailable` | float | Available |  |
| `quantityintransit` | float | In Transit |  |
| `quantityonhand` | float | On Hand |  |
| `quantityonorder` | float | On Order |  |

## Search joins

| Join ID | Label | Joined record |
|---|---|---|
| `user` | User | Employee |
