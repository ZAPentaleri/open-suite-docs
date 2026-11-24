# N/machineTranslation
<!-- id: N/machineTranslation -->

`N/machineTranslation` translates text through a dedicated machine translation service (the Oracle
Cloud Infrastructure language service) rather than a general-purpose large language model. You
wrap each piece of text in a document, send one or more documents together with a target
language, and receive the translations plus a list of any documents that could not be translated.

Reasons to prefer it over [N/llm](N_llm.md):

- Requests are unlimited and do not draw on the account's AI units.
- The source language can be detected automatically.
- It covers more languages than the language models, and its output is more predictable than a
  generative translation.

Use `N/llm` instead when you need to steer the translation with a prompt.

Size limits apply to every request: a single document may hold at most 5,000 characters, and all
documents of one request together at most 20,000 characters.

- **Availability:** server scripts only, SuiteScript 2.1 only. The Server SuiteScript feature must
  be enabled.
- **Load it as:** `define(['N/machineTranslation'], (machineTranslation) => { ... })`
- **Since:** 2025.1

## Members

| Member | Kind | Summary |
|---|---|---|
| [`machineTranslation.createDocument(options)`](#machinetranslationcreatedocumentoptions) | function | Wrap text as a translatable document |
| [`machineTranslation.translate(options)`](#machinetranslationtranslateoptions) | function | Translate a batch of documents |
| [`machineTranslation.translate.promise(options)`](#machinetranslationtranslatepromiseoptions) | function | Promise form of `translate` |
| [`Document`](#document-object) | object | One piece of text with an ID and a language |
| [`Response`](#response-object) | object | Translated documents plus errors |
| [`Error`](#error-object) | object | Why one document was not translated |
| [`machineTranslation.Language`](#machinetranslationlanguage) | enum | Supported languages |

## Functions

### machineTranslation.createDocument(options)
<!-- id: N/machineTranslation.createDocument -->

Builds a [`Document`](#document-object) to hand to `translate`. Every document in a request needs
its own ID, and an empty document is not allowed. Keep each document to one language for the
best results. A document may hold at most 5,000 characters, and the documents of one request
together at most 20,000.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.language` | string | No | |
| `options.id` | string | Yes | |
| `options.text` | string | Yes | |

- `options.language`: the language the text is written in, as a
  [`machineTranslation.Language`](#machinetranslationlanguage) value. Omit it and the service
  detects the language. A value outside the enum raises `INVALID_LANGUAGE`. Since 2025.1.
- `options.id`: your identifier for the document, unique within the request. Since 2025.1.
- `options.text`: the text to translate. Since 2025.1.

**Returns:** `machineTranslation.Document`.

**Errors:**

- `INVALID_LANGUAGE`: `options.language` is not one of the enum's values.

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2025.1

### machineTranslation.translate(options)
<!-- id: N/machineTranslation.translate -->

Sends a batch of documents to the translation service and returns a
[`Response`](#response-object). Building the documents with `createDocument` is the recommended
route, although plain objects carrying an ID and text are accepted too. Document IDs must be
unique, no document may be empty, each document is limited to 5,000 characters and the whole batch
to 20,000. The call does not consume AI units and has no request cap.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.targetLanguage` | string | Yes | |
| `options.timeout` | number | No | `30000` |
| `options.documents` | machineTranslation.Document[] | Yes | |

- `options.targetLanguage`: the language to translate into, a
  [`machineTranslation.Language`](#machinetranslationlanguage) value. Since 2025.1.
- `options.timeout`: how long to wait, in milliseconds. Since 2025.1.
- `options.documents`: either `Document` objects from `createDocument` or ad hoc objects with an
  ID and text. Since 2025.1.

**Returns:** `machineTranslation.Response`, holding the translated documents and any per-document
errors.

**Errors:**

- `DOCUMENT_CANNOT_BE_EMPTY`: a document in `options.documents` is empty (or no documents were
  given).
- `DOCUMENT_IDS_MUST_BE_UNIQUE`: two documents in `options.documents` share an ID.
- `DOCUMENT_TOO_LARGE`: a single document is longer than 5,000 characters.
- `INPUT_TOO_LARGE`: the documents together exceed 20,000 characters.
- `INVALID_LANGUAGE`: a document's language is not in the `Language` enum.

**Governance:** 100 units.

**Availability:** server scripts.

**Since:** 2025.1

### machineTranslation.translate.promise(options)
<!-- id: N/machineTranslation.translate.promise -->

The asynchronous form of [`translate`](#machinetranslationtranslateoptions). It accepts exactly
the same parameters and raises exactly the same errors as `translate`, including the per-document
and per-request size limits; the only difference is that it returns a Promise instead of the
response.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.targetLanguage` | string | Yes | |
| `options.timeout` | number | No | `30000` |
| `options.documents` | machineTranslation.Document[] | Yes | |

- `options.targetLanguage`: a [`machineTranslation.Language`](#machinetranslationlanguage) value.
- `options.timeout`: in milliseconds.
- `options.documents`: `Document` objects or ad hoc objects with an ID and text.

**Returns:** `Promise`. The source does not state what it settles with (the synchronous form
returns a `machineTranslation.Response`).

**Errors:** the same as `translate`: `DOCUMENT_CANNOT_BE_EMPTY`, `DOCUMENT_IDS_MUST_BE_UNIQUE`,
`DOCUMENT_TOO_LARGE`, `INPUT_TOO_LARGE` and `INVALID_LANGUAGE`, under the same conditions.

**Governance:** 100 units.

**Availability:** server scripts.

**Since:** 2025.1

## Objects

### Document (object)
<!-- id: N/machineTranslation.Document -->

A unit of text to translate, or a translated result. It has three read-only properties: `id`,
`language` and `text`. `createDocument` returns one, and the `results` of a `translate` response
are `Document` objects too. Server scripts, since 2025.1.

#### Document.id
<!-- id: N/machineTranslation.Document#id -->

`string`, read-only. The document's identifier, which must be unique among the documents of one
`translate` call. Assigning it raises `READ_ONLY`. Server scripts, since 2025.1.

#### Document.language
<!-- id: N/machineTranslation.Document#language -->

`string`, read-only. Its meaning depends on direction: on a document you send, it is the source
language (and when it is null or undefined the service detects the language); on a document in
the response, it is the language the text was translated into. Assigning it raises `READ_ONLY`.
Server scripts, since 2025.1.

#### Document.text
<!-- id: N/machineTranslation.Document#text -->

`string`, read-only. The text, at most 5,000 characters. One language per document is
recommended. Assigning it raises `READ_ONLY`. Server scripts, since 2025.1.

### Response (object)
<!-- id: N/machineTranslation.Response -->

What `translate` returns: translated documents in `results` and problems in `errors`. When every
document translates, `errors` is empty. Server scripts, since 2025.1.

#### Response.errors
<!-- id: N/machineTranslation.Response#errors -->

`machineTranslation.Error[]`, read-only. One [`Error`](#error-object) per document that failed; an
empty array when nothing failed. Assigning it raises `READ_ONLY`. Server scripts, since 2025.1.

#### Response.results
<!-- id: N/machineTranslation.Response#results -->

`machineTranslation.Document[]`, read-only. The translated documents. Assigning it raises
`READ_ONLY`. Server scripts, since 2025.1.

### Error (object)
<!-- id: N/machineTranslation.Error -->

Describes a document that could not be translated, for example because it contains characters the
service does not recognise. Found in `Response.errors`; carries `documentId` and `message`. Server
scripts, since 2025.1.

#### Error.documentId
<!-- id: N/machineTranslation.Error#documentId -->

`string`, read-only. The ID of the document that failed. Assigning it raises `READ_ONLY`. Server
scripts, since 2025.1.

#### Error.message
<!-- id: N/machineTranslation.Error#message -->

`string`, read-only. Text explaining the failure. Assigning it raises `READ_ONLY`. Server scripts,
since 2025.1.

## Enums

### machineTranslation.Language
<!-- id: N/machineTranslation.Language -->

Languages accepted by `createDocument` (`options.language`) and `translate`
(`options.targetLanguage`). The source lists only the value names; it does not state the
underlying string each name stands for, so always refer to a language through the enum (for
example `machineTranslation.Language.FRENCH`). Since 2025.1.

| Value | Value | Value | Value |
|---|---|---|---|
| `ARABIC` | `BRAZILIAN_PORTUGUESE` | `CANADIAN_FRENCH` | `CROATIAN` |
| `CZECH` | `DANISH` | `DUTCH` | `ENGLISH` |
| `FINNISH` | `FRENCH` | `GERMAN` | `GREEK` |
| `HEBREW` | `HUNGARIAN` | `ITALIAN` | `JAPANESE` |
| `KOREAN` | `NORWEGIAN` | `POLISH` | `PORTUGUESE` |
| `ROMANIAN` | `RUSSIAN` | `SIMPLIFIED_CHINESE` | `SLOVAK` |
| `SLOVENIAN` | `SPANISH` | `SWEDISH` | `THAI` |
| `TRADITIONAL_CHINESE` | `TURKISH` | `VIETNAMESE` | |

## Examples

A Map/Reduce `map` stage that produces German web-store text for each inventory item it receives,
skipping empty fields and logging per-document failures.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType MapReduceScript
 */
define(['N/machineTranslation', 'N/record', 'N/log'], (machineTranslation, record, log) => {
  const getInputData = () => [3101, 3102, 3107];

  const map = (context) => {
    const itemId = Number(context.value);
    const item = record.load({ type: record.Type.INVENTORY_ITEM, id: itemId });
    const pieces = ['displayname', 'storedescription']
      .map((fieldId) => ({ fieldId, text: String(item.getValue({ fieldId }) || '') }))
      .filter((p) => p.text.length > 0 && p.text.length <= 5000);

    if (!pieces.length) {
      return;
    }

    const docs = pieces.map((p) => machineTranslation.createDocument({
      id: `${itemId}-${p.fieldId}`,
      text: p.text,
      language: machineTranslation.Language.ENGLISH
    }));

    const response = machineTranslation.translate({
      documents: docs,
      targetLanguage: machineTranslation.Language.GERMAN
    });

    response.errors.forEach((err) => {
      log.error({ title: `Not translated: ${err.documentId}`, details: err.message });
    });
    response.results.forEach((doc) => {
      context.write({ key: doc.id, value: doc.text });
    });
  };

  return { getInputData, map };
});
```

A small custom module that uses the promise form with ad hoc document objects and a shorter
timeout, translating a batch of delivery instructions into Spanish for a warehouse crew. Because
the source does not state what the Promise settles with, the handler checks for `results` before
using it.

```js
/**
 * @NApiVersion 2.1
 */
define(['N/machineTranslation', 'N/log'], (machineTranslation, log) => {
  const translateInstructions = (instructions) => {
    const documents = instructions
      .filter((line) => line && line.trim().length > 0)
      .map((line, i) => ({ id: `step-${i + 1}`, text: line }));

    return machineTranslation.translate.promise({
      documents,
      targetLanguage: machineTranslation.Language.SPANISH,
      timeout: 10000
    }).then((response) => {
      const results = (response && response.results) || [];
      return results.map((doc) => `${doc.id}: ${doc.text}`);
    }).catch((e) => {
      log.error({ title: 'Instruction translation failed', details: `${e.name}: ${e.message}` });
      return [];
    });
  };

  return { translateInstructions };
});
```
