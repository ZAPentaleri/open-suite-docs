# Global objects and functions
<!-- id: global:core -->

A handful of names are available in every SuiteScript 2.x script without appearing in a dependency
array. `define` and `require` are the AMD loader functions every entry point script and custom module
is built on. `JSON`, `Promise`, `Iterator` and `toString` are general facilities with
SuiteScript-specific rules. `log` and `util` are preloaded copies of the [N/log](modules/N_log.md)
and [N/util](modules/N_util.md) modules.

- Related: [Global Objects topic](topics/global_objects.md), [N/log](modules/N_log.md),
  [N/util](modules/N_util.md).
- Where a global stands for module members or modules, its section links to them; the full
  reference lives on the module pages.

## Members

| Member | Kind | Summary |
|---|---|---|
| `define(id, dependencies, moduleObject)` | function | Declare an entry point script or a custom module |
| `require(dependencies, callback)` | function | Load existing modules lazily, on first use |
| `Iterator(callback)` | function | Walk a collection one item at a time |
| `toString()` | function | Find out what type a native SuiteScript object is |
| `JSON` | object | Parse and produce JSON text |
| `log` | object | Preloaded copy of `N/log` |
| `util` | object | Preloaded copy of `N/util` |
| `Promise` | object | Placeholder for a result that arrives later |
| `require.configuration` | object | Settings that control how custom modules are found and loaded |

## Functions

### define(id, dependencies, moduleObject)
<!-- id: global:define -->

`define` is how you build both entry point scripts and your own reusable modules. It follows the AMD
specification: every listed dependency is loaded first, and only then does your callback run, with
the loaded modules passed in. On the client this loading is asynchronous; on the server it is
synchronous.

The spec records two call shapes:

- `define(moduleObject)`: for a module with no dependencies.
- `define(id, [dependencies,] moduleObject)`: for a module that needs dependencies.

Of the three parameters only `moduleObject` is required; `id` and `dependencies` are both optional.
That is what makes the everyday form `define([dependencies], moduleObject)` valid: you leave out the
optional `id` and pass the dependency array and the callback. The signature list itself does not
spell that shorter form out; it follows from `id` being optional.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `moduleObject` | Function \| Object | Yes | |
| `id` | string | No | |
| `dependencies` | string[] | No | |

- `moduleObject`: may be a function (the usual case), an object, or a plain primitive value. The
  module object that `define` produces is built from it.
- `id`: a module identifier. Omit it unless you need to name the module.
- `dependencies`: each entry is either a SuiteScript module such as `N/record` or a File Cabinet path
  to one of your own modules. There is no limit on how many you list. Dependencies can only come from
  the File Cabinet; scripts cannot be imported over HTTP or HTTPS.

**Returns:** `Object`. For an entry point script it is an object whose keys are entry point names and
whose values are the named functions that implement them. For a custom module it is the module
object, which other scripts can then list as a dependency.

**Errors:**

- `MODULE_DOES_NOT_EXIST`: a listed dependency cannot be found. Only the first missing module is
  reported, so when you see this error check every module path and name in the list, not just the
  one in the message.

**Governance:** not documented; assumed to be free (0 units) by project convention.

**Availability:** the source describes `define` running on both the client (asynchronously) and the
server (synchronously), but does not state a formal availability entry or a list of script types.

**Since:** 2015.2 (all three parameters as well).

Rules and behaviour:

- Any script that hands entry points to NetSuite must use `define`.
- In an entry point script the callback must return an object with at least one key, and each value
  must be a named entry point function. All entry points returned must belong to the same script
  type, and one script file holds one entry point script of one script type.
- An entry point script may load several of your custom modules as dependencies.
- SuiteScript API calls may only run after the callback has returned (inside the entry point
  functions or functions they call), not while the module is being defined. Likewise, while a custom
  module is being defined you cannot use the methods of native modules.
