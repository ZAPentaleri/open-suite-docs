# Map/Reduce Testing
<!-- id: topic:map_reduce.testing -->

Part of: [Map/reduce processing model](map_reduce.md)

See also: [MapReduceScript](../script-types/MapReduceScript.md), [N/log](../modules/N_log.md),
[Map/Reduce Status Page](map_reduce.status_page.md), [Map/reduce interruption handling](map_reduce.interruptions.md).

## Ways to test

Four techniques complement each other when you test a
[map/reduce script](../script-types/MapReduceScript.md):

- **A unit test suite** that exercises each entry point by itself.
- **Execution logs** written with [N/log](../modules/N_log.md).
- **Status monitoring** on the [Map/Reduce Status Page](map_reduce.status_page.md).
- **Restart detection** through the `isRestarted` flag of the input, map, reduce and summary contexts
  ([`inputContext.isRestarted`](../script-types/MapReduceScript.md#inputcontextisrestarted),
  [`mapContext.isRestarted`](../script-types/MapReduceScript.md#mapcontextisrestarted),
  [`reduceContext.isRestarted`](../script-types/MapReduceScript.md#reducecontextisrestarted),
  [`summaryContext.isRestarted`](../script-types/MapReduceScript.md#summarycontextisrestarted)).

You cannot step through a deployed map/reduce script in the SuiteScript Debugger. Testing a
deployment requires its status to be **Testing** and the logged-in user to be the script's owner.

## Unit testing the entry points

- Structure each entry point as a self-contained section so it can be tested on its own.
- When a test has to pass modules in, load them with the [`require`](../globals.md) function using
  absolute paths.
- Tests for `map` and `reduce` need a mock context seeded with the values the function should see.
- `getInputData` takes no parameters, so its tests need no mock context.
- `summarize` is verified with assertions and with log output.
- You can also test how the script depends on other script types.

## Where the mock schemas come from

The context shapes for your mocks are defined in the SuiteScript 2.0 API zip file stored in the File
Cabinet. Inside it, the two files that matter are `mapreducecontext.js` (the map/reduce context
shape) and `mapreducesummary.js` (the summary shape). The spec gives these names in lower case only
and does not state their folder inside the archive. To download the schema files, use the SuiteScript
2.0 API link in the File Cabinet.

> [!SOURCE-ERROR] A page about the development environment calls this link the SuiteScript 2.1 API
> link, while the testing material says SuiteScript 2.0. The spec keeps the 2.0 wording and does not
> say which label is current; look for either link in your File Cabinet.
<!-- source-error: topic:map_reduce.testing | - | dev_environment_page_names_suitescript_2_1_api_link -->

## Example

Keep each entry point self-contained: here `map` only reads `value` and `isRestarted` and calls
`write`, all of which a mock context can supply.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType MapReduceScript
 */
define(['N/log'], (log) => {
  const getInputData = () => [
    { orderId: 'SO-1042', amount: 15200 },
    { orderId: 'SO-1043', amount: 480 }
  ];

  const map = (context) => {
    const order = JSON.parse(context.value);
    if (context.isRestarted) {
      log.debug({ title: 'Map restarted', details: order.orderId });
    }
    const bucket = order.amount >= 10000 ? 'large' : 'standard';
    context.write({ key: bucket, value: order.orderId });
  };

  const summarize = (summary) => {
    summary.output.iterator().each((bucket, orderId) => {
      log.debug({ title: `Bucket ${bucket}`, details: orderId });
      return true;
    });
  };

  return { getInputData, map, summarize };
});
```

A test (plain JavaScript, not a SuiteScript API) can then build a fake context such as
`{ key: '0', value: '{"orderId":"SO-1","amount":20000}', isRestarted: false, write: (pair) => captured.push(pair) }`,
pass it to `map`, and assert that `captured` holds one pair with the key `large`.
