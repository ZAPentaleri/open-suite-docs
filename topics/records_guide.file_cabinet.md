# File Cabinet records
<!-- id: topic:records_guide.file_cabinet -->

Part of: [SuiteScript records guide](records_guide.md)

The File Cabinet is NetSuite's document store. Two record types describe its contents.

## Record types in this group

| Record ID | Reference page |
|---|---|
| `file` | [file](../gen/records/file.md) |
| `folder` | [folder](../gen/records/folder.md) |

The spec says nothing more about this group than its members. For reading, writing and saving file
contents from a script, use the [N/file](../modules/N_file.md) module; the record pages above give
the field IDs you can search on.

## Example

Find the active subfolders of a given folder by name. Field IDs come from the
[folder](../gen/records/folder.md) page; the parent folder ID is illustrative.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType ScheduledScript
 */
define(['N/search', 'N/log'], (search, log) => {
  const ARCHIVE_ROOT = 4210;

  const execute = () => {
    const folders = search.create({
      type: search.Type.FOLDER,
      filters: [
        ['parent', search.Operator.ANYOF, [ARCHIVE_ROOT]],
        'AND',
        ['isinactive', search.Operator.IS, 'F']
      ],
      columns: ['name']
    });

    const names = [];
    folders.run().each((row) => {
      names.push(row.getValue({ name: 'name' }));
      return true;
    });
    log.audit({ title: `Subfolders of ${ARCHIVE_ROOT}`, details: names.join(', ') });
  };

  return { execute };
});
```
