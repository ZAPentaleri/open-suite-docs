# N/util
<!-- id: N/util -->

`N/util` is a small toolbox. It is mostly type checks, the `is...` functions, each of which returns a
boolean. It also has `each`, which iterates over an object or array, and `extend`, which copies
properties from one object onto another. The same functions are available without importing anything,
through the global `util` object (see [Globals](../globals.md)).

- **Availability:** client and server scripts.
- **Load it as:** `define(['N/util'], (util) => { ... })`
- These checks look at plain JavaScript types. To tell which *SuiteScript* object you are holding (a
  record, a search, and so on), the recommended approach is to look at the object's `toString()`
  output instead.
- `isFunction` also returns `true` for async functions.

## Members

| Member | Kind | Summary |
|---|---|---|
| [`util.isArray(obj)`](#utilisarrayobj) | function | Is it an array? |
| [`util.isBoolean(obj)`](#utilisbooleanobj) | function | Is it a boolean? |
| [`util.isDate(obj)`](#utilisdateobj) | function | Is it a Date? |
| [`util.isFunction(obj)`](#utilisfunctionobj) | function | Is it a function (sync or async)? |
| [`util.isAsyncFunction(obj)`](#utilisasyncfunctionobj) | function | Is it an async function? |
| [`util.isNumber(obj)`](#utilisnumberobj) | function | Is it a number (primitive or object)? |
| [`util.isObject(obj)`](#utilisobjectobj) | function | Is it a plain object? |
| [`util.isRegExp(obj)`](#utilisregexpobj) | function | Is it a regular expression? |
| [`util.isString(obj)`](#utilisstringobj) | function | Is it a string (primitive or object)? |
| [`util.each(iterable, callback)`](#utileachiterable-callback) | function | Call a function for each member |
| [`util.extend()`](#utilextend) | function | Copy properties onto a receiver |

## Functions

### util.isArray(obj)
<!-- id: N/util.isArray -->

`true` when the value is an array.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `obj` | Object \| Primitive | Yes | |

**Returns:** `boolean`

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2016.1

### util.isBoolean(obj)
<!-- id: N/util.isBoolean -->

`true` when the value is a boolean.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `obj` | Object \| Primitive | Yes | |

**Returns:** `boolean`

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2016.1

### util.isDate(obj)
<!-- id: N/util.isDate -->

`true` when the value is a date.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `obj` | Object \| Primitive | Yes | |

**Returns:** `boolean`

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2016.1

### util.isFunction(obj)
<!-- id: N/util.isFunction -->

`true` when the value is a function, ordinary or async.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `obj` | Object \| Primitive | Yes | |

**Returns:** `boolean`

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2016.1

### util.isAsyncFunction(obj)
<!-- id: N/util.isAsyncFunction -->

`true` when the value is an async function. Use it when you need to tell async functions apart, since
`isFunction` accepts both kinds.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `obj` | Object \| Primitive | Yes | |

**Returns:** `boolean`

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2020.1

### util.isNumber(obj)
<!-- id: N/util.isNumber -->

`true` when the value is a number, whether a primitive (`7`) or a `Number` object.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `obj` | Object \| Primitive | Yes | |

**Returns:** `boolean`

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2016.1

### util.isObject(obj)
<!-- id: N/util.isObject -->

`true` when the value is a plain object. A typical use is telling an object apart from a function.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `obj` | Object \| Primitive | Yes | |

**Returns:** `boolean`

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2016.1

### util.isRegExp(obj)
<!-- id: N/util.isRegExp -->

`true` when the value is a regular expression.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `obj` | Object \| Primitive | Yes | |

**Returns:** `boolean`

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2016.1

### util.isString(obj)
<!-- id: N/util.isString -->

`true` when the value is a string, whether a primitive (`'abc'`) or a `String` object.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `obj` | Object \| Primitive | Yes | |

**Returns:** `boolean`

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2016.1

### util.each(iterable, callback)
<!-- id: N/util.each -->

Calls `callback` once for each member of an object or array, then returns the collection you passed
in.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `iterable` | Object \| Array | Yes | |
| `callback` | Function | Yes | |

- `iterable`: either a plain object or an array.
- `callback`: called once per member. The source does not state which arguments it receives, nor
  whether its return value can stop the iteration early.

**Returns:** `Object | Array`, the original collection.

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2016.1

### util.extend()
<!-- id: N/util.extend -->

Copies the properties of a contributor object onto a receiver object and returns the receiver.

The source lists no parameters for this function (no names, order or types), so it is shown here
without a parameter table. The description implies at least a receiver and a contributor, but the
source does not state their order, and it does not say whether the copy is shallow or deep. Confirm
both before you rely on it.

**Returns:** `Object`, the receiver.

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2016.1

## Examples

**Normalizing loosely typed RESTlet input.** A RESTlet accepts a single SKU or a list of SKUs, plus an
optional minimum quantity that may come in as a string or a number. The type checks decide how to
read each field.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType Restlet
 */
define(['N/util'], (util) => {
  const asList = (value) => {
    if (util.isArray(value)) {
      return value.filter((entry) => util.isString(entry));
    }
    return util.isString(value) ? [value] : [];
  };

  const asLimit = (value) => {
    if (util.isNumber(value)) {
      return Number(value);
    }
    if (util.isString(value) && value.trim() !== '') {
      const parsed = Number(value);
      return Number.isNaN(parsed) ? null : parsed;
    }
    return null;
  };

  const post = (body) => {
    if (!util.isObject(body)) {
      return { ok: false, error: 'Expected a JSON object' };
    }
    const skus = asList(body.skus);
    const minimumQty = asLimit(body.minimumQty);
    return { ok: true, skus, minimumQty };
  };

  return { post };
});
```

**Accepting either a plain or an async hook.** A small helper in a library module runs a caller-supplied
hook and only awaits it when it is async. The second helper counts the entries of a lookup table with
`util.each`; because the callback's arguments are not documented, it ignores them and only relies on
being called once per member.

```js
/**
 * @NApiVersion 2.1
 */
define(['N/util'], (util) => {
  const runHook = async (hook, payload) => {
    if (!util.isFunction(hook)) {
      return payload;
    }
    if (util.isAsyncFunction(hook)) {
      return await hook(payload);
    }
    return hook(payload);
  };

  const countEntries = (table) => {
    let count = 0;
    util.each(table, () => {
      count += 1;
    });
    return count;
  };

  return { runHook, countEntries };
});
```
