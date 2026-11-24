# N/file
<!-- id: N/file -->

`N/file` is how server scripts work with the File Cabinet: create a file in memory, load an
existing one, read or stream its contents, save it to a folder, copy it, or delete it. A file built
in memory does not have to be saved at all; it can be attached straight to an email or fax.

- **Availability:** server scripts only.
- **Load it as:** `define(['N/file'], (file) => { ... })`
- Related: [N/encode](N_encode.md) (character encodings), [N/render](N_render.md) (produces
  `file.File` objects from templates), [Iterator](../globals.md) (the iterator returned by
  `File.getSegments`).

Points that apply across the module:

- **Typical jobs.** Uploading files to the File Cabinet, attaching files to email without saving
  them first, and copying files while controlling what happens on a name clash.
- **Size limits.** Holding content in memory (`file.create` contents, `File.getContents`) is capped
  at 10 MB. Saving is not subject to that cap when the content is streamed: `File.save` streams,
  so files of any size the File Cabinet accepts can be written. For large text or CSV files, build
  them with `File.appendLine` and read them with `File.lines.iterator`, the Reader, or
  `File.getSegments` instead of loading the whole string.
- **Line-by-line growth.** A file that keeps growing through `appendLine` can slow processing down;
  splitting the output across several files is recommended in that case.
- **Unique names.** When a script writes many files, put a millisecond timestamp or a random number
  into each file name so names do not collide.
- **Malware screening.** Every upload and update is screened for malicious content.
- **Custom segmentation and special reads.** `File.getSegments` splits content on a separator of
  your choosing; `File.getReader` gives a Reader with `readChars` and `readUntil`.
- **Online files.** `isOnline` set to `true` makes the file downloadable without logging in.

## Members