- Code passed to `define` cannot be stepped through in the SuiteScript Debugger. For on-demand
  debugging, use [`require`](#requiredependencies-callback) instead.

### require(dependencies, callback)
<!-- id: global:require -->

`require` loads modules that already exist, also following the AMD specification (asynchronous on
the client, synchronous on the server). Unlike `define`, a dependency is loaded only when it is first
used, and one that is never used is never loaded, which can make scripts faster. When you are
writing an entry point script or a custom module, use [`define`](#defineid-dependencies-moduleobject)
instead. Module resolution can be adjusted with the `@NAmdConfig` tag; see
[`require.configuration`](#requireconfiguration-object).

| Parameter | Type | Required | Default |
|---|---|---|---|
| `dependencies` | string[] | No | |
| `callback` | Function | Yes | |

- `dependencies`: SuiteScript modules like `N/query`, or File Cabinet paths to your own modules.
- `callback`: receives the modules; each dependency is loaded on first use.

**Returns:** `void`

**Errors:**

- `MODULE_DOES_NOT_EXIST`: a requested module cannot be found. As with `define`, only the first
  missing module is reported.

**Governance:** not documented; assumed to be free (0 units) by project convention.

**Availability:** behaviour is described for both client and server, but the source does not state a
formal availability entry or a list of script types.

**Since:** 2015.2 (both parameters as well).

### Iterator(callback)
<!-- id: global:Iterator -->

An iterator hands you the items of a collection one at a time instead of all at once. One place you
get one is `ResultSet.iterator()` in [N/query](modules/N_query.md). It is compatible with the
ECMAScript iterator protocol and can be used in two ways:

- `next()`: each call returns an object with a `value` (the current item) and a `done` flag. When
  `done` is `true`, iteration has finished.

> [!SOURCE-ERROR] The source's reference table for `next()` actually describes `each()`, so its
> description of `next()` cannot be trusted. Rely on the value-and-done behaviour above and confirm
> details in your account.
<!-- source-error: global:Iterator | - | iterator_next_reference_table_describes_each -->

- `each(callback)`: calls the callback once per item. Pick this form when you want to stop as soon
  as you meet a particular value.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `callback` | function | Yes | |

- `callback`: takes one argument. In the source's example that argument is a wrapper object with the
  current item in its `value` property. Return `true` to keep going, `false` to stop.

> [!SOURCE-ERROR] Here the `each` callback is documented with a single argument, but the iterators
> that map/reduce scripts expose (the `errors` of the map and reduce contexts, the summary's map
> `keys` and the summary `output`) are documented with callbacks that receive several arguments,
> such as a key plus error or execution details. The source does not reconcile the two; for those
> iterators follow the [MapReduceScript](script-types/MapReduceScript.md) page and check what your
> callback actually receives.
<!-- source-error: global:Iterator | - | each_callback_one_param_vs_mapreduce_multi_param_iterators -->

**Returns:** `void`

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2015.2

### toString()
<!-- id: global:toString -->

Use `toString` to find out what kind of native SuiteScript API object you are holding: it gives back
the object's type name. Use it instead of `instanceof`, which SuiteScript does not support for these
objects; trying `instanceof` throws an error saying something is undefined. The members of native
SuiteScript objects cannot be changed. The spec lists no parameters and does not show the exact call
form.

**Returns:** `string`, the object's type name.

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2015.2

## Objects

### JSON (object)
<!-- id: global:JSON -->

The standard object for turning JSON text into values and back, following RFC 4627. The source does
not state an availability entry or a `since` version for the object itself; both of its methods are
documented for client and server scripts since 2015.2.

#### JSON.parse(text, reviver)
<!-- id: global:JSON#parse -->

Parses JSON text and gives back the resulting object.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `text` | string | Yes | |
| `reviver` | Function | No | |

- `text`: must be valid JSON as defined by RFC 4627.
- `reviver`: a function that can transform the parsed value before it is returned.

**Returns:** `Object`, the value parsed from `text`.

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2015.2 (both parameters as well).

#### JSON.stringify(value, replacer, space)
<!-- id: global:JSON#stringify -->

Turns an object or other value into JSON text.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `value` | Object | Yes | |
| `replacer` | Function | No | |
| `space` | Object | No | |

- `value`: an object or any other value.
- `replacer`: a function that changes how values are converted.
- `space`: a string or a number that adds whitespace so the output is easier to read.

> [!SOURCE-ERROR] The source types `space` as `Object` while describing it as a string or a number.
> Pass a string or a number, as standard JSON behaviour expects, and treat the `Object` type as
> unreliable.
<!-- source-error: global:JSON#stringify | space | space_typed_object_but_described_string_or_number -->

**Returns:** `string`, the JSON text.

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2015.2 (all three parameters as well).

### log (object)
<!-- id: global:log -->

`log` is the same thing as the [N/log](modules/N_log.md) module and is loaded automatically in every
script type, so you can write log entries without listing `N/log`. Load the module explicitly anyway
when you are testing. It stands for these module members (see the N/log page):

- `log.debug(options)` (`N/log.debug`)
- `log.audit(options)` (`N/log.audit`)
- `log.emergency(options)` (`N/log.emergency`)
- `log.error(options)` (`N/log.error`)

`log` is a reserved word. If a SuiteScript 1.0 script and a SuiteScript 2.1 script run on the same
record and the 1.0 script uses `log` as a variable name, an error occurs. The fix is to rename that
variable in the 1.0 script.

The source does not state an availability entry or a `since` version for the global `log` object;
see the N/log page for its members.

### util (object)
<!-- id: global:util -->

`util` is the same thing as the [N/util](modules/N_util.md) module and is loaded automatically in
every script type. As with `log`, load the module explicitly when testing. It stands for these module
members (see the N/util page):

- `util.isArray` (`N/util.isArray`)
- `util.isBoolean` (`N/util.isBoolean`)
- `util.isDate` (`N/util.isDate`)
- `util.isFunction` (`N/util.isFunction`)
- `util.isNumber` (`N/util.isNumber`)
- `util.isObject` (`N/util.isObject`)
- `util.isRegExp` (`N/util.isRegExp`)
- `util.isString` (`N/util.isString`)
- `util.each` (`N/util.each`)
- `util.extend` (`N/util.extend`)

`util` is a reserved word too. A SuiteScript 1.0 script that uses `util` as a variable name causes an
error when it runs on the same record as a SuiteScript 2.1 script; rename the variable in the 1.0
script.

The source does not state an availability entry or a `since` version for the global `util` object.

### Promise (object)
<!-- id: global:Promise -->

A promise is a placeholder for the result of an asynchronous operation that has not finished yet. It
starts out `pending` and moves to either `fulfilled` or `rejected`; once settled it never changes
again. When the operation completes, the matching success or failure callback runs.

**Availability:**

- Client: available in all client scripts, where you can also create your own promises.
- Server: supported since 2021.1, in SuiteScript 2.1. The modules with server-side asynchronous
  support are [N/http](modules/N_http.md), [N/https](modules/N_https.md), [N/llm](modules/N_llm.md),
  [N/query](modules/N_query.md), [N/search](modules/N_search.md) and
  [N/transaction](modules/N_transaction.md). Using async/await or promises with other modules on the
  server produces errors.
- The source does not state a `since` version for the object as a whole beyond the 2021.1 server
  support.

Naming: the promise version of an API has the name of the synchronous API with `.promise` appended
(for example `email.send` and `email.send.promise`). Promises you build yourself do not go through
these promise APIs.

Which promise APIs run where:

- Client scripts only: the promise APIs of `N/action`, `N/currentRecord`, `N/email`, `N/record` and
  `N/dataset`, plus the promise methods of search pages and result sets.

> [!SOURCE-ERROR] The source lists the `N/dataset` promise APIs as client-only, yet describes the
> `N/dataset` module itself as server-only. The spec does not say which is right; test in the script
> type you intend to use before relying on either.
<!-- source-error: global:Promise | - | n_dataset_promises_listed_client_only_but_module_server_only -->

- Server scripts only: the promise APIs of `N/llm`, `N/documentCapture` and `N/machineTranslation`.

> [!SOURCE-ERROR] The source's table of server-side promise APIs names modules (such as
> `N/documentCapture` and `N/machineTranslation`) that are missing from its own list of modules with
> server-side asynchronous support. The spec does not resolve this; verify server-side async support
> for those modules before depending on it.
<!-- source-error: global:Promise | - | server_promise_table_lists_modules_missing_from_async_list -->

When to use them:

- On the server, promises suit separate operations within one process whose order does not matter.
- Avoid them for bulk processing; use a work queue for that instead.

Recommended practice:

- Always attach a `catch` handler, and use `.catch` rather than the second argument of `then`.
- Chain promises instead of nesting them, and keep chains short: long chains use too much memory and
  CPU.
- Put code that must run whatever the outcome in `finally`, and code that must wait for completion in
  `then`.
- Use `Promise.all` for unrelated asynchronous calls, spread the parallel results, and use a
  map-style helper to limit how many run at once.
- In SuiteScript 2.1, prefer `async`/`await` to writing `Promise` chains by hand. When you use
  async/await: rewrite existing promise code into that style, start asynchronous work first and
  `await` it later, do not mix callback-style and promise APIs, and do not write `return await`.

The global `Promise` relates to the promise APIs of these modules (each module page lists its
`.promise` members):

- [N/action](modules/N_action.md)
- [N/currentRecord](modules/N_currentRecord.md)
- [N/dataset](modules/N_dataset.md)
- [N/documentCapture](modules/N_documentCapture.md)
- [N/email](modules/N_email.md)
- [N/http](modules/N_http.md)
- [N/https](modules/N_https.md)
- [N/llm](modules/N_llm.md)
- [N/machineTranslation](modules/N_machineTranslation.md)
- [N/query](modules/N_query.md)
- [N/record](modules/N_record.md)
- [N/search](modules/N_search.md)
- [N/transaction](modules/N_transaction.md)

### require.configuration (object)
<!-- id: global:require.configuration -->

An optional configuration, meant for advanced users, that controls how custom modules are found and
loaded (it follows the amdjs CommonConfig specification, revision fd45c71). Its settings take effect
before any dependency is loaded.

- The preferred way to supply it is the `@NAmdConfig` tag, pointing at a configuration file that
  lives in the File Cabinet. That file uses JSON syntax, not JavaScript.
- Existing scripts may instead call `require.config(context)`; this still works but is not the
  preferred route. The value passed there is the configuration itself, not a file path.
- Because the configuration is per script, each client script can have its own, while many scripts
  can also share one file.
- The source does not state an availability entry or a `since` version for this object.

| Property | Type | What it is for |
|---|---|---|
| `baseUrl` | string | A root folder, so that relative module paths can be shorter |
| `config` | Object | Attaching extra attributes, such as metadata, to modules |
| `map` | Object | Aliasing a module, giving one module several names, or telling apart modules that share a name (for example different versions) |
| `packages` | Object[] | Using a CommonJS package as a custom module |
| `paths` | Object | Named path aliases, swapping in a mock module for testing, or giving a SuiteScript module a custom name |
| `shim` | Object | Loading a JavaScript library that is not written as an AMD module |

## Examples

A scheduled script that walks vendor balances with a query iterator, calling `next()` until `done`
is `true`, and logs a compact JSON summary. A replacer rounds every number in the output. No
SuiteScript call happens until the entry point runs. (Table and column names in SuiteQL come from
the account's Records Catalog.)

```js
/**
 * @NApiVersion 2.1
 * @NScriptType ScheduledScript
 */
define(['N/query', 'N/log'], (query, log) => {
  const roundNumbers = (key, value) =>
    typeof value === 'number' ? Math.round(value * 100) / 100 : value;

  const execute = () => {
    const resultSet = query.runSuiteQL({
      query: 'SELECT id, entityid, balance FROM vendor WHERE isinactive = ?',
      params: ['F']
    });

    const rows = resultSet.iterator();
    const owing = [];
    let step = rows.next();
    while (!step.done) {
      const [id, name, balance] = step.value.values;
      if (balance > 0) {
        owing.push({ id, name, balance });
      }
      step = rows.next();
    }

    const total = owing.reduce((sum, v) => sum + v.balance, 0);
    log.audit({
      title: `Vendors with open balances: ${owing.length}`,
      details: JSON.stringify({ total, vendors: owing.slice(0, 20) }, roundNumbers, 2)
    });
  };

  return { execute };
});
```

A custom module with no dependencies, written in the `define(moduleObject)` form. It uses
`JSON.parse` with a reviver to turn date strings in a stored settings blob into `Date` objects.
Another script can list this file as a dependency.

```js
/**
 * @NApiVersion 2.1
 */
define(() => {
  const DATE_KEYS = ['effectiveFrom', 'effectiveTo'];

  const reviveDates = (key, value) =>
    DATE_KEYS.includes(key) && typeof value === 'string' ? new Date(value) : value;

  const readFreightRules = (jsonText) => {
    const rules = JSON.parse(jsonText, reviveDates);
    return rules.filter((rule) => !rule.effectiveTo || rule.effectiveTo > new Date());
  };

  return { readFreightRules };
});
```

A client script that loads `N/query` and `N/ui/dialog` with `require` only when the vendor on a bill
actually changes, so pages where nobody touches that field never pay for them. The promise returned
by the alert gets a `catch` handler.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType ClientScript
 */
define([], () => {
  const fieldChanged = (context) => {
    if (context.fieldId !== 'entity') {
      return;
    }
    const vendorId = context.currentRecord.getValue({ fieldId: 'entity' });
    if (!vendorId) {
      return;
    }

    require(['N/query', 'N/ui/dialog'], (query, dialog) => {
      const rows = query.runSuiteQL({
        query: 'SELECT balance FROM vendor WHERE id = ?',
        params: [Number(vendorId)]
      }).asMappedResults();

      const balance = rows.length ? rows[0].balance : 0;
      if (balance > 50000) {
        dialog
          .alert({
            title: 'High open balance',
            message: `This vendor already has ${balance} outstanding.`
          })
          .catch((err) => console.error(err));
      }
    });
  };

  return { fieldChanged };
});
```

A one-off snippet for the debugger, where `require` is the right tool because code inside `define`
cannot be stepped through:

```js
require(['N/query'], (query) => {
  const inactive = query.runSuiteQL({
    query: 'SELECT COUNT(*) AS cnt FROM vendor WHERE isinactive = ?',
    params: ['T']
  }).asMappedResults();
  inactive.forEach((row) => console.log(`Inactive vendors: ${row.cnt}`));
});
```
