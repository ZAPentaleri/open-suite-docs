# Restlet
<!-- id: script:Restlet -->

A RESTlet is a server-side script that you publish as your own HTTP endpoint. External applications
and other NetSuite scripts call it with GET, POST, PUT or DELETE, and whatever the matching entry
point returns becomes the body of the HTTP response. It is meant for custom RESTful integrations:
pulling data into NetSuite, sending data out, or giving other scripts (including scripts that
customize NetSuite pages) a back end to talk to. It is an alternative to SOAP web services and to
Suitelets.

## At a glance

| Item | Value |
|---|---|
| `@NScriptType` | `Restlet` (the annotation value is case-insensitive, so `RESTlet` is accepted as well) |
| Usage-unit limit per execution | 5,000 (five times the Suitelet limit) |
| Time limit | 300 seconds |
| Runs on | Server |
| Largest guaranteed string in or out | 10 MB |
| Record-bound | No |
| SuiteScript 1.0 counterpart | RESTlets ([1.0 concepts](../topics/ss1_api.restlets.md)) |
| Entry points | [`get`](#getrequestparams), [`post`](#postrequestbody), [`put`](#putrequestbody), [`delete`](#deleterequestparams) |

Related pages: [SDF object](../topics/sdf.restlet.md),
[SDF object elements](../gen/catalogs/sdf_restlet_object.md),
[SDF script fields](../gen/catalogs/sdf_restlet_script_fields.md),
[SDF deployments](../gen/catalogs/sdf_restlet_deployments.md),
[script deployment](../topics/script_deployment.md), [N/scriptTypes/restlet](../modules/N_scriptTypes_restlet.md),
[N/https](../modules/N_https.md), [N/url](../modules/N_url.md), [N/task](../modules/N_task.md),
[HTTP status codes](../gen/catalogs/restlet_http_status_codes.md),
[SuiteScript errors returned by RESTlets](../gen/catalogs/restlet_suitescript_errors.md).

Authentication pages: [RESTlet authentication overview](../topics/restlet_auth.md),
[token-based authentication: signing](../topics/restlet_auth.tba_signing.md),
[TBA flow](../topics/restlet_auth.tba_flow.md),
[issuing a TBA token](../topics/restlet_auth.tba_issuetoken.md),
[TBA token REST endpoints](../topics/restlet_auth.token_endpoints.md),
[TBA errors](../topics/restlet_auth.tba_errors.md),
[OAuth 2.0 authorization code grant](../topics/restlet_auth.oauth2_code_grant.md),
[OAuth 2.0 client credentials (machine to machine)](../topics/restlet_auth.oauth2_m2m.md),
[OAuth 2.0 errors](../topics/restlet_auth.oauth2_errors.md),
[user-credential (NLAuth) authentication](../topics/restlet_auth.nlauth.md).

## How it runs

**Only when called.** A RESTlet is not attached to any record, and nothing triggers it except a
request. It runs when a call arrives and at no other time.

**The annotation.** The script needs the `@NScriptType` annotation and at least one RESTlet entry
point. The annotation value is not case-sensitive: the spec records the script type as `Restlet`
and also writes the requirement as `@NScriptType RESTlet`, and both spellings are accepted. This
page uses `Restlet` throughout.

**Entry points decide how it can be called.** The entry points you export determine which HTTP
methods the endpoint answers. The method of an incoming call must match an entry point that the
script implements.

**Before anyone can call it.** The script file must be in the File Cabinet, and it needs a script
record and a deployment record. When you fill in the ID fields on those records, clear the default
text and type your own. A deployment's status is either `Testing` or `Released`; calls from outside
NetSuite only work when the status is `Released`.

**Its URLs.** Saving the deployment record generates two URLs: a partial URL (which leaves out the
RESTlet domain) and a full external URL. They appear on the deployment record in the URL and External
URL fields; the external URL shows up once the deployment has been saved. Since 2017.2 the full URL
uses an account-specific RESTlet domain, and the older data-centre domains keep working.

- The External URL field is for testing. Try the RESTlet with it, but never hard-code that URL in an
  integration.
- For integrations, work out the RESTlet domain at run time. An external client should generate the
  full URL dynamically. A server script calling a RESTlet in another account should discover it
  through the REST roles service.
- A client script in the same account, running in an active session, can use the partial URL from
  the deployment, or build the URL with [`url.resolveScript`](../modules/N_url.md) from the script ID
  and deployment ID. The values you may pass are the IDs shown on the script record and the
  deployment record.
- A Suitelet (and any other server script in the same account) can build the URL the same way with
  `url.resolveScript`, or use the REST roles service; either way the full URL is produced dynamically.
- A POST URL carries no extra parameters; the data goes in the body.

**Where the input travels.** GET and DELETE carry their input in URL parameters. POST and PUT carry
it in the request body, as JSON or plain text.

**Content types.** Send a `Content-Type` header. It is required whenever the request has a body, and
leaving it out then produces HTTP 415 (Unsupported Media Type). The header value is case-sensitive.
Use `application/json` when the values map onto record fields, `application/xml` for GET calls, and
`text/plain` for simple input. When the request has a body, the response comes back in the same
format as that body; with no body, the response defaults to plain text. To answer in another format,
return a custom response built with [`restlet.createResponse`](../modules/N_scriptTypes_restlet.md).

**HTTP method semantics are not enforced.** NetSuite does not check that a GET only reads or that a
POST creates something. Follow ordinary HTTP method semantics (RFC 2616) anyway. In particular, do
not write a `get` that creates data, and do not use `post` for calls that send no input.

**Authentication.** Every call must be authenticated; there is no anonymous RESTlet. Callers from
outside NetSuite must send an authorization header, and the connection must use TLS. The
authentication pages listed above describe the schemes; this page deliberately shows no credentials.

- RESTlets share session authentication with other SuiteScript scripts and with the SuiteTalk REST
  endpoints.
- From inside NetSuite, [`https.requestRestlet`](../modules/N_https.md) and
  [`https.requestSuiteTalkRest`](../modules/N_https.md) call a RESTlet without authenticating again.
  Those two calls exist only for calls made within NetSuite. With `https.requestRestlet`, the RESTlet
  runs with the privileges of the calling script. That removes the need for a Suitelet marked as
  available without login, or for running code as administrator, just to reach shared logic.
- The spec's shell-script caller authenticates with NLAuth and sends `application/json`. NLAuth is
  NetSuite's user-credential scheme for RESTlets; according to
  [its topic](../topics/restlet_auth.nlauth.md), new RESTlets have not accepted it since 2021.1,
  support ends with 2027.1, and new integrations should use OAuth 2.0 (external callers use OAuth 2.0
  or token-based authentication).
- Users can issue, revoke and inspect their own token-based authentication tokens through the
  `/rest/issuetoken`, `/rest/revoketoken` and `/rest/tokeninfo` endpoints, which can be called on the
  RESTlet domain; see [TBA token REST endpoints](../topics/restlet_auth.token_endpoints.md).
- There is no logout operation.

**A correct call needs** the right RESTlet domain, an HTTP method that matches an entry point, a
`Content-Type` header and an authorization header. The spec's sample GET call fetches a phone call
record (ID 9363) by passing the record type and the ID as URL parameters.

**Errors and status codes.** Responses fall into HTTP success codes, HTTP error codes and SuiteScript
errors; the full lists are in the [HTTP status codes](../gen/catalogs/restlet_http_status_codes.md)
and [SuiteScript errors](../gen/catalogs/restlet_suitescript_errors.md) catalogs.

- An error your script does not catch produces HTTP 400.
- An error your script catches and handles produces HTTP 200. A 200 therefore does not prove the logic
  succeeded: report failures in the response body and have callers check it.
- The shape of the error output depends on the content type.
- HTTP 206 (Partial Content) is supported only in some cases.

**Concurrency.** Since 2017.2, RESTlet concurrency is governed per account, together with web
services.

**Handing off long work.** A RESTlet can pass values to a scheduled script through
[`task.ScheduledScriptTask`](../modules/N_task.md).

**Debugging and management.** Debug RESTlets with the NetSuite Debugger or the SuiteCloud IDE debug
client. RESTlets can be managed with the SuiteCloud Development Framework, copied between accounts
with Copy to Account (the control sits at the upper right of the script page), and reviewed with
SuiteScript Analysis.

**Compared with a Suitelet.** A RESTlet can hand plain text or JSON straight back to its caller.

**Scenarios the source covers.** A minimal RESTlet, record create/read/update/delete and upsert,
handling several records in one call, a RESTlet that hands work to a scheduled script, and callers
written as a client script, a Suitelet and a shell script.

## Entry points

Each entry point receives one argument (the URL parameters or the request body, not a context
object) and returns the response body: a string goes back as `text/plain`, an object as JSON or XML.

### get(requestParams)
<!-- id: script:Restlet.get -->

Runs when the RESTlet receives a GET request. Use it to retrieve data.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `requestParams` | Object | Yes | |

- `requestParams`: the URL parameters of the call (written as `&name=value` pairs in the URL),
  delivered as an object whatever the content type. NetSuite passes it in automatically. Since 2015.2.

**Returns:** `string | Object`. A string is sent as `text/plain`; an object as `application/json` or
XML.

### delete(requestParams)
<!-- id: script:Restlet.delete -->

Runs when the RESTlet receives a DELETE request. Use it to delete data.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `requestParams` | Object | Yes | |

- `requestParams`: the URL parameters as an object, regardless of content type; NetSuite passes it in
  automatically. Since 2015.2.

**Returns:** `string | Object`. A string is sent as `text/plain`; an object as JSON or XML.

### put(requestBody)
<!-- id: script:Restlet.put -->

Runs when the RESTlet receives a PUT request. Use it to insert or upsert data.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `requestBody` | string \| Object | Yes | |

- `requestBody`: the request body, passed in automatically. With `text/plain` it arrives as a string;
  with JSON or XML it arrives as an object. Since 2015.2.

> [!SOURCE-ERROR] The source's note on this argument refers to `requestParams`, the argument of the
> URL-based entry points, although `put` receives the request body. Rely on `requestBody` and treat the
> note's wording as a copy error.
<!-- source-error: script:Restlet.put | requestBody | note_names_requestparams_for_body_entry_point -->

**Returns:** `string | Object`. A string is sent as `text/plain`; an object as JSON or XML.

### post(requestBody)
<!-- id: script:Restlet.post -->

Runs when the RESTlet receives a POST request. Use it to create data; one call can create several
records. A typical reply is the internal ID of the new record.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `requestBody` | string \| Object | Yes | |

- `requestBody`: the request body, passed in automatically: a string for `text/plain`, an object for
  JSON or XML. When creating records, send JSON rather than plain text. Since 2015.2.

> [!SOURCE-ERROR] Here too the source's note on the argument names `requestParams`, which belongs to the
> URL-based entry points; `post` receives the request body. Rely on `requestBody`.
<!-- source-error: script:Restlet.post | requestBody | note_names_requestparams_for_body_entry_point -->

**Returns:** `string | Object`. A string is sent as `text/plain`; an object as JSON or XML.

## Examples

A RESTlet for a field-service app. `get` lists the tasks assigned to one employee (input in the URL),
and `post` creates a batch of tasks from a JSON body in one call. Failures are caught and reported in
the body, because a caught error still returns HTTP 200:

```js
/**
 * @NApiVersion 2.1
 * @NScriptType Restlet
 */
define(['N/search', 'N/record', 'N/log'], (search, record, log) => {
  const get = (requestParams) => {
    const employeeId = requestParams && requestParams.employee;
    if (!employeeId) {
      return { ok: false, error: 'Pass ?employee=<internal id> in the URL.' };
    }
    const tasks = [];
    search.create({
      type: 'task',
      filters: [['assigned', 'anyof', employeeId]],
      columns: ['title', 'startdate', 'priority']
    }).run().each((result) => {
      tasks.push({
        id: result.id,
        title: result.getValue({ name: 'title' }),
        start: result.getValue({ name: 'startdate' })
      });
      return tasks.length < 200;
    });
    return { ok: true, tasks };
  };

  const post = (requestBody) => {
    const created = [];
    const failed = [];
    (requestBody.tasks || []).forEach((item, index) => {
      try {
        const task = record.create({ type: 'task' });
        task.setValue({ fieldId: 'title', value: item.title });
        task.setValue({ fieldId: 'assigned', value: item.assignee });
        task.setValue({ fieldId: 'message', value: item.notes || '' });
        created.push(task.save());
      } catch (e) {
        log.error({ title: `Task ${index} rejected`, details: e.message });
        failed.push({ index, reason: e.message });
      }
    });
    return { ok: failed.length === 0, created, failed };
  };

  return { get, post };
});
```

Calling that RESTlet from a scheduled script in the same account. No authorization header is set:
`https.requestRestlet` adds authentication itself, and the RESTlet runs with this script's
privileges. The script and deployment IDs are invented:

```js
/**
 * @NApiVersion 2.1
 * @NScriptType ScheduledScript
 */
define(['N/https', 'N/log'], (https, log) => {
  const execute = () => {
    const response = https.requestRestlet({
      scriptId: 'customscript_fs_task_api',
      deploymentId: 'customdeploy_fs_task_api',
      method: 'GET',
      urlParams: { employee: '412' }
    });
    const payload = JSON.parse(response.body);
    if (response.code !== 200 || !payload.ok) {
      log.error({ title: 'Task lookup failed', details: response.body });
      return;
    }
    log.audit({ title: 'Open tasks', details: `${payload.tasks.length} found` });
  };

  return { execute };
});
```
