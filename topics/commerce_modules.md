# N/commerce Modules
<!-- id: topic:commerce_modules -->

See also: [SuiteScript 2.1 Modules](suitescript_modules.md).

The N/commerce modules let SuiteCommerce SSP applications be written in SuiteScript 2.x. They form a
group of modules under the `N/commerce/` path; `N/commerce` on its own is **not** a module and cannot
be loaded as a dependency.

## Requirements

- Supported by the latest SuiteCommerce release and by SuiteCommerce Advanced 2019.2 and later.
- An active shopping session is required.

## Modules in the group

- [N/commerce/recordView](../modules/N_commerce_recordView.md): fast, cached access to public data
  about items and website settings. Its functions are:
  - [`recordView.viewItems(options)`](../modules/N_commerce_recordView.md#recordviewviewitemsoptions):
    reads chosen fields of several items at once. Pass the item IDs in `options.ids` and the field
    IDs in `options.fields`; the allowed fields (record fields and synthetic fields) are listed in
    [catalog:recordview_item_fields](../gen/catalogs/recordview_item_fields.md). The optional
    `options.fieldOptions` carries `includeVat` (off by default), which affects the
    `onlinecustomerprice_detail` field. The result is an array of objects keyed by field ID, each
    holding the field's value; for a select field, the value holds both the ID and the label.
  - [`recordView.viewWebsite(options)`](../modules/N_commerce_recordView.md): reads chosen fields of
    one website; the allowed fields are in
    [catalog:recordview_website_fields](../gen/catalogs/recordview_website_fields.md).

## Limits

- The modules do not cover every interaction a web store can have.
- The older commerce APIs remain available only to SSP applications written in SuiteScript 1.0;
  2.x code uses these modules instead.

## Example

A helper module for an SSP application that builds short shelf labels for a list of items, using
only fields that `viewItems` exposes. The exact layout of each returned entry beyond "field ID to
an object holding the value" is not spelled out in the source, so the code reads `.value` defensively.

```js
/**
 * @NApiVersion 2.1
 */
define(['N/commerce/recordView'], (recordView) => {
  const valueOf = (entry, fieldId) => {
    const field = entry && entry[fieldId];
    return field ? field.value : undefined;
  };

  const shelfLabels = (itemIds) => {
    const items = recordView.viewItems({
      ids: itemIds,
      fields: ['itemid', 'storedisplayname', 'isinstock']
    });
    return (items || []).map((entry) => {
      const name = valueOf(entry, 'storedisplayname') || valueOf(entry, 'itemid');
      return valueOf(entry, 'isinstock') ? name : `${name} (back soon)`;
    });
  };

  return { shelfLabels };
});
```
