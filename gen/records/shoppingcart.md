# Shopping Cart (`shoppingcart`)
<!-- id: record:shoppingcart -->

| Fact | Value |
|---|---|
| Internal ID | `shoppingcart` |
| Category | website |
| Client scripts | no |
| Server scripts | yes |
| Searchable | yes |
| Tags | `custom_fields_supported`, `search_only`, `supports_search` |

## Record facts

- `script_types`: `server`
- `script_functions`: `search`
- `exposed_data`: `customer_id`, `date_created`, `date_edited`, `item_ids`, `website_id`
- `returns_null_when`: `anonymous_shopper` [`customer_id`]
- `ui_location`: `lists_search_saved_searches_new_shopping_cart`
- `use_cases`: `marketing_campaigns_from_cart_data`
- `value_examples`: `carts_on_website_1_containing_item_352`

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `cartid` | select | Cart ID |  |
| `cartidnumber` | integer | Cart ID (Number) |  |
| `customerid` | select | Customer ID |  |
| `customeridnumber` | integer | Customer ID (Number) |  |
| `datecreated` | datetime | Date Created |  |
| `datemodified` | datetime | Date Last Item Added |  |
| `internalid` | select | Internal ID |  |
| `internalidnumber` | integer | Internal ID (Number) |  |
| `item` | select | Item |  |
| `itemid` | select | Item ID |  |
| `itemidnumber` | integer | Item ID (Number) |  |
| `itemprice` | currency | Item Price |  |
| `subtotal` | currency | SubTotal |  |
| `website` | select | Website |  |
| `websiteid` | select | Website ID |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `cartid` | integer | Cart ID |  |
| `customerid` | integer | Customer ID |  |
| `datecreated` | datetime | Date Created |  |
| `datemodified` | datetime | Date Last Item Added |  |
| `daysold` | integer | Days old |  |
| `internalid` | select | Internal ID |  |
| `item` | select | Items in Cart |  |
| `itemid` | select | Item ID |  |
| `itemprice` | currency | Item Price |  |
| `itemqty` | integer | Item Quantity |  |
| `subtotal` | currency | Sub-Total Amount |  |
| `website` | select | Website |  |
| `websiteid` | integer | Website ID |  |

## Search joins

| Join ID | Label | Joined record |
|---|---|---|
| `item` | Item | Item |
| `shopper` | Shopper | Customer |
| `user` | User | Employee |
