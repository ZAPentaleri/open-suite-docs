# CSV import errors: items, continued
<!-- id: topic:csv_import.errors_items_more -->

Part of: [CSV import](csv_import.md)

The rest of the item import errors (the first part is on [items](csv_import.errors_items.md)). The
first column paraphrases the message; the spec does not keep exact wording. Where the fix column is
empty, the spec states only the cause.

## Names and parents

| Message (gist) | Cause | Fix |
|---|---|---|
| Could not find records by name | A child matrix item's name includes the parent's name. | Give only the child's own name and put the parent in the subitem-of column. |
| Invalid parent reference key | The preference to show display names with item codes is on. | Turn that preference off, or give the value as display name with code. |
| Record has a different type | The record type chosen for the import is not the item's actual type. | |
| Invalid item reference key | A member item's name is formatted differently, or the item is inactive. | Refer to the member item by internal ID. |

## Values and delimiters

| Message (gist) | Cause | Fix |
|---|---|---|
| Bad cost estimate value | The amount includes a currency symbol. | Use a plain number. |
| Multiple values for a single-select dropdown | An external ID contains the multi-select delimiter. | Choose a multi-select delimiter that none of your values use. |

If the multi-select delimiter is left empty (null), a space is used as the delimiter.

## Assemblies and kits

| Message (gist) | Cause | Fix |
|---|---|---|
| Member items are required | The member file has no column linking each member to its assembly. | |
| Unexpected error (assembly) | Units are missing on the assembly or on its components. | |

## Demand plans and worksheets

| Message (gist) | Cause | Fix |
|---|---|---|
| Internal ID not found | The demand plan's ID is not mapped to the item. | |
| Bad start date reference | A future year is imported without its year, start date and end date. | Map the year and both dates. |
| Configure the inventory detail for this item | The preference requiring bins is on. | Turn that preference off while the worksheet import runs. |

## Translations

| Message (gist) | Cause | Fix |
|---|---|---|
| Invalid locale reference key | A new item is added with translation columns in the same import. | Add the item first, then import the translations as an update. |
