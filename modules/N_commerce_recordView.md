# N/commerce/recordView
<!-- id: N/commerce/recordView -->

`N/commerce/recordView` gives scripts fast, cached access to the public data a web store needs:
selected fields of items, and the settings of a website. It suits storefront-style lookups where
speed matters more than reading a whole record. Each call names the fields it wants; both ordinary
record fields and synthetic (computed) fields can be requested, and the supported field IDs are fixed
lists, linked below.

- **Availability:** client and server scripts.
- **Since:** 2019.1 (both functions).
- **Load it as:** `define(['N/commerce/recordView'], (recordView) => { ... })`
- Field lists: [item fields](../gen/catalogs/recordview_item_fields.md),
  [website fields](../gen/catalogs/recordview_website_fields.md).
- Related: [Commerce modules](../topics/commerce_modules.md).

## Members

| Member | Kind | Summary |
|---|---|---|
| [`recordView.viewItems(options)`](#recordviewviewitemsoptions) | function | Read chosen fields of several items |
| [`recordView.viewWebsite(options)`](#recordviewviewwebsiteoptions) | function | Read chosen settings of one website |

## Shape of the results

The spec describes the result of both functions as an array of objects. In each object, every
requested field ID is a key whose value is an object holding the field's value. For a select field,
that value carries both the internal ID and the label of the selected option. The declared return
type, however, is a plain `Object`, and the source's prose describes the result more loosely than its
sample does (see the notes under each function). The source does not name the property names inside
the value objects, so inspect a real response before reading them.

## Functions

### recordView.viewItems(options)
<!-- id: N/commerce/recordView.viewItems -->

Returns the items whose internal IDs you pass, each with only the fields you asked for. You can mix
record fields (such as `itemid` or `storedisplayname`) with synthetic fields (such as `isinstock` or
`onlinecustomerprice_detail`); the supported fields are listed in
[recordview_item_fields](../gen/catalogs/recordview_item_fields.md).

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.fields` | string \| string[] | Yes | |
| `options.fieldOptions` | Array | No | |
| `options.ids` | number[] | Yes | |

- `options.fields`: one field ID or an array of them, taken from the item field catalog.
- `options.fieldOptions`: extra settings for particular fields. The one the source names is
  `includeVat`, which changes what the `onlinecustomerprice_detail` synthetic field reports; it is
  `false` unless you set it. The source does not show the exact shape of a `fieldOptions` entry.
- `options.ids`: internal IDs of the items to read.

**Returns:** `Object`: the requested items with the requested fields. Per the spec this is an array
of objects keyed by field ID, each key holding a value object (ID plus label for select fields); see
[Shape of the results](#shape-of-the-results).

> [!SOURCE-ERROR] The source's prose describes the return value as a flat set of item fields, while
> its sample shows an array of objects whose field entries are value objects. The spec does not say
> which is authoritative; log one real response and code against what you see.
<!-- source-error: N/commerce/recordView.viewItems | - | returns_flat_prose_vs_array_of_value_objects_example -->

**Errors:**

- `SSS_INVALID_TYPE_ARG`: an argument has a value that is not acceptable.
- `FIELD_1_CANNOT_BE_EMPTY`: a required argument was left out.

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2019.1

### recordView.viewWebsite(options)
<!-- id: N/commerce/recordView.viewWebsite -->

Returns the details of one website, limited to the fields you request. Record fields and synthetic
fields (such as `currencies`, `languages` or `shiptocountries`) can both be requested; see
[recordview_website_fields](../gen/catalogs/recordview_website_fields.md). Unlike `viewItems`, this
function supports no field options.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.fields` | string \| string[] | Yes | |
| `options.fieldOptions` | Array | No | |
| `options.id` | number | Yes | |

- `options.fields`: one field ID or an array of them, taken from the website field catalog.
- `options.fieldOptions`: listed as a parameter, but no field options are supported for websites, so
  leave it out.
- `options.id`: internal ID of the website.

> [!SOURCE-ERROR] The source lists `subsidiaries` both among the record fields and among the synthetic
> fields of a website. It does not say which is correct; you can request it by ID either way, but
> check its shape in a real response before depending on it.
<!-- source-error: N/commerce/recordView.viewWebsite | - | subsidiaries_listed_as_record_and_synthetic_field -->

> [!SOURCE-ERROR] One website field ID is spelled `custromeregistrationtype` [sic], which looks like a
> misspelling of a customer registration type field. The spec keeps that spelling; test which form
> the API accepts before relying on it.
<!-- source-error: N/commerce/recordView.viewWebsite | - | field_id_looks_misspelled -->

**Returns:** `Object`: the website's requested fields. Per the spec this is an array of objects
keyed by field ID, each key holding a value object (ID plus label for select fields); see
[Shape of the results](#shape-of-the-results).

> [!SOURCE-ERROR] As with `viewItems`, the source's prose describes a flat set of website fields
> while its sample shows an array of objects holding value objects. The spec does not settle which
> is right; inspect a real response before parsing it.
<!-- source-error: N/commerce/recordView.viewWebsite | - | returns_flat_prose_vs_array_of_value_objects_example -->

**Errors:**

- `SSS_INVALID_TYPE_ARG`: an argument has a value that is not acceptable.
- `FIELD_1_CANNOT_BE_EMPTY`: a required argument was left out.

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2019.1

## Examples

A RESTlet that tells a storefront widget whether a handful of items can be displayed and are in
stock, with their online price detail. No `fieldOptions` are passed, so `includeVat` stays off. The
result is returned untouched because its exact shape should be confirmed against a live response.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType Restlet
 */
define(['N/commerce/recordView'], (recordView) => {
  const get = (requestParams) => {
    const ids = String(requestParams.items || '')
      .split(',')
      .filter(Boolean)
      .map(Number);
    if (!ids.length) {
      return { items: [] };
    }
    const result = recordView.viewItems({
      ids,
      fields: ['storedisplayname', 'isinstock', 'isdisplayable', 'onlinecustomerprice_detail']
    });
    return { items: result };
  };

  return { get };
});
```

Reading a few checkout-related settings of one site and passing them through unchanged:

```js
/**
 * @NApiVersion 2.1
 * @NScriptType Suitelet
 */
define(['N/commerce/recordView'], (recordView) => {
  const onRequest = (context) => {
    const siteId = Number(context.request.parameters.site);
    const site = recordView.viewWebsite({
      id: siteId,
      fields: ['displayname', 'requireloginforpricing', 'shiptocountries', 'currencies']
    });
    context.response.write({ output: JSON.stringify(site) });
  };

  return { onRequest };
});
```
