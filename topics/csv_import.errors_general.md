# CSV import errors: general and relationships
<!-- id: topic:csv_import.errors_general -->

Part of: [CSV import](csv_import.md)

Errors that can hit an import of any record type, followed by errors specific to entity
(relationship) records such as customers, contacts and prospects. The source material behind this
reference keeps each message by its gist rather than its exact wording, so the first column
describes what the message says. An empty fix cell means the source gives only the cause.

## Any record type

| Message (gist) | Cause | Fix |
|---|---|---|
| Duplicate item name | The same value appears twice in one multi-select cell. | Remove the repeated IDs from the cell. |
| The files could not be uploaded | UTF-8 was selected as the encoding, but the file was saved as ANSI. | Save the file again as UTF-8. |
| A mandatory field is missing | Stray extra commas push values out of their columns. | Open the file in a plain text editor and look for the extra commas. |
| You don't have permission to set this element | The field is read-only, or belongs to a feature that is turned off. | Turn on *Ignore read-only fields*. |
| Bad reference to the effective date | The dates in the file use a format different from the user's date format preference. | Write the dates in the format that preference expects. |
| The saved search does not exist | A saved search used for sourcing is inactive, deleted or private. | Make that saved search active again and accessible to the importing user. |
| Please enter a value for amount | Lines are duplicated, or the amount is not a number. | Remove the duplicate lines, write amounts as plain numbers, and avoid zero amounts. |
| A custom record entry with this name already exists | The external ID is already used by another entry. | Use an external ID that no other record has. |
| The number of columns differs from the header | A row holds more values than there are header columns. | Keep every row's values within the header's columns. |
| afterSubmit failed, but the record was saved | A user event script failed after the record had been saved. | Rerun the script's work for that record, not the import. |
| Warning that the language settings don't match | The user's personal language differs from the company language. | |

> [!SOURCE-ERROR] The source's index of messages for this page leaves out the error about a custom
> record entry name that already exists, although the message itself is documented. If you look
> errors up through the source's index, search the page itself for this one.
<!-- source-error: topic:csv_import.errors_general | - | message_index_omits_custom_record_entry_error -->

Two more things to watch for:

- Saved imports created in older releases may have *Ignore read-only fields* turned off; check it
  before you rerun them.
- Excel converts text that looks like a date into a real date. To keep such a value as text, enter
  it as a formula: an equals sign followed by the value in double quotes.

## Relationship records

| Message (gist) | Cause | Fix |
|---|---|---|
| Invalid category reference key for website null | No web site is mapped. | Map the site column, or give the site a default value. |
| No matching record found | An update is keyed on a field that is not a key. | Key on internal ID, external ID or the customer ID. |
| Multiple records matched | An Add or Update of a child customer has no unique key. | Map the internal ID, the external ID or the customer ID. |
| A value for type is required | The type field is mandatory on the form in use. | Clear the mandatory setting on the type field of the preferred form. |
| VAT registration number is too long | The tax registration number is longer than 20 characters. | Shorten it to 20 characters. |
| Country and state don't match | The country field is hidden on the address form. | Show the country field on that form. |
| Invalid reference key for the individual field | The field that marks the entity as a person is not set. | Map it, or give it a default value. |
| Illegal ID, enter a name | The individual flag is set by default while the type field is hidden on the form. | Show the type field on the form. |
| Illegal ID, enter a name | An individual customer is imported together with contacts. | Use an import of customers only. |
| Invalid company reference key | The customer is inactive. | Import it while it is active, then mark it inactive. |
| Record is missing | Phone numbers were mapped incorrectly. | Map the internal IDs of both the prospect and the contact. |
| Bad lead source reference | The value is not in the lead source list, or is spelled differently. | Match an existing lead source value, or add the value to the list. |
| Invalid field value for email | The email address is missing or malformed. | Give a valid email address on every row. |
| Entity already exists | The external ID is already used by a different entity. | Find the record that owns it with a saved search, then change one of the external IDs. |
