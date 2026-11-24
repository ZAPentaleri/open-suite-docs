# N/log
<!-- id: N/log -->

`N/log` writes entries to a script's execution log. There is one function per severity: `debug`,
`audit`, `error` and `emergency`, each taking a title and optional details. Which calls actually leave
an entry depends on the log level set on the script deployment. The same functions are also reachable
without an import, through the global `log` object (see [Globals](../globals.md)).

- **Availability:** client and server scripts (see the client script caveats below).
- **Load it as:** `define(['N/log'], (log) => { ... })`
- Related: [N/runtime](N_runtime.md) (`Script.logLevel` reports the deployment's level),
  [Governance](../topics/governance.md).

## How log levels filter entries

The log level on the deployment decides which severities get recorded:

| Deployment log level | Entries recorded |
|---|---|
| Debug | all of them: debug, audit, error, emergency |
| Audit | audit, error, emergency |
| Error | error, emergency |
| Emergency | emergency only |

Production scripts should not be left at the Debug level. An audit entry suits events worth keeping
on record, such as a request sent to an external site.

## Where entries go and how long they last

- Entries show on the Execution Log subtab of the script deployment, in the execution log of the
  script debugger, and on the Script Execution Logs page (Customization > Scripting > Script Execution
  Logs). That page lets you filter entries, download them as CSV or Excel, and print them. Errors that
  NetSuite records on its own also appear on the execution log subtab.
- Calls made while running in the debugger are not written to the deployment's logs.
- Logs older than 30 days are purged. Deployment logs and the log search together hold at most
  5,000,000 entries per database. The Script Execution Logs page is not subject to that database
  ceiling and keeps 30 days of entries.
- Deployment logs are not guaranteed to persist. For a lasting audit trail, write to custom records
  instead.
- A company may make at most 100,000 log calls per 60 minutes. A script that logs excessively has its
  log level lowered automatically, and its owner is notified.
- When `details` is an object, it is converted to text with `JSON.stringify`.

## Client scripts

- In a client script attached to a form at the form level, log calls are ignored. Use `console.log`
  (plain browser JavaScript) there instead.
- When a client script is attached to a form through the form's client script properties, its entries
  are written to the parent deployment.

## Members

| Member | Kind | Summary |
|---|---|---|
| [`log.debug(options)`](#logdebugoptions) | function | Development-time detail |
| [`log.audit(options)`](#logauditoptions) | function | Notable events in production |
| [`log.error(options)`](#logerroroptions) | function | Errors in production |
| [`log.emergency(options)`](#logemergencyoptions) | function | Critical problems; always recorded |

## Functions

### log.debug(options)
<!-- id: N/log.debug -->

Meant for development: detail you want while building and testing. Entries are recorded only when the
deployment's log level is Debug.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.title` | string | Yes | |
| `options.details` | any | No | |

- `options.title`: up to 99 characters. When it is null, empty or left out, the entry is titled
  `Untitled`. Since 2016.1.

> [!SOURCE-ERROR] The source marks the title as required and also describes what happens when it is
> omitted (the entry becomes `Untitled`). It does not settle which holds; always pass a title.
<!-- source-error: N/log.debug | options.title | title_required_but_omitted_shows_untitled -->

- `options.details`: the spec has no usable type for it (`any`). Objects are serialized with
  `JSON.stringify`; anything after 3999 characters is cut off. Since 2016.1.

**Returns:** `void`

**Governance:** 0 units. This figure is recorded as stated by the project owner rather than taken
from a vendor page. The practical limit is the logging volume per 60 minutes described above.

**Availability:** client and server scripts. In form-level client scripts, use `console.log` instead.

**Since:** 2016.1

### log.audit(options)
<!-- id: N/log.audit -->

Meant for production: records events worth keeping. Entries are recorded when the deployment's log
level is Audit or Debug.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.title` | string | Yes | |
| `options.details` | any | No | |

- `options.title`: up to 99 characters; a null, empty or missing title becomes `Untitled`. Since
  2016.1.

> [!SOURCE-ERROR] The title is listed as required, yet the source also gives a fallback for an omitted
> title. Which one applies is not settled; pass a title.
<!-- source-error: N/log.audit | options.title | title_required_but_omitted_shows_untitled -->

- `options.details`: no usable type in the spec (`any`); objects go through `JSON.stringify`, and the
  text is cut off after 3999 characters. Since 2016.1.

**Returns:** `void`

**Governance:** 0 units (recorded as stated by the project owner, not from a vendor page). Subject to
the logging volume per 60 minutes.

**Availability:** client and server scripts. Not supported in client scripts attached to forms; use
`console.log` there.

**Since:** 2016.1

### log.error(options)
<!-- id: N/log.error -->

Meant for production: records errors. Entries are recorded when the deployment's log level is Error,
Audit or Debug.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.title` | string | Yes | |
| `options.details` | any | No | |

- `options.title`: up to 99 characters; a null, empty or missing title becomes `Untitled`. Since
  2016.1.

> [!SOURCE-ERROR] The source both requires the title and describes the `Untitled` fallback for a
> missing one. Do not rely on the fallback; pass a title.
<!-- source-error: N/log.error | options.title | title_required_but_omitted_shows_untitled -->

- `options.details`: no usable type in the spec (`any`); objects go through `JSON.stringify`, and the
  text is cut off after 3999 characters. Since 2016.1.

**Returns:** `void`

**Governance:** 0 units (recorded as stated by the project owner, not from a vendor page). Subject to
the logging volume per 60 minutes.

**Availability:** client and server scripts. In form-level client scripts, use `console.log` instead.

**Since:** 2016.1

### log.emergency(options)
<!-- id: N/log.emergency -->

Meant for production: the most severe level. Entries are recorded whatever the deployment's log level.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.title` | string | Yes | |
| `options.details` | any | No | |

- `options.title`: up to 99 characters; a null, empty or missing title becomes `Untitled`. Since
  2016.1.

> [!SOURCE-ERROR] As with the other log functions, the title is listed as required while an omitted
> title is said to produce `Untitled`. Treat the title as required.
<!-- source-error: N/log.emergency | options.title | title_required_but_omitted_shows_untitled -->

- `options.details`: no usable type in the spec (`any`); objects go through `JSON.stringify`, and the
  text is cut off after 3999 characters. Since 2016.1.

**Returns:** `void`

**Governance:** 0 units (recorded as stated by the project owner, not from a vendor page). Subject to
the logging volume per 60 minutes.

**Availability:** client and server scripts. In form-level client scripts, use `console.log` instead.

**Since:** 2016.1

## Examples

**Picking severities in a RESTlet.** The inbound payload is logged only at debug level, the outcome
at audit level, and rejections at error level with an object as details (serialized for you).

```js
/**
 * @NApiVersion 2.1
 * @NScriptType Restlet
 */
define(['N/log'], (log) => {
  const post = (body) => {
    log.debug({ title: 'Inbound shipment notice', details: body });
    try {
      const lines = Array.isArray(body.lines) ? body.lines : [];
      if (!lines.length) {
        throw new Error('Notice has no lines');
      }
      log.audit({ title: 'Shipment notice accepted', details: `${lines.length} lines from carrier ${body.carrier}` });
      return { accepted: true, lines: lines.length };
    } catch (e) {
      log.error({ title: 'Shipment notice rejected', details: { reason: e.message, carrier: body.carrier } });
      return { accepted: false, reason: e.message };
    }
  };

  return { post };
});
```

**Keeping titles within bounds.** Titles stop at 99 characters and details at 3999, so a helper trims
long titles and the summary stage logs a short line rather than a whole result set.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType MapReduceScript
 */
define(['N/log'], (log) => {
  const shortTitle = (text) => (text.length > 99 ? `${text.slice(0, 96)}...` : text);

  const getInputData = () => [{ sku: 'BRK-220', qty: 4 }, { sku: 'BRK-310', qty: 0 }];

  const map = (context) => {
    const line = JSON.parse(context.value);
    if (line.qty === 0) {
      log.error({ title: shortTitle(`Zero quantity on replenishment line for ${line.sku}`), details: line });
    }
  };

  const summarize = (summary) => {
    log.audit({ title: 'Replenishment run finished', details: `run took ${summary.seconds}s` });
  };

  return { getInputData, map, summarize };
});
```
