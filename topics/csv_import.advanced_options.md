# CSV import advanced options
<!-- id: topic:csv_import.advanced_options -->

Part of: [CSV import](csv_import.md)

The advanced options are set in the import-options step of the Import Assistant and are saved with
the import map; a scripted import through [`N/task.CsvImportTask`](../modules/N_task.md#csvimporttask-object)
uses the options saved with its map. Whatever you set on a job overrides the account-wide CSV import
preferences (see [jobs, status and preferences](csv_import.jobs.md)).

| Option | Default | Section |
|---|---|---|
| Custom field system notes | off | [System notes](#system-notes) |
| Overwrite missing fields | off | [Missing, empty and read-only values](#missing-empty-and-read-only-values) |
| Validate mandatory custom fields | on | [Missing, empty and read-only values](#missing-empty-and-read-only-values) |
| Overwrite sublists | off | [Sublist handling](#sublist-handling) |
| Ignore read-only fields | on | [Missing, empty and read-only values](#missing-empty-and-read-only-values) |
| Prevent duplicate records | off | [Duplicate prevention](#duplicate-prevention) |
| Custom multi-select delimiter | pipe | [Delimiters](#delimiters) |
| Decimal delimiter | period | [Delimiters](#delimiters) |
| Custom form | role's preferred form | [Custom form](#custom-form) |
| Run server SuiteScript and trigger workflows | the company preference | [Scripts and workflows](#scripts-and-workflows) |
| Use multi-threading | off | [Threads and queues](csv_import.multithreading.md) |
| Queue number | 1 | [Threads and queues](csv_import.multithreading.md) |

## Custom form

Without a custom form, the fields offered for mapping are those on the role's preferred form.
Picking a custom form changes that set. What then happens to the form stored on the record depends
on the kind of record and on the data handling mode:

| | Transaction forms | Entry forms (non-transaction records) |
|---|---|---|
| **Adding records** | The chosen form is saved on each new record. | The form is stored on the record only if that form is set to be stored with the record. |
| **Updating records** | The chosen form is not saved. Values for fields that are not on the form already stored on the transaction are skipped. | The form stored on the record is replaced, or cleared. |

- A form that can be used only as a popup cannot be used here: the import fails with an invalid
  custom form reference error.
- If the chosen custom form has since been deleted, the import quietly falls back to the preferred
  form.

## Delimiters

- **Multi-select delimiter.** Separates several values inside one multi-select cell. The default
  is the pipe character, and any single character is accepted. A value set on the job overrides the
  account preference. Choose a character that never appears inside your values; the
  [items errors, continued](csv_import.errors_items_more.md) page shows what goes wrong otherwise.
- **Decimal delimiter.** The decimal mark in numbers: a period (the default) or a comma. The job's
  value overrides both the account preference and the subsidiary preference.

## Duplicate prevention

- Applies to contacts, customers, partners and vendors only, and is off by default.
- Works only when the Duplicate Detection & Merge feature is enabled **and** duplicate detection is
  switched on for that record type.
- With the option on, a row that matches an existing record fails with an error saying a duplicate
  record was detected.
- With it off, the duplicate is created anyway and flagged so someone can review it later.
- It is not a guarantee: imports running at the same time, or rows processed on parallel threads,
  can still produce duplicates.

## Sublist handling

**Overwrite sublists** is off by default and matters only for Update and Add or Update imports.

- **On:** the sublist rows in the CSV replace all existing lines of that sublist on the record.
- **Off:** on a sublist with key fields, a CSV row whose key matches an existing line updates that
  line and every other row is added as a new line; on a sublist without keys, rows are simply
  appended.
- Even with the option on, an import cannot remove every line of a sublist.
- Turning it on cancels the effect of *Overwrite missing fields* for the sublists you mapped.
- Keep it off for item pricing and demand plans.

### Which sublists have keys

Whether the "Off" behaviour can update a line in place depends on whether the sublist has
selective-update key fields. The [csv_import_sublists](../gen/catalogs/csv_import_sublists.md)
catalog gives them per sublist, using the exact field labels. Some typical keys:

- address sublists of entities: `Internal ID` and `Label`;
- translation sublists: `Language`;
- transaction `Items` on sales and purchase transactions: `Line/Order Line` and `Item` (see
  [transaction line items](csv_import.record_types.md#transaction-line-items) for how these match);
- accounting book detail sublists: `Accounting Book`;
- item `Pricing`: `Currency`, `Quantity` and `Level`, depending on which pricing features are
  enabled;
- payment sublists: `Invoice` (customer payments) or `Bill` (vendor payments), with `Line` as an
  optional extra key.

For many sublists the catalog lists no key (for example sales teams, the paycheck sublists, or the
credit memo item and apply sublists); the source does not say how their lines are matched on
update. Two sublists are limited by data handling mode whatever this option says: credit card
sublists on customers, leads and prospects can be imported only when adding records (existing
credit card lines cannot be updated), and the contact access sublist of customers and partners is
available only when updating existing records. A full overview of these conditions is on
[record types](csv_import.record_types.md#per-sublist-conditions).

## Missing, empty and read-only values

- **Ignore read-only fields** (on by default): a value aimed at a read-only field is dropped
  without an error. It must be on when entities and their contacts are imported together.
- **Validate mandatory custom fields** (on by default): with it off, records save even when custom
  fields marked as required have no value.
- **Overwrite missing fields** (off by default): with it on, an empty cell in a mapped column clears
  that field on the record. Fields you did not map are never affected, whatever this option says.
  To clear a single field without turning this on, set that field to null in the field mapping
  instead ([field mapping](csv_import.field_mapping.md)).

> [!SOURCE-ERROR] The source page that describes overwrite missing fields opens with a sentence that
> is cut off, so part of its explanation is missing. The behaviour above comes from the rest of the
> source; test it on a sample record before relying on it.
<!-- source-error: topic:csv_import.advanced_options | - | page_opening_sentence_truncated -->

## System notes

**Custom field system notes** is off by default. While it is off, changes the import makes to
custom fields are not written to system notes; changes to standard fields are still logged. The
source recommends leaving it off for Add imports.

## Scripts and workflows

**Run server SuiteScript and trigger workflows** starts from the company preference. When it is
off, user event scripts and workflows do not run for the imported records.

- An administrator can lock this option for a role, so users with that role cannot change it.
- Skipping scripts carries a risk: if the account relies on user events or workflows to keep data
  consistent, records imported without them can end up corrupted.
- The permissions needed to change the option on a job are on
  [jobs, status and preferences](csv_import.jobs.md).

## Threads and queues

*Use multi-threading* and *Queue number* are explained in
[threads and queues](csv_import.multithreading.md), including the license they need.
