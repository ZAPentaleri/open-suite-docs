# SuiteScript 2.1 advantages
<!-- id: topic:ss21_advantages -->

Part of: [SuiteScript 2.1 overview](ss21_overview.md)

## Which version to use

Write every new script in SuiteScript 2.1. SuiteScript 1.0 is still supported but receives no new
features (see [Transitioning to SuiteScript 2.1](ss21_transition.md) for the retirement schedule).

## Why modules help

- Code is split into modules, each with its own scope, so internals stay encapsulated.
- Names no longer collide in one global namespace.
- Custom modules can be shared between scripts.
- The module loader works out and loads each script's dependencies for you.

## What changed from 1.0

| 1.0 | 2.1 |
|---|---|
| Global `nlapi*` functions and `nlobj*` objects | Module members with no prefixes |
| Getter and setter methods | Properties on objects |
| Magic strings for constants | Enums |
| Sublist lines numbered from 1 | Sublist lines indexed from 0 |
| Third-party libraries loaded ad hoc | Third-party libraries wrapped as custom modules |

## What only 2.x offers

| Capability | Where |
|---|---|
| Map/reduce processing | [MapReduceScript](../script-types/MapReduceScript.md) |
| Promises | Many modules offer `.promise` variants |
| SFTP transfers | [N/sftp](../modules/N_sftp.md) |
| Caching | [N/cache](../modules/N_cache.md) |
| Paged search results | [N/search](../modules/N_search.md) |
| Streaming large flat files | [N/file](../modules/N_file.md) |
| Setting the HTTP content type | [N/http](../modules/N_http.md), [N/https](../modules/N_https.md) |
| Encryption and encoding | [N/crypto](../modules/N_crypto.md), [N/encode](../modules/N_encode.md) |

- **Promises:** client scripts can use promises with every module; server scripts only with
  selected modules. Client scripts may also create their own promises.
- **Map/reduce:** processing several stages in parallel needs SuiteCloud Plus. The number of
  processors is set on the script deployment record.
- **Flat-file streaming:** each line can be at most 10 MB.
- **SFTP:** the host key may be DSA, ECDSA or RSA.

## Example

A client script that leans on two of the points above: a module instead of a global function, and
a promise-based call so the page stays responsive while a lookup runs.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType ClientScript
 */
define(['N/search', 'N/ui/message'], (search, message) => {
  const fieldChanged = (context) => {
    if (context.fieldId !== 'assigned') {
      return;
    }
    const employeeId = context.currentRecord.getValue({ fieldId: 'assigned' });
    if (!employeeId) {
      return;
    }
    search.lookupFields.promise({ type: 'employee', id: employeeId, columns: ['email'] })
      .then((result) => {
        message.create({
          title: 'Assignee',
          message: `Notifications will go to ${result.email || 'no email on file'}.`,
          type: message.Type.INFORMATION
        }).show({ duration: 5000 });
      })
      .catch((e) => console.error(e.message));
  };

  return { fieldChanged };
});
```
