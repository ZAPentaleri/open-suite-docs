# SuiteScript 2.0 vs 2.1 integration behavior
<!-- id: topic:ss21_transition.from_ss20.integration -->

Part of: [SuiteScript 2.0 to 2.1 differences](ss21_transition.from_ss20.md)

These differences show up at the edges of a script: in the values it saves, in what a RESTlet hands
back to its caller, and in which asynchronous calls work on the server.

## Decimal values saved through setValue

On 2.1, a decimal that ends in zeros after the point is saved as a whole number when set with
[`Record.setValue`](../modules/N_record.md):

| Value passed | Value saved |
|---|---|
| `616.00` | `616` |
| `616.01` | `616.01` |

If a downstream system compares the stored text, account for the missing `.00`.

## RESTlet responses

- A RESTlet's POST return value is not turned into a string for you on 2.1.
- The response is formatted according to the request's content type, so a caller that sends the
  wrong content type can get a different result than it did on 2.0.

See [Restlet](../script-types/Restlet.md) for the entry points.

## parseInt and leading zeros

On 2.1, `parseInt` reads a string with a leading zero as decimal: `parseInt('08')` is `8`. Code that
relied on another interpretation of such strings must be checked.

> [!SOURCE-ERROR] The source spells the function `parstInt` [sic] in this section. The function is
> the standard JavaScript `parseInt`; there is no `parstInt`.
<!-- source-error: topic:ss21_transition.from_ss20.integration | - | function_name_misspelled_parstint -->

## Promises and async/await in server scripts

In server scripts, promises (and therefore `async`/`await`) are supported only with these modules:
[N/http](../modules/N_http.md), [N/https](../modules/N_https.md), [N/llm](../modules/N_llm.md),
[N/query](../modules/N_query.md), [N/search](../modules/N_search.md) and
[N/transaction](../modules/N_transaction.md). Using `async`/`await` with any other module throws an
error.

> [!SOURCE-ERROR] The source writes the keyword as `asnyc` [sic]. JavaScript only recognises
> `async`; the misspelt form would be a syntax error or an ordinary identifier.
<!-- source-error: topic:ss21_transition.from_ss20.integration | - | keyword_misspelled_asnyc -->

## Example

A RESTlet written so its behaviour does not depend on the runtime: it gives `parseInt` an explicit
radix and returns a string it built itself, rather than relying on automatic conversion.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType Restlet
 */
define(['N/record'], (record) => {
  const post = (body) => {
    const input = typeof body === 'string' ? JSON.parse(body) : body;
    const qty = parseInt(String(input.quantity), 10);
    if (Number.isNaN(qty)) {
      return JSON.stringify({ ok: false, reason: 'quantity must be a whole number' });
    }
    const task = record.load({ type: record.Type.TASK, id: input.taskId });
    task.setValue({ fieldId: 'message', value: `Requested quantity: ${qty}` });
    const savedId = task.save();
    return JSON.stringify({ ok: true, savedId, qty });
  };

  return { post };
});
```
