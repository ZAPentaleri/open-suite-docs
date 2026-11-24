# SuiteScript 1.0 conversion samples (searches and imports)
<!-- id: topic:ss21_transition.ss1_samples_more -->

Part of: [Transitioning to SuiteScript 2.1](ss21_transition.md)

See also: [SuiteScript 1.0 conversion samples](ss21_transition.ss1_samples.md),
[CSV import](csv_import.md), [Converting SuiteScript 1.0 to 2.1](../conversion/ss1-to-ss21.md).

Two more vendor conversion samples, summarised for what they teach and for the mistakes the spec
found in them. Original conversion pairs follow.

## CSV import

Mappings shown:

| 1.0 | 2.1 |
|---|---|
| `nlapiLoadFile` | [`file.load`](../modules/N_file.md) |
| `nlapiCreateCSVImport` | [`task.create`](../modules/N_task.md) with the CSV import task type |
| `setMapping` | the task's `mappingId` property |
| `setPrimaryFile` | the task's `importFile` property |
| `setLinkedFile` | the task's `linkedFiles` property |
| `setOption` | the task's `name` property |
| `nlapiSubmitCSVImport` | the task's `submit` method |

The 2.1 sample refers to a `mappingFileId` variable it never declares and keys its linked files
with `addresbook` [sic].

> [!SOURCE-ERROR] The sample uses a mapping ID variable that is never defined, and spells the
> address book sublist key with a single "s". As printed it would fail; define the mapping ID
> yourself and take the linked-file key from the import's sublist IDs (see
> [catalog:csv_import_sublists](../gen/catalogs/csv_import_sublists.md)).
<!-- source-error: topic:ss21_transition.ss1_samples_more | - | csv_sample_uses_undefined_mappingfileid_and_addresbook_key -->

## Joined item search

Mapping shown: running a search with `nlapiSearchRecord` becomes calling
[`Search.run`](../modules/N_search.md) on a search object. The 2.1 sample, however, never creates
that search object before running it.

> [!SOURCE-ERROR] The converted sample calls `run` on a search it never built. Create the search
> first (with `search.create`, or `search.load` for a saved search) and run that object.
<!-- source-error: topic:ss21_transition.ss1_samples_more | - | joined_search_sample_never_creates_search -->

## Original pair: submitting a CSV import

1.0:

```js
function importVendorPriceFile(fileId, savedImportId) {
  var job = nlapiCreateCSVImport();
  job.setMapping(savedImportId);
  job.setPrimaryFile(nlapiLoadFile(fileId));
  return nlapiSubmitCSVImport(job);
}
```

2.1:

```js
/**
 * @NApiVersion 2.1
 */
define(['N/task', 'N/file'], (task, file) => {
  const importVendorPriceFile = (fileId, savedImportId) => {
    const job = task.create({ taskType: task.TaskType.CSV_IMPORT });
    job.mappingId = savedImportId;
    job.importFile = file.load({ id: fileId });
    job.name = `Vendor prices ${new Date().toISOString().slice(0, 10)}`;
    return job.submit();
  };

  return { importVendorPriceFile };
});
```

## Original pair: a search with a join

1.0, reading the customer's email through the `customer` join of a support case search:

```js
function urgentCaseContacts() {
  var filters = [new nlobjSearchFilter('priority', null, 'anyof', '1')];
  var columns = [new nlobjSearchColumn('title'), new nlobjSearchColumn('email', 'customer')];
  var rows = nlapiSearchRecord('supportcase', null, filters, columns) || [];
  var out = [];
  for (var i = 0; i < rows.length; i++) {
    out.push({ title: rows[i].getValue('title'), email: rows[i].getValue('email', 'customer') });
  }
  return out;
}
```

2.1, creating the search object before running it:

```js
/**
 * @NApiVersion 2.1
 */
define(['N/search'], (search) => {
  const urgentCaseContacts = () => {
    const caseSearch = search.create({
      type: search.Type.SUPPORT_CASE,
      filters: [['priority', 'anyof', '1']],
      columns: ['title', search.createColumn({ name: 'email', join: 'customer' })]
    });
    const out = [];
    caseSearch.run().each((row) => {
      out.push({
        title: row.getValue({ name: 'title' }),
        email: row.getValue({ name: 'email', join: 'customer' })
      });
      return true;
    });
    return out;
  };

  return { urgentCaseContacts };
});
```
