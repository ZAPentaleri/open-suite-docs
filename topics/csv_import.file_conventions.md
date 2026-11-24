# CSV import file conventions
<!-- id: topic:csv_import.file_conventions -->

Part of: [CSV import](csv_import.md)

Rules for how the CSV file itself must be shaped and how the values in it are read.

## Size limits

- At most 25,000 records per import job. (The parent page states the same figure per file.)
- At most 50 MB per job, counting all of the job's files together.

## Header row and file shape

- If two columns carry the same header, the last one wins. That includes an empty cell: when the
  second of the duplicate columns is blank, it clears the value the first one supplied.
- Values to the right of the last header, with no header above them, are dropped.
- Blank lines at the end of the file are ignored, and so are trailing delimiters at the end of a
  row.
- A value that contains the column delimiter must be wrapped in quotes.
- If the decimal mark is the same character as the column delimiter (a comma, for instance), quote
  every decimal number.

## Record IDs and auto-numbering

- Bringing in the IDs of a legacy system for auto-numbered records works only if auto-numbering
  allows the generated number to be overridden. That setting is made per record type on the Set Up
  Auto-Generated Numbers page. Without it, the ID values in the file are ignored and NetSuite
  assigns its own numbers.
- An Update that tries to set an auto-generated ID fails with a no-permission error on the entity ID
  field; turn on *Ignore read-only fields* (see [advanced options](csv_import.advanced_options.md))
  so the rest of the row still goes through.
- For auto-numbered records, key your updates on external ID.
- A name reference to an auto-numbered record must include the number's prefix.

## Spreadsheet pitfalls

- Spreadsheet programs strip leading zeros; format such columns as text before saving.
- Excel turns long numbers into scientific notation and rounds them; format those columns as text
  as well.

## Booleans

| Field kind | Accepted for true | Accepted for false |
|---|---|---|
| Radio button | `TRUE` | `FALSE` |
| Checkbox | `TRUE`, `YES`, `T`, `Y` | `FALSE`, `NO`, `F`, `N` |

Letter case does not matter, but the words must be English whatever language the account uses.

## Hierarchies, names and multi-selects

- Levels of a hierarchy are separated by a space, a colon and a space (`Parent : Child`). Write the
  full path even where the UI hides the hierarchy and shows only the last level.
- Subsidiary values in particular must spell out the full hierarchy with that separator.
- A name reference must match what the UI displays, exactly.
- Several values in one multi-select cell are separated by a pipe by default; the separator can be
  changed in the advanced options.
- An import whose language does not match the account's runs more slowly.

## States and countries

- Give states and provinces by their short names. Full names may save the wrong value.
- **Give countries by their name, never by their code.** A country column holds the country's name
  as NetSuite displays it; a two-letter code in that column is not accepted (it fails with an
  invalid country reference key error, see [jobs](csv_import.jobs.md)). Country names are localized
  to the user's UI language, so the expected spelling depends on that language.
- The source also lists each country's internal ID, an ISO-style code, in
  [countries](../gen/catalogs/countries.md). Those codes are the internal identifiers of the
  countries; they are not what a CSV country column expects. The source does not state where in a
  CSV import, if anywhere, these codes may be used instead of names.

> [!SOURCE-ERROR] In the source's country table, the row for Namibia has no internal ID. The
> generated countries catalog shows a code for it, but verify that value in your account before
> relying on it.
<!-- source-error: topic:csv_import.file_conventions | - | country_table_row_missing_internal_id -->

## Example

A vendor file that exercises several of these rules. The column names are your own (they are
matched to fields during mapping); the values are invented.

```csv
Legacy Key,Vendor Name,Subsidiary,Payment Methods,On Hold,Country,Credit Limit
VND-00417,"Brennan, Shaw & Partners",Harbor Group : EMEA : Ireland,Wire|Check,N,Ireland,15000.50
VND-00418,Kestrel Freight,Harbor Group : Americas,Wire,no,Canada,8000
```

The quoted vendor name contains a comma; the subsidiary uses the full hierarchy with ` : `; the
multi-select cell uses the default pipe; the checkbox uses two accepted spellings of false; the
country is given by name; and the decimal mark is a period, so the amounts need no quotes.
