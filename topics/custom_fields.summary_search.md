# Summary search custom fields
<!-- id: topic:custom_fields.summary_search -->

Part of: [Custom fields](custom_fields.md)

See also: [Saved searches](saved_searches.md).

A summary search field shows, on each record, a roll-up of a saved search's results for that record:
for example the number of open cases for a customer, or the latest order date. It is an
out-of-the-box alternative to computing such values in SuiteScript.

## How it is evaluated

- The value is calculated every time a form showing the field is opened.
- The value is not stored. If a user edits the displayed value, the edit is discarded.
- Because it is computed per viewer, the result depends on which records the current user is allowed
  to see.
- Summary field values cannot be used in searches.
- Each such field runs a search when the form opens, so keep the number of summary fields on checkout
  forms small.

## Setting one up

- The saved search must use a summary type of count, sum, minimum, maximum or average. Grouping is not
  supported.
- The saved search needs an available filter on a field that matches the current record; that is what
  ties the roll-up to the record being viewed. A common technique is to use the internal ID as the
  available filter, which scopes the roll-up to exactly that record.
- When the record type of the field differs from the search's record type, choose the field to compare
  against, so the system knows how the two relate.
- Clear Store Value on the field.

## Supported fields

- **Field kinds**: entity, CRM, transaction body, other record and custom record fields.
- **Data types**: currency, date, date/time, decimal number, email address, free-form text, hyperlink,
  integer number, long text, percent, phone number, rich text, text area and time of day.
