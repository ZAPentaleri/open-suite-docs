# N/crypto/random
<!-- id: N/crypto/random -->

`N/crypto/random` produces cryptographically secure random values: raw random bytes, random
integers in a range, and version 4 UUIDs. Use it whenever an unpredictable value guards something,
such as a one-time code, a nonce or an unguessable reference.

- **Availability:** client and server scripts.
- **Server-side version requirement:** in server scripts the module requires SuiteScript 2.1. A
  server script that is not on 2.1 can still obtain a random value by calling a SuiteScript 2.1
  RESTlet through [`https.requestRestlet`](N_https.md) and letting that RESTlet use this module.
- **Load it as:** `define(['N/crypto/random'], (random) => { ... })`
- Related: [N/crypto](N_crypto.md).

## Members

| Member | Kind | Summary |
|---|---|---|
| [`random.generateBytes(options)`](#randomgeneratebytesoptions) | function | Random bytes as a `Uint8Array` |
| [`random.generateInt(options)`](#randomgenerateintoptions) | function | Random integer in a range, as a string |
| [`random.generateUUID()`](#randomgenerateuuid) | function | Random version 4 UUID |

## Functions

### random.generateBytes(options)
<!-- id: N/crypto/random.generateBytes -->

Returns the requested number of cryptographically secure random bytes.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.size` | number | Yes | |

- `options.size`: how many bytes to produce, from 1 to 512.

**Returns:** `Uint8Array` holding the random bytes.

**Governance:** 0 units.

**Availability:** client and server scripts (server scripts must be SuiteScript 2.1).

**Since:** 2023.1

### random.generateInt(options)
<!-- id: N/crypto/random.generateInt -->

Returns a secure random integer that is at least `min` and strictly below `max`. The result is a
**string**, not a number; convert it before doing arithmetic.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.max` | number | Yes | |
| `options.min` | number | No | `0` |

- `options.max`: upper bound, excluded from the possible results.
- `options.min`: lower bound, included in the possible results.

**Returns:** `string`, the random integer written as text.

**Governance:** 5 units. Another source gives 0 units; the spec does not say which is right.

> [!SOURCE-ERROR] The vendor's sources disagree on this function's cost: one gives 5 units, another
> 0. Neither is confirmed; plan for the higher figure and check actual usage if it matters.
<!-- source-error: N/crypto/random.generateInt | - | governance_sources_disagree -->

**Availability:** client and server scripts (server scripts must be SuiteScript 2.1).

**Since:** 2023.1

### random.generateUUID()
<!-- id: N/crypto/random.generateUUID -->

Returns a newly generated version 4 UUID. Takes no arguments.

**Returns:** `string`, the UUID.

**Governance:** 0 units.

**Availability:** client and server scripts (server scripts must be SuiteScript 2.1).

**Since:** 2023.1

## Examples

A user event script that stamps each new return authorization with an unguessable external ID and
a six-digit drop-off code:

```js
/**
 * @NApiVersion 2.1
 * @NScriptType UserEventScript
 */
define(['N/crypto/random'], (random) => {
  const beforeSubmit = (context) => {
    if (context.type !== context.UserEventType.CREATE) {
      return;
    }
    const rma = context.newRecord;
    const dropOffCode = random.generateInt({ min: 100000, max: 1000000 });
    rma.setValue({ fieldId: 'memo', value: `Drop-off code ${dropOffCode}` });
    rma.setValue({ fieldId: 'externalid', value: random.generateUUID() });
  };

  return { beforeSubmit };
});
```

Turning random bytes into a hex nonce for an outbound request:

```js
/**
 * @NApiVersion 2.1
 */
define(['N/crypto/random'], (random) => {
  const makeNonce = () => {
    const bytes = random.generateBytes({ size: 16 });
    return Array.from(bytes, (b) => b.toString(16).padStart(2, '0')).join('');
  };

  return { makeNonce };
});
```
