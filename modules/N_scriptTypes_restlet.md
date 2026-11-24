# N/scriptTypes/restlet
<!-- id: N/scriptTypes/restlet -->

`N/scriptTypes/restlet` lets a RESTlet send a custom HTTP response, in particular one whose content
type you choose. Without it, a RESTlet's reply takes the content type of the request; a response built
with this module overrides that default. That helps when, for example, a caller posts JSON but wants
CSV or plain text back. See [RESTlet](../script-types/Restlet.md) for how request and response formats
work otherwise.

- **Availability:** server side, [RESTlet](../script-types/Restlet.md) scripts only.
- **Load it as:** `define(['N/scriptTypes/restlet'], (restlet) => { ... })`
- **Since:** 2024.1

## Members

| Member | Kind | Summary |
|---|---|---|
| [`restlet.createResponse(options)`](#restletcreateresponseoptions) | function | Build a reply with your own content type |
| [`Response`](#response-object) | object | The custom reply |

## Functions

### restlet.createResponse(options)
<!-- id: N/scriptTypes/restlet.createResponse -->

Builds a custom RESTlet response from a body and a content type. The source does not spell out how
the resulting object is sent; the example below returns it from the entry point function.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.contentType` | string | Yes | |
| `options.content` | string | Yes | |

- `options.contentType`: the content type of the reply. It overrides the default; when no custom
  response is used, the reply has the same content type as the request.
- `options.content`: the response body, as a string.

**Returns:** `restlet.Response`, a read-only custom response.

**Errors:**

- `SSS_MISSING_REQD_ARGUMENT`: a required argument was left out.
- `SSS_INVALID_TYPE_ARG`: an argument has the wrong type.

**Governance:** 0 units.

**Availability:** server side; RESTlet scripts only.

**Since:** 2024.1

## Objects

### Response (object)
<!-- id: N/scriptTypes/restlet.Response -->

The read-only custom response returned by [`restlet.createResponse`](#restletcreateresponseoptions).
Server side, RESTlet scripts only; since 2024.1.

#### Response.contentType
<!-- id: N/scriptTypes/restlet.Response#contentType -->

`string`, read-only. The value of the content type header of the reply. Since 2024.1.

#### Response.content
<!-- id: N/scriptTypes/restlet.Response#content -->

`string`, read-only. The body of the reply. Since 2024.1.

## Examples

A RESTlet that receives a JSON request for one vendor's purchase orders and answers in CSV, so a
spreadsheet tool can import the reply as it is.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType Restlet
 */
define(['N/scriptTypes/restlet', 'N/search'], (restlet, search) => {
  const post = (body) => {
    const columns = ['tranid', 'trandate', 'total'];
    const rows = [columns.join(',')];
    search.create({
      type: search.Type.PURCHASE_ORDER,
      filters: [['entity', 'anyof', String(body.vendorId)]],
      columns
    }).run().each((result) => {
      rows.push(columns.map((name) => result.getValue({ name })).join(','));
      return rows.length < 500;
    });

    return restlet.createResponse({
      contentType: 'text/csv',
      content: rows.join('\n')
    });
  };

  return { post };
});
```

A GET handler that answers in plain text for a monitoring probe, whatever the probe sent:

```js
/**
 * @NApiVersion 2.1
 * @NScriptType Restlet
 */
define(['N/scriptTypes/restlet', 'N/runtime'], (restlet, runtime) => {
  const get = () => {
    const units = runtime.getCurrentScript().getRemainingUsage();
    return restlet.createResponse({
      contentType: 'text/plain',
      content: `OK release=${runtime.version} units=${units}`
    });
  };

  return { get };
});
```