| Member | Kind | Summary |
|---|---|---|
| [`file.create(options)`](#filecreateoptions) | function | Build a new file in memory |
| [`file.load(options)`](#fileloadoptions) | function | Load a File Cabinet file by id or path |
| [`file.copy(options)`](#filecopyoptions) | function | Copy a file into a folder, with name-conflict handling |
| [`file.delete(options)`](#filedeleteoptions) | function | Remove a file from the File Cabinet |
| [`File`](#file-object) | object | A file: metadata plus methods to read, stream and save content |
| [`Reader`](#reader-object) | object | Reads a file by delimiter or by character count |
| [`file.Type`](#filetype) | enum | File types |
| [`file.Encoding`](#fileencoding) | enum | Character encodings |
| [`file.NameConflictResolution`](#filenameconflictresolution) | enum | What `copy` does when the target name exists |

## Functions

### file.create(options)
<!-- id: N/file.create -->

Builds a new `file.File` in memory. Nothing is written to the File Cabinet until you call
[`File.save()`](#filesave), and you may never need to (for example, if you only attach the file to
an email). In-memory content is limited to 10 MB.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.name` | string | Yes | |
| `options.fileType` | enum | Yes | |
| `options.contents` | string | No | |
| `options.folder` | number | No | |
| `options.encoding` | string | No | |
| `options.isInactive` | boolean | No | `false` |
| `options.isOnline` | boolean | No | `false` |
| `options.description` | string | No | |

- `options.name`: the file name. Since 2015.2.
- `options.fileType`: a [`file.Type`](#filetype) value. It becomes the file's
  [`fileType`](#filefiletype), and this is the only place it can be set; it cannot be changed after
  creation. Since 2015.2.
- `options.contents`: loaded lazily. Content for binary file types must be Base64-encoded. Since
  2015.2.
- `options.folder`: the internal id of the destination folder; sets [`File.folder`](#filefolder).
  You need a folder before you can save. Since 2016.1.
- `options.encoding`: a [`file.Encoding`](#fileencoding) value. See also [N/encode](N_encode.md).
  Since 2016.2.
- `options.isInactive`: sets [`File.isInactive`](#fileisinactive). An inactive file is hidden in the
  File Cabinet unless the user chooses to show inactive items. Since 2016.2.
- `options.isOnline`: sets [`File.isOnline`](#fileisonline). Since 2016.2.
- `options.description`: text that appears in the Description field of the file record. Since
  2016.2.

**Returns:** `file.File`, the new (unsaved) file object.

**Errors:**

- `SSS_MISSING_REQD_ARGUMENT`: a required option is missing.
- `SSS_INVALID_TYPE_ARG`: `fileType` is not a valid type (wrong argument type).
- `SSS_FILE_CONTENT_SIZE_EXCEEDED`: the content is larger than 10 MB.

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2015.2

### file.load(options)
<!-- id: N/file.load -->

Loads an existing File Cabinet file, up to 2048 MB in size. Loading gives you the metadata; the
content itself is fetched only when you ask for it (see [`File`](#file-object)).

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.id` | number \| string | Yes | |

- `options.id` accepts three forms: the file's internal id, an absolute File Cabinet path, or a
  path relative to the folder the script lives in. For instance `Exports/daily.csv`,
  `./Exports/daily.csv` and `../Exports/daily.csv` are all valid path shapes. Internal ids are shown
  in the UI under Documents > Files > File Cabinet. Since 2015.2.

**Returns:** `file.File`

**Errors:**

- `INSUFFICIENT_PERMISSION`: the internal id is not valid.
- `RCRD_DSNT_EXIST`: the relative path does not resolve to a file.
- `SSS_MISSING_REQD_ARGUMENT`: `options.id` is missing.

**Governance:** 10 units.

**Availability:** server scripts.

**Since:** 2015.2

### file.copy(options)
<!-- id: N/file.copy -->

Copies a file into a folder. The copy carries the same properties as the original; for example, a
file that is available without login stays that way after copying. If a file of the same name
already exists in the target folder, `conflictResolution` decides what happens.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.id` | number | Yes | |
| `options.folder` | number | Yes | |
| `options.conflictResolution` | string | No | `FAIL` |

- `options.id`: internal id of the file to copy; the file must exist. Since 2021.1.
- `options.folder`: internal id of the destination folder; the folder must exist. Since 2021.1.
- `options.conflictResolution`: a [`file.NameConflictResolution`](#filenameconflictresolution)
  value. Since 2021.1.

**Returns:** `file.File`, the copy.

**Errors:**

- `INVALID_CONFLICT_RESOLUTION_1`: `options.conflictResolution` is not one of the
  `file.NameConflictResolution` values.
- `SSS_MISSING_REQD_ARGUMENT`: a required option is missing.

**Governance:** 20 units.

**Availability:** server scripts.

**Since:** 2021.1

### file.delete(options)
<!-- id: N/file.delete -->

Permanently removes a file from the File Cabinet. You can find a file's internal id in the UI under
Documents > Files > File Cabinet.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.id` | number \| string | Yes | |

- `options.id`: internal id of the file to delete. Since 2015.2.

**Returns:** `void`

**Errors:**

- `SSS_MISSING_REQD_ARGUMENT`: `options.id` is missing.

**Governance:** 20 units.

**Availability:** server scripts.

**Since:** 2015.2

## Objects

### File (object)
<!-- id: N/file.File -->

A file, whether newly created or loaded. The object itself holds only metadata; the content is
pulled in when you call [`getContents()`](#filegetcontents). Things to keep in mind:

- Binary content must be Base64-encoded.
- At creation time a file can hold at most 10 MB.
- You can save it to the File Cabinet, or attach it to an email or fax without saving.
- [`folder`](#filefolder) must be set before [`save()`](#filesave).
- For line-level work on text and CSV files, use [`appendLine`](#fileappendlineoptions) to write
  and [`lines.iterator`](#filelinesiteratorlinecontext) to read.

Server scripts; since 2015.2.

#### File.getContents()
<!-- id: N/file.File#getContents -->

Returns the whole file content as a string. Content is held in memory, so this is limited to
10 MB; check [`size`](#filesize) first if you are not sure. For text or CSV files larger than that,
use [`appendLine`](#fileappendlineoptions) and [`lines.iterator`](#filelinesiteratorlinecontext)
instead.

**Returns:** `string`

**Errors:**

- `SSS_FILE_CONTENT_SIZE_EXCEEDED`: the file is larger than 10 MB.

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2015.2

#### File.save()
<!-- id: N/file.File#save -->

Writes the file to the File Cabinet: uploads it if it is new, or stores the changes if it already
exists. Saving streams the content, so the 10 MB in-memory limit does not apply; any size the File
Cabinet allows will work. [`folder`](#filefolder) must be set first.

**Returns:** `number`, the file's internal id.

**Errors:**

- `INVALID_KEY_OR_REF`: no folder is set, or the script does not have permission for the folder.
- `SSS_MISSING_REQD_ARGUMENT`: a required value is missing.

**Governance:** 20 units.

**Availability:** server scripts.

**Since:** 2015.2

#### File.appendLine(options)
<!-- id: N/file.File#appendLine -->

Adds a line at the end of a text or CSV file. Each line can be up to 10 MB. Reading and writing
use separate streams, and once you have started reading (through
[`lines.iterator`](#filelinesiteratorlinecontext)) you cannot append. To switch from reading to
writing, call [`resetStream()`](#fileresetstream) or [`save()`](#filesave) first.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.value` | string | Yes | |

- `options.value`: the text of the line. Since 2017.1.

**Returns:** `file.File`, the file object (so calls can be chained).

**Errors:**

- `SSS_FILE_CONTENT_SIZE_EXCEEDED`: the line exceeds the size limit.
- `YOU_CANNOT_WRITE_TO_A_FILE_AFTER_YOU_BEGAN_READING_FROM_IT`: called after line-by-line reading
  has begun on this file.

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2017.1

#### File.lines.iterator(lineContext)
<!-- id: N/file.File#lines.iterator -->

Streams a text or CSV file line by line, passing each line to a callback, so you never need the
whole file in memory. The callback controls the loop: return `true` to continue to the next line,
`false` to stop. At the end of the file the iteration yields `false`. Each line can be up to 10 MB.
Once you have appended with [`appendLine`](#fileappendlineoptions) you cannot start reading; call
[`resetStream()`](#fileresetstream) or [`save()`](#filesave) before switching from writing to
reading.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `lineContext` | iterator | Yes | |

- `lineContext`: the spec lists this single parameter of type `iterator` (since 2017.1) and does
  not describe the shape of the per-line value handed to the callback.

**Returns:** `boolean`

> [!SOURCE-ERROR] The source's parameter table describes a `lineContext` parameter with a boolean
> return, while its syntax instead obtains an iterator and walks it with `each` (see
> [Iterator](../globals.md)). The two shapes do not match; verify the calling form in your account
> before relying on either.
<!-- source-error: N/file.File#lines.iterator | - | table_param_boolean_return_vs_syntax_iterator_each -->

**Errors:**

- `SSS_FILE_CONTENT_SIZE_EXCEEDED`: a line exceeds the size limit.
- `YOU_CANNOT_READ_FROM_A_FILE_AFTER_YOU_BEGAN_WRITING_TO_IT`: called after `appendLine` has been
  used on this file.

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2017.1

#### File.resetStream()
<!-- id: N/file.File#resetStream -->

Puts a text or CSV file's streams back to their starting state: lines appended since the last save
are discarded, both the read and write streams are reset, and the line pointer returns to the
start. Use it to move between reading and writing without saving. The 10 MB-per-line limit applies
to these files.

**Returns:** `void`

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2017.1

#### File.getReader()
<!-- id: N/file.File#getReader -->

Returns a [`file.Reader`](#reader-object) for reads that are not line-based: up to a delimiter, or a
fixed number of characters at a time.

**Returns:** `file.Reader`

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2019.1

#### File.getSegments(options)
<!-- id: N/file.File#getSegments -->

Splits the file into segments using a separator you choose and returns an iterator over them. Each
segment includes the separator that ends it. With a newline separator you get the file line by line;
other separators let you define your own record boundaries.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.separator` | string | Yes | |

- `options.separator`: must be a string and must not be empty. A newline (`'\n'`) is the obvious
  choice when you want lines. Since 2019.1.

**Returns:** `Iterator`, the general SuiteScript [Iterator](../globals.md). It can be consumed in
either of two ways:

- call `next()` repeatedly; each call gives an object with `value` (the segment) and `done`, and
  `done` becoming `true` means there are no more segments (the Iterator follows the ECMAScript
  iterator protocol);
- call `each(callback)`; the callback takes one argument (in the source's illustration a wrapper
  object whose `value` property holds the item) and returns `true` to move to the next segment or
  `false` to stop early.

**Errors:**

- `SSS_INVALID_SEGMENT_SEPARATOR`: `options.separator` is empty or missing.
- `SSS_INVALID_ARG_TYPE`: `options.separator` is not a string.
- `SSS_MISSING_REQD_ARGUMENT`: a required argument is missing.

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2019.1

#### File.description
<!-- id: N/file.File#description -->

`string`. Text shown in the Description field of the file record. Since 2015.2.

#### File.folder
<!-- id: N/file.File#folder -->

`number | string`. Internal id of the File Cabinet folder that holds (or will hold) the file. It
must be set before [`save()`](#filesave). Since 2015.2.

#### File.id
<!-- id: N/file.File#id -->

`number`. The file's internal id in the File Cabinet, assigned by NetSuite. Read-only: assigning it
throws `READ_ONLY_PROPERTY`. Since 2015.2.

#### File.name
<!-- id: N/file.File#name -->

`string`. The file name. Since 2015.2.

#### File.size
<!-- id: N/file.File#size -->

`number`. The file size in bytes, including lines appended through streaming. Check it before
calling [`getContents()`](#filegetcontents) to stay within that method's 10 MB limit. Read-only:
assigning it throws `READ_ONLY_PROPERTY`. Since 2015.2.

#### File.fileType
<!-- id: N/file.File#fileType -->

`enum`. The file's type, one of the [`file.Type`](#filetype) values. It can only be set through
[`file.create`](#filecreateoptions); assigning it afterwards throws `READ_ONLY_PROPERTY`. Since
2015.2.

#### File.isText
<!-- id: N/file.File#isText -->

`boolean`. `true` when the file type is text-based. Read-only: assigning it throws
`READ_ONLY_PROPERTY`. Since 2015.2.

#### File.url
<!-- id: N/file.File#url -->

`string`. The file's URL. Read-only: assigning it throws `READ_ONLY_PROPERTY`. Since 2015.2.

#### File.path
<!-- id: N/file.File#path -->

`string`. The file's path relative to the File Cabinet. Until a folder is set, it is just the file
name. Read-only: assigning it throws `READ_ONLY_PROPERTY`. Since 2015.2.

#### File.isInactive
<!-- id: N/file.File#isInactive -->

`boolean`, default `false`. `true` marks the file inactive, which hides it in the File Cabinet
unless the user chooses to show inactive items. Since 2015.2.

#### File.isOnline
<!-- id: N/file.File#isOnline -->

`boolean`, default `false`. `true` lets anyone download the file without logging in, which is how
media for SuiteCommerce sites can be served without a login. This is a property of the file and is
unrelated to the "available without login" setting on a Suitelet deployment (see
[Suitelet deployment](../topics/suitelet.deployment.md)). Since 2015.2.

#### File.encoding
<!-- id: N/file.File#encoding -->

`string`. The file's character encoding, one of the [`file.Encoding`](#fileencoding) values. Since
2015.2.

### Reader (object)
<!-- id: N/file.Reader -->

A reader returned by [`File.getReader()`](#filegetreader). It reads forward through the file
either up to a delimiter or a set number of characters at a time. Server scripts; since 2019.1.

#### Reader.readUntil(options)
<!-- id: N/file.Reader#readUntil -->

Reads from the current position up to the next occurrence of `tag`. If the tag never appears again,
you get the rest of the file. Once everything has been read, it returns `null`.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.tag` | string | Yes | |

- `options.tag`: the delimiter to read up to; must not be empty. Since 2019.1.

**Returns:** `string`, or `null` when the reader has reached the end.

**Errors:**

- `SSS_TAG_CANNOT_BE_EMPTY`: `options.tag` is empty or missing.
- `SSS_INVALID_ARG_TYPE`: `options.tag` is not a string.
- `SSS_MISSING_REQD_ARGUMENT`: a required argument is missing.

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2019.1

#### Reader.readChars(options)
<!-- id: N/file.Reader#readChars -->

Reads the next `number` characters. Near the end of the file you may get fewer; after everything
has been read, it returns `null`.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.number` | number | Yes | |

- `options.number`: how many characters to read; must be positive. Since 2019.1.

**Returns:** `string`, or `null` when the reader has reached the end.

**Errors:**

- `SSS_INVALID_READ_SIZE`: `options.number` is not a positive number.
- `SSS_INVALID_ARG_TYPE`: `options.number` is not a number.
- `SSS_MISSING_REQD_ARGUMENT`: a required argument is missing.

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2019.1

## Enums

### file.Type
<!-- id: N/file.Type -->

File types accepted by `options.fileType` of [`file.create`](#filecreateoptions) and reported by
[`File.fileType`](#filefiletype). Since 2015.2. The spec gives names only, without descriptions.

All 41 values:

| Value |
|---|
| `APPCACHE` |
| `AUTOCAD` |
| `BMPIMAGE` |
| `CERTIFICATE` |
| `CONFIG` |
| `CSV` |
| `EXCEL` |
| `FLASH` |
| `FREEMARKER` |
| `GIFIMAGE` |
| `GZIP` |
| `HTMLDOC` |
| `ICON` |
| `JAVASCRIPT` |
| `JPGIMAGE` |
| `JSON` |
| `MESSAGERFC` |
| `MP3` |
| `MPEGMOVIE` |
| `MSPROJECT` |
| `PDF` |
| `PJPGIMAGE` |
| `PLAINTEXT` |
| `PNGIMAGE` |
| `POSTSCRIPT` |
| `POWERPOINT` |
| `QUICKTIME` |
| `RTF` |
| `SCSS` |
| `SMS` |
| `STYLESHEET` |
| `SVG` |
| `TAR` |
| `TIFFIMAGE` |
| `VISIO` |
| `WEBAPPPAGE` |
| `WEBAPPSCRIPT` |
| `WORD` |
| `XMLDOC` |
| `XSD` |
| `ZIP` |

### file.Encoding
<!-- id: N/file.Encoding -->

Character encodings for [`file.create`](#filecreateoptions) (`options.encoding`) and
[`File.encoding`](#fileencoding). Since 2015.2.

| Value | Character set family |
|---|---|
| `UTF_8` | Unicode |
| `WINDOWS_1252` | Western |
| `ISO_8859_1` | Western |
| `GB18030` | Chinese (simplified) |
| `SHIFT_JIS` | Japanese |
| `MAC_ROMAN` | Western |
| `GB2312` | Chinese (simplified) |
| `BIG5` | Chinese (traditional) |

### file.NameConflictResolution
<!-- id: N/file.NameConflictResolution -->

Tells [`file.copy`](#filecopyoptions) what to do when the destination folder already holds a file
with the same name. Since 2021.1.

| Value | Effect |
|---|---|
| `FAIL` | Throw an error and do not copy (the default) |
| `OVERWRITE` | Replace the existing file's content but keep its existing attributes and permissions |
| `OVERWRITE_CONTENT_AND_ATTRIBUTES` | Replace the content, attributes and permissions |
| `RENAME_TO_UNIQUE` | Keep both: a numeric suffix is added to the copy's name so it is unique |

For `RENAME_TO_UNIQUE`, the source's illustration is a text file whose copy gets a `1` added to its
name; the exact punctuation of the suffix is not stated.

## Examples

Writing a CSV export line by line and saving it with a timestamped name, so repeated runs never
collide. Appending keeps memory use flat however many rows there are.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType ScheduledScript
 */
define(['N/file', 'N/log'], (file, log) => {
  const EXPORT_FOLDER_ID = 812;

  const execute = () => {
    const rows = [
      { sku: 'BRK-220', bin: 'A-04', onHand: 37 },
      { sku: 'BRK-221', bin: 'A-05', onHand: 0 },
      { sku: 'HNG-090', bin: 'C-11', onHand: 152 }
    ];

    const exportFile = file.create({
      name: `bin_counts_${Date.now()}.csv`,
      fileType: file.Type.CSV,
      folder: EXPORT_FOLDER_ID,
      description: 'Nightly bin count snapshot'
    });

    exportFile.appendLine({ value: 'sku,bin,on_hand' });
    rows.forEach((r) => exportFile.appendLine({ value: `${r.sku},${r.bin},${r.onHand}` }));

    const fileId = exportFile.save();
    log.audit({ title: 'Bin count export saved', details: `file ${fileId}` });
  };

  return { execute };
});
```

Reading a large pipe-delimited feed record by record with a Reader. Each record ends with `~`, so
`readUntil` hands back one record at a time and returns `null` at the end.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType ScheduledScript
 */
define(['N/file', 'N/log'], (file, log) => {
  const execute = () => {
    const feed = file.load({ id: './Inbound/carrier_rates.txt' });
    const reader = feed.getReader();

    let count = 0;
    let chunk = reader.readUntil({ tag: '~' });
    while (chunk !== null) {
      const [lane, rate] = chunk.replace('~', '').split('|');
      if (lane) {
        count += 1;
        log.debug({ title: `Lane ${lane}`, details: rate });
      }
      chunk = reader.readUntil({ tag: '~' });
    }

    log.audit({ title: 'Rate records read', details: count });
  };

  return { execute };
});
```

Splitting a semicolon-separated list of serial numbers with `getSegments` and walking it with
`next()` until `done` is `true`. Each segment still ends with its `;`, so it is trimmed off.

```js
/**
 * @NApiVersion 2.1
 */
define(['N/file'], (file) => {
  const collectSerials = (fileId) => {
    const serialFile = file.load({ id: fileId });
    const segments = serialFile.getSegments({ separator: ';' });

    const serials = [];
    let step = segments.next();
    while (!step.done) {
      const serial = step.value.replace(';', '').trim();
      if (serial) {
        serials.push(serial);
      }
      step = segments.next();
    }
    return serials;
  };

  return { collectSerials };
});
```

Archiving a template by copying it into another folder without overwriting anything already there,
then removing the original.

```js
/**
 * @NApiVersion 2.1
 */
define(['N/file'], (file) => {
  const archiveTemplate = (templateFileId, archiveFolderId) => {
    const copy = file.copy({
      id: templateFileId,
      folder: archiveFolderId,
      conflictResolution: file.NameConflictResolution.RENAME_TO_UNIQUE
    });
    file.delete({ id: templateFileId });
    return copy.path;
  };

  return { archiveTemplate };
});
```
