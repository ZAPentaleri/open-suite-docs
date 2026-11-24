# Global Objects
<!-- id: topic:global_objects -->

See also: [Globals reference](../globals.md), [N/log](../modules/N_log.md), [N/util](../modules/N_util.md).

A handful of names are available in every SuiteScript 2.x script without listing anything in the
`define` dependency array:

| Global | Reference |
|---|---|
| `define` | [global:define](../globals.md) |
| `require` | [global:require](../globals.md) |
| `log` | [global:log](../globals.md) |
| `util` | [global:util](../globals.md) |
| `toString` | [global:toString](../globals.md) |
| `JSON` | [global:JSON](../globals.md) |
| `Promise` | [global:Promise](../globals.md) |
| `Iterator` | [global:Iterator](../globals.md) |

## Import them anyway

Being available is not the same as being safe to rely on. The recommendation, especially for
SuiteScript 2.1, is to load `require`, [N/log](../modules/N_log.md) and [N/util](../modules/N_util.md)
as ordinary dependencies rather than reaching for the global names.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType ScheduledScript
 */
define(['N/log', 'N/util'], (log, util) => {
  const execute = () => {
    const batch = { region: 'EMEA', openOrders: 42 };
    // log and util come from the dependency array, not from the global scope.
    if (util.isObject(batch)) {
      log.audit({ title: 'Nightly batch', details: JSON.stringify(batch) });
    }
  };

  return { execute };
});
```

`JSON` here is the built-in global; see the [globals reference](../globals.md) for its members.
