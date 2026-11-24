# Backend Suitelet Samples
<!-- id: topic:suitelet.samples.backend -->

Part of: [Suitelet Samples](suitelet.samples.md)

See also: [Suitelet](../script-types/Suitelet.md), [N/http](../modules/N_http.md),
[N/cache](../modules/N_cache.md), [N/plugin](../modules/N_plugin.md), [N/render](../modules/N_render.md),
[N/runtime](../modules/N_runtime.md), [N/xml](../modules/N_xml.md), [N/record](../modules/N_record.md).

"Backend Suitelets" is the source's group of samples for Suitelets that answer with data, a file or a
redirect instead of building a NetSuite form. The samples in the group:

| What the sample does | Main API |
|---|---|
| Sends the user to a new sales order with the customer (entity) already filled in | [`ServerResponse.sendRedirect`](../modules/N_http.md), [`http.RedirectType`](../modules/N_http.md), [`record.Type`](../modules/N_record.md) |
| Returns the city for a ZIP code, caching answers and loading misses from a CSV file | [`Cache.get`](../modules/N_cache.md) |
| Iterates over the implementations of a custom plug-in | [`plugin.findImplementations`](../modules/N_plugin.md) |
| Renders a PDF directly into the response | [N/render](../modules/N_render.md) |
| Renders saved search results into a PDF | [`TemplateRenderer.addSearchResults`](../modules/N_render.md) |
| Writes information about the current user and session into the response | [N/runtime](../modules/N_runtime.md) |
| Loads an XML file and reads the values of child elements | [N/xml](../modules/N_xml.md) |
| Parses an XML string and logs its elements | [N/xml](../modules/N_xml.md) |

## Notes on the samples

- **Redirect sample.** It calls `sendRedirect` with the type `RECORD`, the identifier `SALES_ORDER`
  (from the record type enum) and a `parameters` object holding `entity`, so the new sales order opens
  with that customer selected. The entity value in the source sample is only a placeholder; put a real
  customer internal ID there before running it.
- **ZIP code sample.** The CSV file that the loader reads has to exist in the File Cabinet before the
  Suitelet runs.
- **Plug-in sample.** A Suitelet that uses a custom plug-in must name the plug-in type on its script
  record, on the Custom Plug-in Types subtab.

## Example: open a new sales order for a chosen customer

A "New order" link on a customer dashboard points to this Suitelet with the customer's internal ID in
the `custom_cust` URL parameter. The Suitelet checks the parameter and redirects to a blank sales
order with that customer already set, or answers with a short message when the parameter is missing.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType Suitelet
 */
define(['N/http', 'N/record'], (http, record) => {
  const onRequest = (context) => {
    const customerId = String(context.request.parameters.custom_cust || '').trim();

    if (!/^\d+$/.test(customerId)) {
      context.response.write({ output: 'Choose a customer before starting an order.' });
      return;
    }

    context.response.sendRedirect({
      type: http.RedirectType.RECORD,
      identifier: record.Type.SALES_ORDER,
      parameters: { entity: customerId }
    });
  };

  return { onRequest };
});
```

## Example: cached region lookup

An order-entry client script asks this Suitelet which sales region a postal code belongs to. Answers
are cached; on a miss, the loader reads a two-column CSV (`postal code,region`) from the File Cabinet,
and whatever the loader returns is put into the cache automatically.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType Suitelet
 */
define(['N/cache', 'N/file'], (cache, file) => {
  const CSV_PATH = 'SuiteScripts/data/postal_regions.csv';

  const loadRegion = (context) => {
    const lines = file.load({ id: CSV_PATH }).getContents().split(/\r?\n/);
    const hit = lines
      .map((line) => line.split(','))
      .find(([code]) => code && code.trim() === context.key);
    return hit && hit[1] ? hit[1].trim() : 'UNASSIGNED';
  };

  const onRequest = (context) => {
    const postal = String(context.request.parameters.custom_postal || '').trim();
    context.response.setHeader({ name: 'Content-Type', value: 'application/json' });

    if (!postal) {
      context.response.write({ output: JSON.stringify({ ok: false, reason: 'custom_postal missing' }) });
      return;
    }

    const regions = cache.getCache({ name: 'postal_region_lookup', scope: cache.Scope.PROTECTED });
    const region = regions.get({ key: postal, loader: loadRegion, ttl: 3600 });
    context.response.write({ output: JSON.stringify({ ok: true, postal, region }) });
  };

  return { onRequest };
});
```

## Example: accept a carrier's XML status push

A carrier posts parcel status updates as XML. The Suitelet parses the body, reads each
`trackingNumber` element and answers with a small acknowledgement. Malformed XML makes the parser
throw, so the call is wrapped in `try`/`catch`. A Suitelet called from outside NetSuite has to be
deployed as available without login; see [Suitelet Deployment Record](suitelet.deployment.md) for
what that implies.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType Suitelet
 */
define(['N/xml', 'N/log'], (xml, log) => {
  const onRequest = (context) => {
    const { request, response } = context;
    response.setHeader({ name: 'Content-Type', value: 'application/xml' });

    let doc;
    try {
      doc = xml.Parser.fromString({ text: request.body || '' });
    } catch (e) {
      log.error({ title: 'Rejected carrier payload', details: e.message });
      response.write({ output: '<ack status="rejected"/>' });
      return;
    }

    const parcels = doc.getElementsByTagName({ tagName: 'trackingNumber' });
    parcels.forEach((node, index) => {
      log.audit({ title: `Parcel ${index + 1}`, details: node.textContent });
    });

    response.write({ output: `<ack status="accepted" count="${parcels.length}"/>` });
  };

  return { onRequest };
});
```
