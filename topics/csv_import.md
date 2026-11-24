# CSV import
<!-- id: topic:csv_import -->

Sub-topics:

- [CSV import advanced options](csv_import.advanced_options.md)
- [CSV import errors: general and relationships](csv_import.errors_general.md)
- [CSV import errors: items](csv_import.errors_items.md)
- [CSV import errors: items, continued](csv_import.errors_items_more.md)
- [CSV import errors: transactions](csv_import.errors_transactions.md)
- [CSV import data handling and field mapping](csv_import.field_mapping.md)
- [CSV import file conventions](csv_import.file_conventions.md)
- [CSV import jobs, status and preferences](csv_import.jobs.md)
- [CSV import threads and queues](csv_import.multithreading.md)
- [CSV import record types, sublists and subrecords](csv_import.record_types.md)
- [Saved CSV import maps](csv_import.saved_imports.md)
- [Single-file layout of CSV sublist rows](csv_import.single_file_sublists.md)

See also: [`CsvImportTask`](../modules/N_task.md#csvimporttask-object) in N/task,
[csv_import_sublists](../gen/catalogs/csv_import_sublists.md),
[csv_import_subrecords](../gen/catalogs/csv_import_subrecords.md),
[permissions](../gen/catalogs/permissions.md).

CSV import loads record data in bulk: you hand it CSV files and it creates new records or updates
existing ones in volume. People use it through the Import Assistant, which is aimed at small and
medium-sized data sets. Scripts reach the same engine through
[`N/task.CsvImportTask`](../modules/N_task.md#csvimporttask-object) (see
[Running imports from SuiteScript](#running-imports-from-suitescript)).

## Who can run it

- The user needs the `Import CSV File` permission (`ADMI_IMPORTCSVFILE`). Working in the Import
  Assistant requires that permission at the Full level.
- When a job finishes, the notification email is sent to the address the importing user logged in
  with for that session.

## The Import Assistant

The assistant walks through these steps in order:

1. Scan and upload the file or files.
2. Choose the import options (data handling and the advanced options).
3. File mapping. You only see this step when the import uses more than one file.
4. Field mapping.
5. Save the mapping and start the job.

Steps 2 to 5 are covered in the sub-topics: [advanced options](csv_import.advanced_options.md),
[data handling and field mapping](csv_import.field_mapping.md), [saved maps](csv_import.saved_imports.md)
and [jobs](csv_import.jobs.md).

### Upload modes

An import uses either a single file, or one primary file plus linked files. In the linked-file mode
the record's main (body) fields have to be in the primary file, and the linked files hold sublist or
subrecord data only. A saved map remembers which mode it was built for. If you later run it with
the files arranged the other way, the job does not stop with an error; the data that does not fit
the map's mode is skipped.

A single file can hold at most 25,000 records. The per-job size limit is on the
[file conventions](csv_import.file_conventions.md) page.

### Import types

The assistant sorts record types into these import types: Accounting, Activities, Classification,
Communications, Custom Records, Customization, Employees, Items, Relationships, Rules Setup,
Subscription Billing, Supply Chain, Support, Transactions and Website. Which record types show up
under each one depends on the account; see [record types](csv_import.record_types.md).

Project data goes in through the Jobs import, which belongs to the Relationships import type.

Apart from the assistant, the source names one specialized import, for Peachtree CSV files.

> [!SOURCE-ERROR] A question-and-answer passage in the source calls some record types unsupported
> (credit memos, return authorizations, and support cases, events and messages), while other parts
> of the source treat them as importable; the sublist catalog, for instance, lists credit memo
> sublists. The source does not say which is right; check the import types your own account offers.
<!-- source-error: topic:csv_import | - | faq_calls_supported_types_unsupported -->

### Character encoding

Accepted encodings: UTF-8, Windows-1252, ISO-8859-1, GB18030, GBK, Big5, Shift_JIS, MacRoman,
ISO-2022-KR and EUC-KR. US-edition accounts start with Windows-1252 selected. UTF-8 files are
accepted with or without a byte-order mark.

### Column delimiter

Columns may be separated by comma, semicolon, pipe, space or tab; comma is the default. A
delimiter picked for a job wins over both the account preference and the subsidiary preference.

If the file contains characters the upload cannot handle, the upload stops with an error and hands
back a zip archive. The file inside has an extra column that explains the problem on each affected
row.

## What cannot go through this path

- Published saved searches cannot be imported.
- Budgets, single journal entries, single inventory worksheets, project tasks and website
  redirects use a separate two-step assistant, and scripted imports cannot handle any of them.
- Bulk order fulfillment, vendor price lists, employee commission data and single-use coupon codes
  are loaded only through their own templates, outside the Import Assistant.
- For a record type CSV import does not support, or for a large migration that keeps running over
  time, the source points to REST web services instead.

## Sublists in an import

Records with sublists (transaction lines, addresses, pricing, translations and so on) can carry
their sublist data in the same file or in linked files. The general rules are on
[record types](csv_import.record_types.md) and [single-file layout](csv_import.single_file_sublists.md).
Only some sublists have key fields that let an update match and change existing lines; on the
others an update can only append lines or replace the whole sublist.

Each importable sublist has its own conditions, listed per sublist in
[csv_import_sublists](../gen/catalogs/csv_import_sublists.md). The kinds of conditions you will
find there:

- **Features and preferences** that must be on before the sublist appears, written with their
  exact NetSuite names: for example `Multi-Language` for translation sublists,
  `Multi-Book Accounting` for accounting book sublists, and `SuiteTax` for tax registrations. Some
  sublists accept any one of several features, and one (job addresses) needs
  `Project Management` to be **off**.
- **Selective-update key fields**: the columns used to match a CSV row to an existing line.
- **Add-only or update-only sublists**: customer, lead and prospect credit cards can only be added
  with new records, and the contact access sublist of customers and partners exists only for
  updates of existing records.
- **Separate-file requirements**: the expenses sublists of purchase orders, vendor bills, vendor
  credits and vendor return authorizations, and the vendor credit apply sublist, must come from a
  file of their own.
- **Import types**: some item sublists are offered only in certain item imports, and the contact
  address sublist only in the contacts-only import.
- **Minimum lines**: for example at least one item line on a new sales order, two lines on a
  journal entry and four on an intercompany journal entry.

[Record types, sublists and subrecords](csv_import.record_types.md#per-sublist-conditions) walks
through the recurring patterns and the unusual cases.

## Running imports from SuiteScript

A script runs an import with [`N/task.CsvImportTask`](../modules/N_task.md#csvimporttask-object),
created through `task.create` with the CSV import task type. The task cannot carry a mapping of
its own: it relies on a **saved import map** that someone built and saved in the Import Assistant,
and the script names that map by its ID. The two-step import types listed above are out of its
reach. Queue and threading behaviour is described in [threads and queues](csv_import.multithreading.md),
and following a submitted job in [jobs](csv_import.jobs.md).

## Planning a migration

- **Import in dependency order:** chart of accounts, then classifications, then entities, then
  items, then CRM records, and transactions last.
- **One CSV file per record type.** Do not put several record types in one file.
- **Keep the old system's keys.** Map each unique ID from the source system to the record's
  external ID, so later imports and updates can find the same records.
- **Pick the strongest update key.** Internal ID is the most reliable, external ID comes next, and
  name is the weakest. Name keys break easily because hierarchy formatting or spacing in the file
  often differs from what NetSuite holds.
- Only a subset of sublists has keys for updates (see [Sublists in an import](#sublists-in-an-import)).
- The fields you can map are the editable fields on the role's preferred form, or on the custom
  form you choose in the advanced options.

### External IDs

The external ID is not displayed on records in the UI and cannot be edited there. A CSV import,
SuiteScript or REST web services can set it, and a saved search can read it back.

### Useful saved-search tricks

- To get the internal IDs of existing records, run a saved search and export the results to CSV.
- To find names that will not match because of stray spaces, export them with a saved search and
  inspect the export.

## Entities

- **Leads, prospects and customers** all import into the customer record; the stage field decides
  which one each row becomes. A record moves between stages through related activity: a lead
  becomes a prospect with an estimate or an opportunity, a lead becomes a customer with a sales
  transaction, and a prospect becomes a customer with a sales transaction or a closed opportunity.
- **Customers and contacts** can be imported in one job or separately; doing them together is the
  more involved route.
- **Individual or company.** Customer, partner and vendor imports require the field that says
  whether the entity is an individual. It reads `true`, `t` and `yes`, in any letter case, as "this
  entity is a person". When it is false, a company name is required. You can give the field a
  default in field mapping. Note that the account's default entity type preferences may set the
  individual flag for you.

> [!SOURCE-ERROR] The source's answers about the individual field and about subscription status
> contain what look like typing mistakes. Rely on the rules as described on this page and confirm
> the exact values with a test import.
<!-- source-error: topic:csv_import | - | typos_in_individual_and_subscription_answers -->

### Global subscription status

- Of the global subscription status values, an import can set only soft opt-in and soft opt-out.
  The confirmed statuses can be set only by the recipient.
- When the column is not imported, the record gets soft opt-in if the preference that makes new
  records unsubscribed by default is off (its default), and soft opt-out if that preference is on.
- A record must be soft opted in to receive campaign email.

## Items and pricing

- If no price level is mapped, the item's base price is used.
- If your file has no price level column, you can map the price level with a default value of
  "Custom", but that default is not valid for every item.
- Importing item pricing needs at least one of these features: `Multiple Prices`,
  `Quantity Pricing` or `Multiple Currencies`. Any one of them is enough.
- The recommended layout puts item pricing in its own linked file.
- Which fields act as keys for pricing rows depends on which of those features are enabled. The
  sublist catalog names the candidates as `Currency`, `Quantity` and `Level`.

## Accounting data

- **Chart of accounts.** The importable fields are account number, name, type, description and
  currency. Account balances are not imported. Bring opening balances in as journal entries dated
  at go-live, using the journal entry import template.
- **Journal entry numbers.** In a single journal entry import the entry number is not required,
  because a default is filled in; you can see it through the edit icon before running the import.
  In an import of multiple journal entries the entry number is required.

## Scripts and workflows during imports

Workflows count as server SuiteScript here. They run during an import only when the option to run
server SuiteScript and trigger workflows is on (see [advanced options](csv_import.advanced_options.md)).

> [!SOURCE-ERROR] In at least one place the source gives this preference a name with an extra word
> ("CSV") that the option does not otherwise carry. Look for the preference about running server
> SuiteScript and triggering workflows, whatever its exact label in your account.
<!-- source-error: topic:csv_import | - | preference_name_has_extra_word_csv -->

## Example

A scheduled script that feeds a nightly bin list from the File Cabinet through a saved map, then
logs the task ID and its first status so a later run can check on it. The map script ID and the
file path are invented; the map must already exist in the account.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType ScheduledScript
 */
define(['N/task', 'N/file', 'N/log'], (task, file, log) => {
  const MAP_ID = 'CUSTIMPORT_warehouse_bins';

  const execute = () => {
    const binsCsv = file.load({ id: 'Imports/bins_east_wing.csv' });

    const importTask = task.create({
      taskType: task.TaskType.CSV_IMPORT,
      importFile: binsCsv,
      mappingId: MAP_ID,
      name: `Bin refresh ${new Date().toISOString().slice(0, 10)}`
    });

    const taskId = importTask.submit();
    const state = task.checkStatus({ taskId }).status;
    log.audit({ title: 'Bin import queued', details: `${taskId} is ${state}` });
  };

  return { execute };
});
```
