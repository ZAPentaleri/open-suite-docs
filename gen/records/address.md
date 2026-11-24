# Address (`address`)
<!-- id: record:address -->

| Fact | Value |
|---|---|
| Internal ID | `address` |
| Subrecord | yes |
| Tags | `deleted_record_search_supported` |

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `addr1` | text | Address 1 | no | no |  | `purpose`: `address_line_1`; `max_length`: `50` characters; `displayed_on`: `printed_forms`; `ui_effect`: `autofills_forms_when_default_ship_or_bill_address`; `required_when`: `online_bill_pay_feature` |
| `addr2` | text | Address 2 | no | no |  | `purpose`: `address_line_2`; `max_length`: `50` characters; `displayed_on`: `printed_forms`; `ui_effect`: `autofills_forms_when_default_ship_or_bill_address` |
| `addressee` | text | Addressee | no | no |  | `purpose`: `addressee_name`; `displayed_on`: `shipping_labels`; `ui_effect`: `printed_below_attention_on_label` |
| `addrphone` | phone | Phone | no | no |  | `purpose`: `address_phone` |
| `addrtext` | address | Address | no | no |  | `purpose`: `formatted_address_text`; `value_source`: `built_from_address_fields` |
| `attention` | text | Attention | no | no |  | `purpose`: `shipment_attention_name`; `displayed_on`: `shipping_labels`; `required_when`: `ups_integration` |
| `city` | text | City | no | no |  | `purpose`: `city`; `displayed_on`: `printed_forms`; `not_displayed_on`: `checks` |
| `country` | select | Country | yes | no |  | `purpose`: `country` |
| `override` | checkbox | Override | no | no |  | `purpose`: `override_free_form_address_text`; `true_means`: `addrtext_not_rebuilt_from_address_fields`; `false_means`: `addrtext_built_from_address_fields` |
| `state` | text | State | no | no |  | `purpose`: `state_or_province`; `displayed_on`: `printed_forms`; `not_displayed_on`: `checks` |
| `zip` | text | Zip | no | no |  | `purpose`: `postal_code`; `displayed_on`: `printed_forms`; `not_displayed_on`: `checks` |
| `addr3` | text | Address 3 | no | no |  |  |
| `addressformat` | textarea | Address Template | no | no |  |  |
| `customform` | select | Custom Form | no | no |  |  |
| `externalid` | text | ExternalId | no | no |  |  |

## Tabs

| Tab ID | Label |
|---|---|
| `custom` | Custom |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `address` | textarea | Address Text |  |
| `address1` | text | Address 1 |  |
| `address2` | text | Address 2 |  |
| `address3` | text | Address 3 |  |
| `addressee` | text | Addressee |  |
| `attention` | text | Attention |  |
| `city` | text | City |  |
| `country` | select | Country |  |
| `countrycode` | text | Country Code |  |
| `externalid` | select | External ID |  |
| `externalidstring` | text | External ID (Text) |  |
| `internalid` | select | Internal ID |  |
| `internalidnumber` | integer | Internal ID (Number) |  |
| `override` | checkbox | Override |  |
| `phone` | text | Address Phone |  |
| `state` | text | State |  |
| `zip` | text | Zip |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `address` | text | Address |  |
| `address1` | text | Address 1 |  |
| `address2` | text | Address 2 |  |
| `address3` | text | Address 3 |  |
| `addressee` | text | Addressee |  |
| `attention` | text | Attention |  |
| `city` | text | City |  |
| `country` | select | Country |  |
| `countrycode` | text | Country Code |  |
| `externalid` | select | External ID |  |
| `internalid` | select | Internal ID |  |
| `override` | text | Override |  |
| `phone` | text | Phone |  |
| `state` | text | State |  |
| `zip` | text | Zip |  |
