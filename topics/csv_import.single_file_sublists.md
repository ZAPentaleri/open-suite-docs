# Single-file layout of CSV sublist rows
<!-- id: topic:csv_import.single_file_sublists -->

Part of: [CSV import](csv_import.md)

When a record and its sublist lines share one CSV file, the sublist data takes one of two shapes.
You do not pick the shape: each record type has a fixed one.

## The two layouts

| Layout | How lines appear | Suits |
|---|---|---|
| Multi-line | One row per sublist line; the rows of one record sit one after another. | Records whose number of lines varies. |
| Multi-column | One row per record, with the sublist's fields repeated as numbered copies of the columns. | Records with a small, fixed number of lines. |

The source does not state the naming pattern for the numbered column copies of the multi-column
layout. Map those columns in the Import Assistant and check the result on a sample file.

The record types and their sublists are listed in
[record types, sublists and subrecords](csv_import.record_types.md) and, with each sublist's
conditions, in [csv_import_sublists](../gen/catalogs/csv_import_sublists.md).

## Rules for multi-line files

- For new records, put the external ID on every row so the import can tell which rows belong to
  the same record. If neither an external ID nor an internal ID is mapped, the import fails with an
  error saying a unique identifier must be mapped.
- Body-field values are read from a record's first row only; the same columns on its later rows are
  ignored.
- Gaps in sublist data may appear only at the end of a record's block of rows, not between rows
  that carry sublist data.
- With multi-threading on, a record's rows must be consecutive (see
  [threads and queues](csv_import.multithreading.md)).

## Line counts per record

Some sublists set a minimum number of lines, which in a multi-line file means a minimum number of
rows per record:

- A new cash sale, estimate, invoice, purchase order, sales order, vendor bill, vendor credit or
  vendor return authorization needs at least one item line. The same minimum of one line applies
  to statistical journal entry lines, manufacturing routing steps and manufacturing cost template
  cost detail on new records.
- A journal entry needs at least 2 lines; intercompany and advanced intercompany journal entries
  need at least 4. The lines must balance, and the intercompany kinds need a debit and a credit for
  both the originating and the receiving subsidiary.

The full per-sublist list is in [csv_import_sublists](../gen/catalogs/csv_import_sublists.md).

## Splitting sublists into separate files

- For sublist updates, separate files are recommended.
- Once an import uses more than one file, the primary file carries only the body fields, and every
  other file refers back to the record by its ID.
- Moving one sublist into its own file means every sublist of that import has to move too.
- Multi-line sublist rows are not supported in the primary file when linked files are used. The
  source gives no further detail; test the combination on a sample before relying on it.

### Sublists that must come from their own file

The sublist catalog marks some sublists as requiring a separate CSV file:

- the `Expenses` sublist of purchase orders, vendor bills, vendor credits and vendor return
  authorizations (purchase order expenses also need the preference
  `Allow Expenses on Purchase Orders`), and
- the `Apply` sublist of vendor credits.

Taken together with the rule above that moving one sublist out moves all of them, an import that
includes one of these sublists has its sublists in linked files rather than in the primary file.

This sits awkwardly with the caveat about multi-line sublists and linked files: expense and apply
are both listed as multi-line sublists. The source does not reconcile the two statements; confirm
the file arrangement for these transactions with a small test import.

Other per-sublist conditions (required features, key fields for selective updates, add-only and
update-only sublists, which imports offer a sublist) are summarized on
[record types](csv_import.record_types.md#per-sublist-conditions).

## Example: a multi-line file

An invented sales order file in the multi-line layout. Column names are your own and are matched to
fields during mapping. The order-level columns are filled on each order's first row only, and
every row carries the external ID.

```csv
Order Ref,Customer,Order Date,Item,Quantity
SO-WEB-5521,Lakeside Dental Supply,11/02/2026,Nitrile Gloves L,40
SO-WEB-5521,,,Disposable Bibs,200
SO-WEB-5522,Corbel Veterinary,11/03/2026,Suture Kit,12
```

## Example: a vendor bill import with linked files

A RESTlet that receives vendor bill headers and expense lines as two CSV texts and queues one
import. The primary file holds the bill body fields; the expense lines go in as a linked file keyed
by the sublist's internal ID (`expense` on the vendor bill record). The saved map ID is invented and
must point to a map built in the Import Assistant for this two-file arrangement.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType Restlet
 */
define(['N/task', 'N/log'], (task, log) => {
  const post = (body) => {
    if (!body.billsCsv || !body.expensesCsv) {
      return { ok: false, reason: 'billsCsv and expensesCsv are both required' };
    }

    const importTask = task.create({
      taskType: task.TaskType.CSV_IMPORT,
      mappingId: 'CUSTIMPORT_freight_bills',
      importFile: body.billsCsv,
      linkedFiles: { expense: body.expensesCsv },
      name: `Freight bills from carrier portal (${body.batchRef || 'no ref'})`
    });

    const taskId = importTask.submit();
    log.audit({ title: 'Freight bill import queued', details: taskId });
    return { ok: true, taskId };
  };

  return { post };
});
```
