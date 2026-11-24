# N/compress
<!-- id: N/compress -->

`N/compress` does two things with `file.File` objects on the server: gzip/gunzip a single file, and
bundle several files into one archive (ZIP, TAR and related formats). Every function hands back a
**temporary** `file.File`; if you want to keep the result, give it a folder and save it with
[N/file](N_file.md).

- **Availability:** server scripts only.
- **Load it as:** `define(['N/compress'], (compress) => { ... })`
- Archive formats are listed in [`compress.Type`](#compresstype).
- **Size limit:** `gzip` and `Archiver.add` reject files larger than 680 MB with
  `COMPRESS_API_FILE_IS_TOO_LARGE`. The same rule also carries a note about a 2 GB limit for binary
  and ASCII files; the source does not state how that 2 GB figure relates to the 680 MB one, so
  keep inputs under 680 MB.
- Related: [N/file](N_file.md).

## Members

| Member | Kind | Summary |
|---|---|---|
| [`compress.gzip(options)`](#compressgzipoptions) | function | Gzip one file |
| [`compress.gunzip(options)`](#compressgunzipoptions) | function | Decompress a gzipped file |
| [`compress.createArchiver()`](#compresscreatearchiver) | function | Start building a ZIP/TAR-style archive |
| [`Archiver`](#archiver-object) | object | Collects files, then produces the archive |
| [`compress.Type`](#compresstype) | enum | Archive formats |

## Functions

### compress.gzip(options)
<!-- id: N/compress.gzip -->

Compresses one file with gzip and returns the compressed result as a temporary file.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.level` | number | No | |
| `options.file` | file.File | Yes | |

- `options.level`: 0 to 9, where 0 means no compression and 9 the strongest compression. The
  source does not state which level is used when you leave it out; pass a level explicitly if the
  result size matters.
- `options.file`: the file to compress.

**Returns:** `file.File`, a temporary compressed file.

**Errors:**

- `COMPRESS_API_COMPRESSION_LEVEL_OUT_OF_RANGE`: `options.level` is outside 0 to 9.
- `COMPRESS_API_UNABLE_TO_RETRIEVE_FILE_CONTENTS`: the input file's contents could not be read.
- `COMPRESS_API_FILE_IS_TOO_LARGE`: the input file is larger than 680 MB. A 2 GB limit for binary
  and ASCII files is mentioned alongside this rule; the source does not state how the two figures
  relate.

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2020.2

### compress.gunzip(options)
<!-- id: N/compress.gunzip -->

Decompresses a gzipped file and returns the result as a temporary file.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.file` | file.File | Yes | |

- `options.file`: the gzipped file to decompress.

**Returns:** `file.File`, a temporary decompressed file.

**Errors:**

- `COMPRESS_API_DECOMPRESS_ERROR`: decompression failed.
- `COMPRESS_API_UNABLE_TO_RETRIEVE_FILE_CONTENTS`: the input file's contents could not be read.

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2020.2

### compress.createArchiver()
<!-- id: N/compress.createArchiver -->

Returns a new [`Archiver`](#archiver-object) for building a ZIP or TAR-family archive. Takes no
arguments.

**Returns:** `compress.Archiver`.

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2020.2

## Objects

### Archiver (object)
<!-- id: N/compress.Archiver -->

Collects files with `add` and then writes them into one archive with `archive`. Created by
[`compress.createArchiver()`](#compresscreatearchiver). Server scripts; since 2020.2.

#### Archiver.add(options)
<!-- id: N/compress.Archiver#add -->

Queues a file for the archive. Inside the archive it is stored at `directory` plus the file's name,
or at the archive root plus the file's name when no directory is given.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.file` | file.File | Yes | |
| `options.directory` | string | No | |

> [!SOURCE-ERROR] The source's note on the target path is cut off (it ends mid-word at
> "options.file nam"). Read it as: the path is the directory (or root) followed by the file's name,
> and check the resulting entry names in a test archive.
<!-- source-error: N/compress.Archiver#add | - | path_note_says_options_file_nam -->

**Returns:** `void`

**Errors:**

- `COMPRESS_API_DUPLICATE_PATH`: a file with the same target path was already added.
- `COMPRESS_API_FILE_IS_TOO_LARGE`: the file is larger than 680 MB. A 2 GB limit for binary and
  ASCII files is noted next to this rule; the source does not state how it combines with the
  680 MB figure.

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2020.2

#### Archiver.archive(options)
<!-- id: N/compress.Archiver#archive -->

Builds the archive from everything added so far and returns it as a temporary file.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.type` | string | No | |
| `options.name` | string | Yes | |

- `options.type`: a value from [`compress.Type`](#compresstype). You can leave it out when
  `options.name` ends in an extension the module recognises: `.cpio`, `.tar`, `.tar.gz`,
  `.tar.bz2`, `.tgz`, `.tbz2` or `.zip`.
- `options.name`: the archive's file name.

**Returns:** `file.File`, a temporary archive file.

**Errors:**

- `COMPRESS_API_UNSUPPORTED_ARCHIVE_TYPE`: `options.type` is not a supported archive type.
- `COMPRESS_API_UNRECOGNIZED_ARCHIVE_FILE_EXTENSION`: `options.type` was left out and the name's
  extension does not identify a format.
- `COMPRESS_API_UNABLE_TO_RETRIEVE_FILE_CONTENTS`: the contents of an added file could not be read.

**Governance:** 25 units. Another source gives 0 units; the spec does not say which is right.

> [!SOURCE-ERROR] The vendor's sources disagree on this method's usage cost (25 units versus 0),
> and neither is confirmed. Budgeting for the higher figure avoids surprises.
<!-- source-error: N/compress.Archiver#archive | - | governance_sources_disagree -->

**Availability:** server scripts.

**Since:** 2020.2

## Enums

### compress.Type
<!-- id: N/compress.Type -->

Archive formats accepted by `options.type` of `Archiver.archive`. Since 2020.2. The source lists
the names without further description.

| Value |
|---|
| `CPIO` |
| `TAR` |
| `TBZ2` |
| `TGZ` |
| `ZIP` |

## Examples

A Map/Reduce summarize stage that bundles the day's generated packing slips into one ZIP and saves
it to an export folder. The file ids and the folder id come from script parameters.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType MapReduceScript
 */
define(['N/compress', 'N/file', 'N/runtime', 'N/log'], (compress, file, runtime, log) => {
  const getInputData = () => [];

  const summarize = () => {
    const script = runtime.getCurrentScript();
    const slipIds = String(script.getParameter({ name: 'custscript_slip_file_ids' })).split(',');
    const archiver = compress.createArchiver();
    slipIds.forEach((id) => {
      archiver.add({ file: file.load({ id: Number(id) }), directory: 'packing-slips/' });
    });
    const zip = archiver.archive({ name: `slips_${Date.now()}.zip` });
    zip.folder = Number(script.getParameter({ name: 'custscript_export_folder' }));
    const savedId = zip.save();
    log.audit({ title: 'Archive saved', details: `${slipIds.length} slips, file ${savedId}` });
  };

  return { getInputData, summarize };
});
```

Shrinking a CSV extract before transferring it, skipping files that exceed the 680 MB input limit
and choosing the compression level explicitly:

```js
/**
 * @NApiVersion 2.1
 */
define(['N/compress', 'N/file'], (compress, file) => {
  const MAX_INPUT_BYTES = 680 * 1024 * 1024;

  const gzipExtract = (fileId) => {
    const csv = file.load({ id: fileId });
    if (csv.size > MAX_INPUT_BYTES) {
      return null;
    }
    return compress.gzip({ file: csv, level: 6 });
  };

  return { gzipExtract };
});
```
