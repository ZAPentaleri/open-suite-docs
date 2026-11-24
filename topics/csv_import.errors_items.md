# CSV import errors: items
<!-- id: topic:csv_import.errors_items -->

Part of: [CSV import](csv_import.md)

Errors seen when importing item records and their sublists. More item errors are on
[items, continued](csv_import.errors_items_more.md). The first column paraphrases the message; the
spec does not keep exact wording. Where the fix column is empty, the spec states only the cause.

> [!SOURCE-ERROR] The source text behind this page starts in the middle of a sentence, so material
> before the first entry below may be missing. Treat the list as possibly incomplete.
<!-- source-error: topic:csv_import.errors_items | - | page_text_starts_mid_sentence -->

## Pricing and units

| Message (gist) | Cause | Fix |
|---|---|---|
| Please enter the missing prices | Quantity pricing tiers are imported without a price for quantity 0. | Include the quantity-0 price in the import. |
| Bad price level reference | The price level is inactive or misspelled. | Pick the price level as a default value instead. |
| Invalid unit reference key for a null units type | Units are set but the units type is not. | Map the units type as well. |

## References to other records

| Message (gist) | Cause | Fix |
|---|---|---|
| Invalid reference key for a custom item field | The custom field value was not found. | Check spelling, spaces, letter case and the multi-select delimiter. |
| Invalid item reference key | A related item's name is wrong, or a display name was given. | Use the web store display name or the internal ID. |
| Invalid asset or COGS account reference key | The account name in the file is not formatted the way NetSuite shows it. | Use the account's internal ID, or set it as a default value. |
| Invalid parent reference key | The parent item is of a different item type. | Give the child the same item type as its parent. |

## Sublists and matrix items

| Message (gist) | Cause | Fix |
|---|---|---|
| Adding a line to locations is not allowed | The location sublist is not mapped. | Map the location key. |
| A subsidiary used on a transaction cannot be removed | The import replaced the item's subsidiary list. | List all of the item's subsidiaries, separated by pipes. |
| Please enter a value for type | A related item has no type. | Run an update with the type mapped. |
| You may only update existing matrix headers | Only part of the quantity matrix was supplied. | Turn on *Overwrite sublists* and supply all the quantities. |

## Names

| Message (gist) | Cause | Fix |
|---|---|---|
| Unexpected error | Both the name and the display name are mapped. | Map only one of them, together with the translation language. |
