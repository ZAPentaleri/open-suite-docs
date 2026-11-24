# Suitelet
<!-- id: script:Suitelet -->

A Suitelet is server-side code that answers HTTP requests at a URL NetSuite generates for its
deployment. It follows a plain request/response model: a GET or POST arrives, your `onRequest`
function builds a response, and the browser (or calling code) receives it. Use it for custom pages
inside NetSuite and for back-end logic that other scripts call.

## At a glance

| Item | Value |
|---|---|
| `@NScriptType` | `Suitelet` |
| Usage-unit limit per execution | 1,000 |
| Time limit | 300 seconds |
| Runs on | Server |
| SuiteScript 1.0 counterpart | Suitelets ([1.0 concepts](../topics/ss1_api.suitelets.md)) |
| Default response encoding | UTF-8 |
| Entry points | [`onRequest`](#onrequestcontext) |

Related pages: [deployment](../topics/suitelet.deployment.md), [sample patterns](../topics/suitelet.samples.md),
[SDF object](../topics/sdf.suitelet.md), [script deployment](../topics/script_deployment.md),
[N/http](../modules/N_http.md), [N/ui/serverWidget](../modules/N_ui_serverWidget.md),
[N/https](../modules/N_https.md), [N/file](../modules/N_file.md),
[Restlet](Restlet.md), [ClientScript](ClientScript.md).

## How it runs

**Execution flow.** A client sends a GET or POST to the Suitelet's system-generated URL. NetSuite
invokes the script with a request object ([`http.ServerRequest`](../modules/N_http.md)) and a
response object ([`http.ServerResponse`](../modules/N_http.md)). Whatever the script puts on the
response is returned to the client, and the browser displays it.

**What it can use.** The whole server-side SuiteScript API is available.

**What it can send back.** Free-form text, HTML, RSS, XML, or a redirect, either to an external URL
or to another NetSuite page. Redirecting to an external URL requires the Suitelet to be available
without login.

**Two kinds.**

- *UI Suitelets* build pages with the NetSuite look out of portlets, forms, fields, sublists, tabs,
  lists and columns (see [N/ui/serverWidget](../modules/N_ui_serverWidget.md)).
- *Back-end Suitelets* return data instead of a page. They act as a service for other scripts, and let
  you move logic out of a client script so that its source is not exposed in the browser. For
  back-end work, a [RESTlet](Restlet.md) is the alternative to consider.

**What it is not for.** Suitelets are not intended for systems integration or for web stores. To put a
form on a web store, use online forms instead.

**Embedding HTML.** HTML is the way to show components that serverWidget does not offer. There are
two routes:

- *Inline*: add a field of type `INLINEHTML` with [`Form.addField`](../modules/N_ui_serverWidget.md)
  and put the markup in its [`defaultValue`](../modules/N_ui_serverWidget.md). A company logo shown as
  an inline image is a typical case.
- *Linked*: fetch an HTML file with [`https.get`](../modules/N_https.md), for instance a page that
  links to a form. Because the HTML and its data are then handled as strings, this route gets
  complicated quickly.

**Reserved URL parameters.** These names are taken by NetSuite and must not be used for your own
parameters: `e`, `id`, `cp`, `l`, `popup`, `s`, `d`, `_nodrop`, `sc`, `sticky`, `print`, `email`,
`q`, `si`, `st`, `r`, `displayonly`, `nodisplay`, `deploy`, `script`, `ns-at`. Using one produces an
error saying there are no records of this type; the source does not state an error code for it.
Prefix your own parameters with `custom`, for example `custom_id` rather than `id`.

**Headers.** The `Authorization` header is reserved for OAuth requests. If you need to pass your own
credentials header, use a custom name such as `X-Authorization`.

**Concurrency.** A Suitelet that is available without login is subject to the same concurrency limit
as RESTlets. A Suitelet that requires login has no concurrency limit.

**Response encoding.** Responses are UTF-8 by default, and there is no 2.x counterpart of the 1.0
`nlobjResponse.setEncoding`. When a different encoding is needed:

1. create a file in that encoding with [`file.create`](../modules/N_file.md);
2. set a `Content-Type` header with a matching charset using
   [`ServerResponse.setHeader`](../modules/N_http.md);
3. send the file with [`ServerResponse.writeFile`](../modules/N_http.md).

The encoding survives only if the charset in the header matches the file's encoding.

**Managing scripts.** Suitelets can be managed with SDF, copied between accounts with Copy to Account
(the control is at the top right of the Suitelet page), and reviewed with SuiteScript Analysis.

## Entry points

### onRequest(context)
<!-- id: script:Suitelet.onRequest -->

Runs whenever the Suitelet is triggered, that is, when an HTTP GET or POST reaches its URL.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `context` | Object | Yes | |
| `context.request` | http.ServerRequest | - | |
| `context.response` | http.ServerResponse | - | |

Both context properties exist since 2015.2.

- `context.request` is the incoming request, an [`http.ServerRequest`](../modules/N_http.md).
- `context.response` is the response the Suitelet sends back, an
  [`http.ServerResponse`](../modules/N_http.md).

**Returns:** `void`

**Errors:** using a reserved URL parameter name gives a "no records of this type" error. The source
does not state an error code for it.

## Examples

A department phone list downloaded as CSV for an older desk-phone system that only reads Western
(Windows-1252) text. Because Suitelet responses are UTF-8 by default, the script builds a file in the
other encoding, sets a matching charset, and writes the file. The department comes from the
`custom_dept` URL parameter (not `d` or `id`, which are reserved), with a script parameter as the
fallback.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType Suitelet
 */
define(['N/search', 'N/file', 'N/runtime'], (search, file, runtime) => {
  const cell = (value) => `"${String(value || '').replace(/"/g, '""')}"`;

  const onRequest = (context) => {
    const { request, response } = context;
    const department = request.parameters.custom_dept
      || runtime.getCurrentScript().getParameter({ name: 'custscript_phone_list_dept' });

    if (!department) {
      response.write({ output: 'No department given. Add custom_dept to the URL.' });
      return;
    }

    const rows = ['Name,Job Title,Phone,Email'];
    search.create({
      type: search.Type.EMPLOYEE,
      filters: [['department', 'anyof', department]],
      columns: ['entityid', 'title', 'phone', 'email']
    }).run().each((result) => {
      const values = ['entityid', 'title', 'phone', 'email'].map((name) => result.getValue({ name }));
      rows.push(values.map(cell).join(','));
      return true;
    });

    const csv = file.create({
      name: `phones_dept_${department}.csv`,
      fileType: file.Type.CSV,
      contents: rows.join('\r\n'),
      encoding: file.Encoding.WINDOWS_1252
    });
    response.setHeader({ name: 'Content-Type', value: 'text/csv; charset=windows-1252' });
    response.writeFile({ file: csv });
  };

  return { onRequest };
});
```

A back-end Suitelet for the client-script permission workaround (see the [ClientScript](ClientScript.md)
page). Deployed to run as administrator, it answers a GET with the supervisor of an employee whose
record the calling user's role may not be able to open, and returns JSON. The select field comes back
from `search.lookupFields` as a value/text pair; the code accepts either an array or a single object,
because the source describes that shape both ways.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType Suitelet
 */
define(['N/search', 'N/http'], (search, http) => {
  const reply = (response, body) => {
    response.setHeader({ name: 'Content-Type', value: 'application/json' });
    response.write({ output: JSON.stringify(body) });
  };

  const onRequest = (context) => {
    const { request, response } = context;
    if (request.method !== http.Method.GET) {
      reply(response, { ok: false, reason: 'Use GET' });
      return;
    }

    const employeeId = request.parameters.custom_employee || '';
    if (!/^\d+$/.test(employeeId)) {
      reply(response, { ok: false, reason: 'custom_employee must be an internal ID' });
      return;
    }

    const found = search.lookupFields({
      type: search.Type.EMPLOYEE,
      id: employeeId,
      columns: ['supervisor', 'issalesrep']
    });
    const raw = found.supervisor;
    const supervisor = Array.isArray(raw) ? raw[0] : raw;

    reply(response, {
      ok: true,
      supervisorId: supervisor ? supervisor.value : null,
      supervisorName: supervisor ? supervisor.text : null,
      isSalesRep: found.issalesrep === true
    });
  };

  return { onRequest };
});
```
