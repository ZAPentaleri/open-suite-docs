# N/url
<!-- id: N/url -->

`N/url` resolves NetSuite URLs and formats URL strings, so you do not have to hard-code addresses.
It can work out the address of a record, of a Suitelet or RESTlet deployment, of a NetSuite page
given its task ID, or of one of an account's domains, and it can serialize a set of query
parameters onto a base URL. Every function costs 0 governance units.

- **Availability:** client and server scripts. `resolveRecord`, `resolveScript` and
  `resolveTaskLink` are not supported in unauthenticated client contexts.
- **Load it as:** `define(['N/url'], (url) => { ... })`
- **Errors:** the source does not state which errors any `N/url` function can raise.
- Related: [N/redirect](N_redirect.md) (send the user somewhere instead of computing the address),
  [N/https](N_https.md).

## Members

| Member | Kind | Summary |
|---|---|---|
| [`url.format(options)`](#urlformatoptions) | function | Serialize query parameters onto a base URL |
| [`url.resolveDomain(options)`](#urlresolvedomainoptions) | function | Domain name of an account for a host type |
| [`url.resolveRecord(options)`](#urlresolverecordoptions) | function | URL of a record |
| [`url.resolveScript(options)`](#urlresolvescriptoptions) | function | URL of a Suitelet or RESTlet deployment |
| [`url.resolveTaskLink(options)`](#urlresolvetasklinkoptions) | function | Internal URL of a NetSuite page by task ID |
| [`url.HostType`](#urlhosttype) | enum | Kinds of account domain |

## Functions

### url.format(options)
<!-- id: N/url.format -->

Takes a base address and an object of parameters and returns the address with the parameters
serialized onto it as a query string.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.domain` | string | Yes | |
| `options.params` | Object | Yes | |

**Returns:** `string`, the URL with its query parameters.

**Errors:** the source does not state any.

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2015.1

### url.resolveDomain(options)
<!-- id: N/url.resolveDomain -->

Returns the domain name an account uses for a particular kind of traffic (UI, forms, RESTlets and so
on). If you need to find an account ID, it is shown in the UI under Setup > Company > Company
Information.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.hostType` | string | Yes | |
| `options.accountId` | string | No | |

- `options.hostType`: a value from [`url.HostType`](#urlhosttype).
- `options.accountId`: the account whose domain you want; when omitted, the current account is
  used.

**Returns:** `string`, the domain name.

**Errors:** the source does not state any.

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2017.1

### url.resolveRecord(options)
<!-- id: N/url.resolveRecord -->

Returns the URL of a specific record, opened in view or edit mode.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.recordType` | string | Yes | |
| `options.recordId` | string \| number | Yes | |
| `options.isEditMode` | boolean | Yes | `View` |
| `options.params` | Object | No | |

- `options.recordType`: the record type ID, for example `purchaseorder`.
- `options.isEditMode`: `true` produces an edit-mode URL, `false` a view-mode URL.

> [!SOURCE-ERROR] `isEditMode` is listed as required, yet the source also gives it a default
> ("View"), which is not even a boolean. Pass `true` or `false` explicitly rather than relying on a
> default.
<!-- source-error: N/url.resolveRecord | options.isEditMode | required_but_default_stated_as_view -->

- `options.params`: custom URL parameters of your own, such as a query.

**Returns:** `string`, the record's URL.

**Errors:** the source does not state any.

**Governance:** 0 units.

**Availability:** client and server scripts, not in unauthenticated client contexts.

**Since:** 2015.1

### url.resolveScript(options)
<!-- id: N/url.resolveScript -->

Returns the URL of a RESTlet or Suitelet deployment.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.scriptId` | string \| number | Yes | |
| `options.deploymentId` | string \| number | Yes | |
| `options.returnExternalUrl` | boolean | No | `false` |
| `options.params` | Object | No | |

- `options.scriptId`: the script ID or the internal ID of a RESTlet or Suitelet.
- `options.deploymentId`: the deployment's script ID or internal ID.
- `options.returnExternalUrl`: `true` returns the external URL, which requires a trusted,
  authenticated context. The internal URL (returned by default) is only usable from client
  scripts. To call a Suitelet from server code, use
  [`https.requestSuitelet`](N_https.md#httpsrequestsuiteletoptions) instead.
- `options.params`: extra URL parameters.

**Returns:** `string`, the RESTlet or Suitelet URL.

**Errors:** the source does not state any.

**Governance:** 0 units.

**Availability:** client and server scripts, not in unauthenticated client contexts.

**Since:** 2015.1

### url.resolveTaskLink(options)
<!-- id: N/url.resolveTaskLink -->

Returns the internal URL of a NetSuite page, identified by its task ID.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.id` | string | Yes | |
| `options.params` | Map | No | |

- `options.id`: the task ID of the page.
- `options.params`: URL parameters to add (typed `Map` in the spec).

**Returns:** `string`, the page's internal URL.

**Errors:** the source does not state any.

**Governance:** 0 units.

**Availability:** client and server scripts, not in unauthenticated client contexts.

**Since:** 2015.2

> [!SOURCE-ERROR] The source dates this function to 2015.2 but its parameters to 2015.1, earlier
> than the function itself. Treat 2015.2 as the release where the function is documented and do
> not rely on the parameter dates.
<!-- source-error: N/url.resolveTaskLink | - | params_since_2015.1_before_method_since_2015.2 -->

## Enums

### url.HostType
<!-- id: N/url.HostType -->

The kinds of domain [`url.resolveDomain`](#urlresolvedomainoptions) can return. The domain results
can change without notice, so discover them at run time with `resolveDomain` instead of storing
them. Since 2017.1.

| Value | Domain label | Used for |
|---|---|---|
| `APPLICATION` | `app` | Access to the NetSuite UI |
| `CUSTOMER_CENTER` | `app` | The Customer Center |
| `FORM` | `extforms` | Online forms, which are usually Suitelets |
| `RESTLET` | `restlets_api` (as the spec records it) | Calling RESTlets from outside |
| `SUITETALK` | `suitetalk_api` (as the spec records it) | SOAP web services |

## Examples

A client script on a vendor bill that, when the linked purchase order changes, builds an edit link
to that order and writes it into the memo so the clerk can jump straight to it.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType ClientScript
 */
define(['N/url'], (url) => {
  const fieldChanged = (context) => {
    if (context.fieldId !== 'custbody_source_po') {
      return;
    }
    const poId = context.currentRecord.getValue({ fieldId: 'custbody_source_po' });
    if (!poId) {
      return;
    }
    const link = url.resolveRecord({
      recordType: 'purchaseorder',
      recordId: poId,
      isEditMode: true
    });
    context.currentRecord.setValue({ fieldId: 'memo', value: `PO: ${link}` });
  };

  return { fieldChanged };
});
```

A RESTlet that tells an integration partner where to send follow-up calls. It resolves the external
URL of a status RESTlet (the external form needs a trusted, authenticated context, which this
assumes the calling integration provides), tags it with a correlation value using `format`, and
reports the account's RESTlet domain, discovered at run time rather than stored.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType Restlet
 */
define(['N/url'], (url) => {
  const get = (requestParams) => {
    const statusEndpoint = url.resolveScript({
      scriptId: 'customscript_rma_status',
      deploymentId: 'customdeploy_rma_status',
      returnExternalUrl: true
    });
    const callbackUrl = url.format({
      domain: statusEndpoint,
      params: { correlation: requestParams.correlation || 'none' }
    });
    const restletHost = url.resolveDomain({ hostType: url.HostType.RESTLET });
    return { callbackUrl, restletHost };
  };

  return { get };
});
```

A client script that opens a Suitelet through its internal URL (internal URLs are for client-side
use).

```js
/**
 * @NApiVersion 2.1
 * @NScriptType ClientScript
 */
define(['N/url'], (url) => {
  const pageInit = () => {};

  const openReturnWizard = (rmaId) => {
    const target = url.resolveScript({
      scriptId: 'customscript_rma_wizard',
      deploymentId: 'customdeploy_rma_wizard',
      params: { rma: rmaId }
    });
    window.open(target, '_blank');
  };

  return { pageInit, openReturnWizard };
});
```
