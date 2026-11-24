# N/https/clientCertificate
<!-- id: N/https/clientCertificate -->

`N/https/clientCertificate` sends SSL requests that present a client certificate, for endpoints
that only accept callers who identify themselves with one. You point the call at the certificate by
its ID. Every function returns an [`https.ClientResponse`](N_https.md#clientresponse-object).

- **Availability:** server scripts only.
- **Permission:** the running role needs the Certificate Access permission (`LIST_CERTIFICATES` in
  the [permissions catalog](../gen/catalogs/permissions.md)).
- **Load it as:** `define(['N/https/clientCertificate'], (clientCertificate) => { ... })`
- **Timeouts (all functions):** 5 seconds to negotiate the connection, 45 seconds to transfer the
  payload.
- **Since:** the source does not state when the module itself was introduced; `post` dates from
  2019.1 and the other functions from 2019.2.
- **Errors:** the source does not state which errors any of these functions can raise.
- Related: [N/https](N_https.md) (the `Method` enum and the `ClientResponse` object live there).

## Members

| Member | Kind | Summary |
|---|---|---|
| [`clientCertificate.get(options)`](#clientcertificategetoptions) | function | GET with a client certificate |
| [`clientCertificate.post(options)`](#clientcertificatepostoptions) | function | POST with a client certificate |
| [`clientCertificate.put(options)`](#clientcertificateputoptions) | function | PUT with a client certificate |
| [`clientCertificate.delete(options)`](#clientcertificatedeleteoptions) | function | DELETE with a client certificate |
| [`clientCertificate.request(options)`](#clientcertificaterequestoptions) | function | Any method from `https.Method` with a client certificate |

## Functions

### clientCertificate.get(options)
<!-- id: N/https/clientCertificate.get -->

Sends a GET request, presenting the given client certificate.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.certId` | string | Yes | |
| `options.url` | string | Yes | |
| `options.headers` | object | No | |

- `options.certId`: the ID of the client certificate to present.

**Returns:** `https.ClientResponse`, the server's reply.

**Errors:** the source does not state any.

**Governance:** 10 units.

**Availability:** server scripts.

**Since:** 2019.2

- Connection negotiation times out after 5 seconds; payload transfer after 45 seconds.

### clientCertificate.post(options)
<!-- id: N/https/clientCertificate.post -->

Sends a POST request with a string body, presenting the given client certificate.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.certId` | string | Yes | |
| `options.url` | string | Yes | |
| `options.body` | string | Yes | |
| `options.headers` | object | No | |

- `options.certId`: the ID of the client certificate to present.

**Returns:** `https.ClientResponse`

**Errors:** the source does not state any.

**Governance:** 10 units.

**Availability:** server scripts.

**Since:** 2019.1

- Connection negotiation times out after 5 seconds; payload transfer after 45 seconds.

### clientCertificate.put(options)
<!-- id: N/https/clientCertificate.put -->

Sends a PUT request with a string body, presenting the given client certificate.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.certId` | string | Yes | |
| `options.url` | string | Yes | |
| `options.body` | string | Yes | |
| `options.headers` | object | No | |

- `options.certId`: the ID of the client certificate to present.

**Returns:** `https.ClientResponse`

**Errors:** the source does not state any.

**Governance:** 10 units.

**Availability:** server scripts.

**Since:** 2019.2

- Connection negotiation times out after 5 seconds; payload transfer after 45 seconds.

### clientCertificate.delete(options)
<!-- id: N/https/clientCertificate.delete -->

Sends a DELETE request (no body), presenting the given client certificate.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.certId` | string | Yes | |
| `options.url` | string | Yes | |
| `options.headers` | object | No | |

- `options.certId`: the ID of the client certificate to present.

**Returns:** `https.ClientResponse`

**Errors:** the source does not state any.

**Governance:** 10 units.

**Availability:** server scripts.

**Since:** 2019.2

- Connection negotiation times out after 5 seconds; payload transfer after 45 seconds.

### clientCertificate.request(options)
<!-- id: N/https/clientCertificate.request -->

The general form: you pick the HTTP method from [`https.Method`](N_https.md#httpsmethod), and the
call presents the given client certificate.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.method` | string | Yes | |
| `options.certId` | string | Yes | |
| `options.url` | string | Yes | |
| `options.headers` | object | Yes | |
| `options.body` | string | No | |

- `options.method`: a value from [`https.Method`](N_https.md#httpsmethod).
- `options.certId`: the ID of the client certificate to present.
- `options.headers`: marked required here, unlike in `get`, `post`, `put` and `delete`, where it is
  optional.

> [!SOURCE-ERROR] The source makes `headers` required for `request` but optional for the sibling
> functions, and gives no reason for the difference. Passing a headers object (even an empty one)
> satisfies both readings.
<!-- source-error: N/https/clientCertificate.request | options.headers | request_headers_required_vs_optional_in_siblings -->

- `options.body`: applies to PUT, POST and PATCH; ignored for every other method.

**Returns:** `https.ClientResponse`

**Errors:** the source does not state any.

**Governance:** 10 units.

**Availability:** server scripts.

**Since:** 2019.2

- Connection negotiation times out after 5 seconds; payload transfer after 45 seconds.

## Examples

A scheduled script submits a daily remittance file to a bank endpoint that requires a client
certificate. The certificate ID and endpoint come from script parameters, so nothing
environment-specific is hard-coded.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType ScheduledScript
 */
define(['N/https/clientCertificate', 'N/runtime'], (clientCertificate, runtime) => {
  const execute = () => {
    const script = runtime.getCurrentScript();
    const certId = script.getParameter({ name: 'custscript_remit_cert_id' });
    const endpoint = script.getParameter({ name: 'custscript_remit_upload_url' });

    const remittance = {
      valueDate: new Date().toISOString().slice(0, 10),
      lines: [{ vendorRef: 'V-3307', amount: 912.4, currency: 'CHF' }]
    };

    const response = clientCertificate.post({
      certId,
      url: endpoint,
      body: JSON.stringify(remittance),
      headers: { 'content-type': 'application/json' }
    });

    if (response.code !== 200 && response.code !== 202) {
      log.error({ title: 'Remittance refused', details: `HTTP ${response.code}` });
      return;
    }
    log.audit({ title: 'Remittance accepted', details: `${remittance.lines.length} line(s)` });
  };

  return { execute };
});
```

Checking the status of an earlier submission with `request`, passing `headers` because this
function lists it as required.

```js
/**
 * @NApiVersion 2.1
 */
define(['N/https/clientCertificate', 'N/https'], (clientCertificate, https) => {
  const fetchSubmissionStatus = (certId, statusUrl) => {
    const response = clientCertificate.request({
      method: https.Method.GET,
      certId,
      url: statusUrl,
      headers: { accept: 'application/json' }
    });
    return response.code === 200 ? JSON.parse(response.body).status : null;
  };

  return { fetchSubmissionStatus };
});
```
