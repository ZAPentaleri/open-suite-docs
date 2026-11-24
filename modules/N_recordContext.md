# N/recordContext
<!-- id: N/recordContext -->

`N/recordContext` tells a script which context types apply to a record, so the script can branch its
logic on the answer. At present there is one context type, localization, which identifies the country
the record is executed in (the values are ISO 3166-2 country codes). A typical use is applying
country-specific rules in a script shared across several countries.

- **Availability:** client and server scripts.
- **Load it as:** `define(['N/recordContext'], (recordContext) => { ... })`
- **Since:** 2020.2

## Members

| Member | Kind | Summary |
|---|---|---|
| [`recordContext.getContext(options)`](#recordcontextgetcontextoptions) | function | Get a record's context values |
| [`RecordContext`](#recordcontext-object) | object | Map of context type to value(s) |
| [`recordContext.ContextType`](#recordcontextcontexttype) | enum | Kinds of context you can ask for |

## Functions

### recordContext.getContext(options)
<!-- id: N/recordContext.getContext -->

Determines the context of one record. You identify the record in one of two ways, never both: pass
`recordType` and `recordId` when the record is not loaded, or pass the loaded `record` object.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | No | |
| `options.recordType` | string | No | |
| `options.recordId` | string | No | |
| `options.record` | record.Record | No | |
| `options.contextTypes` | Array\<string\> | No | |

- `options.recordType`: conditionally required; needed when you are not passing a loaded record.
  Since 2020.2.
- `options.recordId`: conditionally required; needed when you are not passing a loaded record.
  Since 2020.2.
- `options.record`: conditionally required; needed when you work from a loaded record instead of a
  type and ID. Since 2020.2.
- `options.contextTypes`: the context types you want, as values from
  [`recordContext.ContextType`](#recordcontextcontexttype). Since 2020.2.

**Returns:** `recordContext.RecordContext`.

**Errors:**

- `MUTUALY_EXCLUSIVE_ARGUMENT` [sic]: `record` was passed together with `recordId` or `recordType`.

> [!SOURCE-ERROR] The source spells this error code `MUTUALY_EXCLUSIVE_ARGUMENT`, with a single L and
> no final S, whereas other modules use `MUTUALLY_EXCLUSIVE_ARGUMENTS`. The spec keeps the source
> spelling; if you match on the code, check the actual value thrown.
<!-- source-error: N/recordContext.getContext | - | error_code_spelled_MUTUALY_EXCLUSIVE_ARGUMENT -->

- `MISSING_REQD_ARGUMENT`: no record was given, and the record type or ID is missing.
- `SSS_INVALID_TYPE_ARG`: an argument has a value that is not acceptable.
- `UNKNOWN_CONTEXT_TYPE`: a requested context type is not known.
- `INVALID_UNSUPRTD_RCRD_TYP`: the record type is not valid or is not supported.

**Governance:** 10 units.

**Availability:** client and server scripts.

**Since:** 2020.2

## Objects

### RecordContext (object)
<!-- id: N/recordContext.RecordContext -->

The result of `getContext`: a map from each context type to its value. When a context has more than
one value, they come as an array. The source does not state the exact keys of this map (for example,
whether the key is the `ContextType` value or some other string) and lists no named properties.
Since 2020.2.

## Enums

### recordContext.ContextType
<!-- id: N/recordContext.ContextType -->

Context types accepted by `options.contextTypes` of `getContext`. The source does not state the string
value behind the member, so always use the enum member rather than a literal. Since 2020.2.

| Value | Meaning |
|---|---|
| `LOCALIZATION` | The country context of the record; its values are ISO 3166-2 country codes |

## Examples

A user event script on vendor bills that requires a memo carrying the supplier's tax reference when
the bill is processed in a German or Austrian context. Because the source does not name the keys of
the result, the script looks the value up with the enum member and accepts either a single value or
an array.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType UserEventScript
 */
define(['N/recordContext', 'N/error'], (recordContext, error) => {
  const DACH = ['DE', 'AT'];

  const beforeSubmit = (context) => {
    const bill = context.newRecord;
    const ctx = recordContext.getContext({
      record: bill,
      contextTypes: [recordContext.ContextType.LOCALIZATION]
    });
    const raw = ctx[recordContext.ContextType.LOCALIZATION];
    const countries = [].concat(raw || []);

    const needsMemo = countries.some((code) => DACH.includes(code));
    if (needsMemo && !bill.getValue({ fieldId: 'memo' })) {
      throw error.create({
        name: 'MEMO_REQUIRED',
        message: `Bills in ${countries.join(', ')} need a memo with the supplier tax reference.`
      });
    }
  };

  return { beforeSubmit };
});
```

Looking up the context of a record that is not loaded, from a Suitelet, and returning the raw map so
you can see its keys in a test run:

```js
/**
 * @NApiVersion 2.1
 * @NScriptType Suitelet
 */
define(['N/recordContext'], (recordContext) => {
  const onRequest = (context) => {
    const ctx = recordContext.getContext({
      recordType: 'invoice',
      recordId: String(context.request.parameters.invoice),
      contextTypes: [recordContext.ContextType.LOCALIZATION]
    });
    context.response.write({ output: JSON.stringify(ctx) });
  };

  return { onRequest };
});
```
