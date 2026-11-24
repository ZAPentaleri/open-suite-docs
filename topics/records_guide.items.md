# Item records
<!-- id: topic:records_guide.items -->

Part of: [SuiteScript records guide](records_guide.md)

Sub-topics:

- [Item pricing sublist](records_guide.pricing_sublist.md)

Item records describe what an account buys, sells, builds, plans or bills for, together with the
planning and subscription records that sit around them.

## Record types in this group

| Kind | Records |
|---|---|
| Generic item | [item](../gen/records/item.md) |
| Inventory and assemblies | [inventoryitem](../gen/records/inventoryitem.md), [lotnumberedinventoryitem](../gen/records/lotnumberedinventoryitem.md), [serializedinventoryitem](../gen/records/serializedinventoryitem.md), [assemblyitem](../gen/records/assemblyitem.md), [lotnumberedassemblyitem](../gen/records/lotnumberedassemblyitem.md), [serializedassemblyitem](../gen/records/serializedassemblyitem.md) |
| Other sellable items | [noninventoryitem](../gen/records/noninventoryitem.md), [serviceitem](../gen/records/serviceitem.md), [otherchargeitem](../gen/records/otherchargeitem.md), [downloaditem](../gen/records/downloaditem.md), [giftcertificateitem](../gen/records/giftcertificateitem.md), [kititem](../gen/records/kititem.md), [itemgroup](../gen/records/itemgroup.md) |
| Transaction-line helper items | [descriptionitem](../gen/records/descriptionitem.md), [discountitem](../gen/records/discountitem.md), [markupitem](../gen/records/markupitem.md), [paymentitem](../gen/records/paymentitem.md), [shipitem](../gen/records/shipitem.md), [subtotalitem](../gen/records/subtotalitem.md) |
| Supply planning | [plannedorder](../gen/records/plannedorder.md), [planningitemcategory](../gen/records/planningitemcategory.md), [planningitemgroup](../gen/records/planningitemgroup.md), [planningrulegroup](../gen/records/planningrulegroup.md), [planningview](../gen/records/planningview.md), [supplychainsnapshotsimulation](../gen/records/supplychainsnapshotsimulation.md), [supplychangeorder](../gen/records/supplychangeorder.md), [supplyplandefinition](../gen/records/supplyplandefinition.md), [reallocateitem](../gen/records/reallocateitem.md) |
| Subscriptions and usage | [subscriptionplan](../gen/records/subscriptionplan.md), [subscriptionterm](../gen/records/subscriptionterm.md), [saasmetric](../gen/records/saasmetric.md) |
| Analytical impact | [analyticalimpact](../gen/records/analyticalimpact.md), [impactsubcategory](../gen/records/impactsubcategory.md) |

The grouping into kinds is this page's own; the source gives the records as one list.

Subscription plans are priced through price books and price plans: a price book holds the pricing
for the items of one subscription plan, and every item of the plan needs a price plan before saving. Those records are documented on
[pricebook](../gen/records/pricebook.md) and [priceplan](../gen/records/priceplan.md) (both require
SuiteBilling).

## Loading lot-numbered and serialized items

The [`record.Type`](../modules/N_record.md) value for inventory items also reaches lot-numbered and
serialized inventory items, and the value for assembly items also reaches lot-numbered and
serialized assemblies. So `record.Type.INVENTORY_ITEM` and `record.Type.ASSEMBLY_ITEM` are enough to
load those variants.

## Filtering an item search by item type

The item search's `type` filter takes short type IDs, and they are **case-sensitive**: `InvtPart`
matches, `invtpart` does not.

| Type ID | Type ID | Type ID |
|---|---|---|
| `Assembly` | `Group` | `Payment` |
| `Description` | `InvtPart` | `Service` |
| `Discount` | `Kit` | `ShipItem` |
| `DwnLdItem` | `Markup` | `Subtotal` |
| `EndGroup` | `NonInvtPart` | `TaxGroup` |
| `GiftCert` | `OthCharge` | `TaxItem` |

## Reading item fields with lookupFields

[`search.lookupFields`](../modules/N_search.md) is a light way to read a few item fields. Plan for
two behaviours:

- This group's notes say a single-select field comes back as one object pairing the value with its
  display text. The `search.lookupFields` reference describes select results as an array of such
  pairs, and that difference is flagged as a source error on the module page. Write code that
  accepts either shape.
- The item `subsidiary` field is a multi-select, yet a lookup returns only the **first**
  value/text pair. When an item belongs to several subsidiaries, load the record or run a search to
  get all of them.

## Advanced Revenue Management fields

Items carry accounting fields that serve Advanced Revenue Management. Two of them, the item revenue
category (`itemrevenuecategory`) and the revenue allocation group (`revenueallocationgroup`), are set
up through the revenue allocation part of that feature (feature ID `ADVANCEDREVENUERECOGNITION` in
the [features catalog](../gen/catalogs/features.md)). See, for example, the
[inventoryitem](../gen/records/inventoryitem.md) page for their field facts.

## Example

Log the first subsidiary of each active kit or assembly. Note the exact capitalization of the type
IDs, and that the lookup returns only the first subsidiary even when several are selected.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType ScheduledScript
 */
define(['N/search', 'N/log'], (search, log) => {
  const firstPair = (value) => {
    if (Array.isArray(value)) {
      return value.length > 0 ? value[0] : null;
    }
    return value || null;
  };

  const execute = () => {
    const builtItems = search.create({
      type: search.Type.ITEM,
      filters: [
        ['type', search.Operator.ANYOF, ['Kit', 'Assembly']],
        'AND',
        ['isinactive', search.Operator.IS, 'F']
      ],
      columns: ['itemid']
    });

    builtItems.run().getRange({ start: 0, end: 50 }).forEach((row) => {
      const fields = search.lookupFields({
        type: row.recordType,
        id: row.id,
        columns: ['subsidiary']
      });
      const sub = firstPair(fields.subsidiary);
      log.debug({
        title: row.getValue({ name: 'itemid' }),
        details: `First subsidiary: ${sub ? sub.text : '(none)'}`
      });
    });
  };

  return { execute };
});
```
