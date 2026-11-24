# N/plugin
<!-- id: N/plugin -->

`N/plugin` lets a script load the implementations of a custom plug-in. Given the script ID of a
custom plug-in type, you can list the script IDs of the implementations installed for it and load
one of them as an object whose methods your script then calls.

One restriction matters while developing: scripts that use this module cannot be debugged on
demand; use deployed debugging instead.

- **Availability:** server scripts only.
- **Load it as:** `define(['N/plugin'], (plugin) => { ... })`
- **Since:** both functions are available since 2016.1.

## Members

| Member | Kind | Summary |
|---|---|---|
| [`plugin.findImplementations(options)`](#pluginfindimplementationsoptions) | function | Script IDs of a plug-in type's implementations |
| [`plugin.loadImplementation(options)`](#pluginloadimplementationoptions) | function | Load one implementation as an object |

## Functions

### plugin.findImplementations(options)
<!-- id: N/plugin.findImplementations -->

Returns the script IDs of the implementations available for a custom plug-in type. When the
plug-in type is not available, you get an empty list rather than an error.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.includeDefault` | boolean | No | `true` |
| `options.type` | string | Yes | |

- `options.includeDefault`: when `true`, the default implementation is part of the list; pass
  `false` to leave it out. Since 2016.1.
- `options.type`: the script ID of the custom plug-in type to look up. Since 2016.1.

> [!SOURCE-ERROR] The source's summary table names `N/config` as the module of this function,
> although the function is documented as a member of `N/plugin`. Load it from `N/plugin`.
<!-- source-error: N/plugin.findImplementations | - | module_row_says_n_config -->

**Returns:** `string[]`, the script IDs of the implementations; an empty array when the plug-in
type is unavailable.

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2016.1

### plugin.loadImplementation(options)
<!-- id: N/plugin.loadImplementation -->

Loads one implementation of a custom plug-in type and returns it as an object. The source does not
describe the members of that object; they are whatever the plug-in's implementation provides.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.implementation` | string | No | |
| `options.type` | string | Yes | |

- `options.implementation`: the script ID of the custom plug-in implementation to load. If you
  leave it out, you get the implementation currently selected for the plug-in type in the
  account's plug-in management settings. Since 2016.1.
- `options.type`: the script ID of the custom plug-in type. Since 2016.1.

> [!SOURCE-ERROR] Like `findImplementations`, this function is attributed to `N/config` in the
> source's summary table. It belongs to `N/plugin`.
<!-- source-error: N/plugin.loadImplementation | - | module_row_says_n_config -->

**Returns:** `Object`, the plug-in implementation.

**Errors:**

- `UNABLE_TO_FIND_IMPLEMENTATION_1_FOR_PLUGIN_2`: the requested implementation or the plug-in type
  does not exist. The code is written as the source has it; `1` and `2` look like slots for the
  implementation and plug-in names.

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2016.1

## Examples

A user event script that asks every installed freight-quote implementation for a price and keeps
the lowest. The plug-in type `customscript_freight_quote_plugin` and its `quote(weight)` method are
invented for this example; your own plug-in type defines its interface. The script IDs returned by
`findImplementations` are passed back as `implementation`, which takes an implementation script ID.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType UserEventScript
 */
define(['N/plugin', 'N/log'], (plugin, log) => {
  const PLUGIN_TYPE = 'customscript_freight_quote_plugin';

  const beforeSubmit = (context) => {
    const order = context.newRecord;
    const weight = Number(order.getValue({ fieldId: 'custbody_ship_weight' })) || 0;

    const implIds = plugin.findImplementations({ type: PLUGIN_TYPE, includeDefault: false });
    if (!implIds.length) {
      log.debug({ title: 'Freight quote', details: 'No carrier implementations installed' });
      return;
    }

    const quotes = implIds.map((implementation) => {
      const carrier = plugin.loadImplementation({ type: PLUGIN_TYPE, implementation });
      return { implementation, amount: carrier.quote(weight) };
    });
    const best = quotes.reduce((a, b) => (b.amount < a.amount ? b : a));
    order.setValue({ fieldId: 'shippingcost', value: best.amount });
  };

  return { beforeSubmit };
});
```

Using whichever implementation an administrator has selected, by leaving `implementation` out. The
plug-in type and its `score` method are invented.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType ScheduledScript
 */
define(['N/plugin', 'N/log'], (plugin, log) => {
  const execute = () => {
    let scorer;
    try {
      scorer = plugin.loadImplementation({ type: 'customscript_credit_score_plugin' });
    } catch (e) {
      log.error({ title: 'Credit scoring unavailable', details: e.message });
      return;
    }
    log.audit({ title: 'Credit scoring', details: `sample score ${scorer.score({ balance: 1200, overdueDays: 14 })}` });
  };

  return { execute };
});
```
