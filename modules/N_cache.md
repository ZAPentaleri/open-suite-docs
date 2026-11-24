# N/cache
<!-- id: N/cache -->

`N/cache` keeps short-lived string values in memory on the server so a script does not have to fetch
the same data over and over (for example, a lookup that many executions repeat). Values live in a
named cache whose visibility is set by a scope: only the current script, the server scripts in the
same bundle, or every server script in the account. A value stays until the time-to-live given when
it was stored runs out, although the cache can drop it earlier.

- **Availability:** server scripts only.
- **Load it as:** `define(['N/cache'], (cache) => { ... })`
- Related: [Governance](../topics/governance.md), [N/runtime](N_runtime.md).

## Members

| Member | Kind | Summary |
|---|---|---|
| [`cache.getCache(options)`](#cachegetcacheoptions) | function | Get a named cache, creating it if needed |
| [`Cache`](#cache-object) | object | A named cache with `get`, `put` and `remove` |
| [`cache.Scope`](#cachescope) | enum | Who can see a cache |

## Functions

### cache.getCache(options)
<!-- id: N/cache.getCache -->

Returns the cache with the given name. If no cache of that name exists yet in the requested scope,
one is created and returned, so you never need a separate "create" step.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.name` | string | Yes | |
| `options.scope` | string | No | `PRIVATE` |

- `options.name` can be at most 1 kilobyte long.
- `options.scope` takes a value from [`cache.Scope`](#cachescope).

**Returns:** `cache.Cache`, either the existing cache or a newly created one.

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2016.2

## Objects

### Cache (object)
<!-- id: N/cache.Cache -->

A handle to one named cache, obtained from [`cache.getCache(options)`](#cachegetcacheoptions).
Available in server scripts since 2016.2.

#### Cache.get(options)
<!-- id: N/cache.Cache#get -->

Reads the value stored under a key. The intended way to fill a cache is to pass a `loader`: when
the key has no value, the loader is called, and whatever it returns is stored in the cache for you
and handed back. You can make the loader the only way values ever enter the cache, and that is the
recommended pattern rather than calling `put` yourself.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.key` | string | Yes | |
| `options.loader` | string \| function | No | |
| `options.ttl` | number | No | `no_limit` |

- `options.key`: up to 4096 bytes; must not be null. A typical key identifies the thing being
  looked up, such as a postal code whose city name you want.
- `options.loader`: a function, or the name of a function. It is called with an object carrying the
  key. Its result is cached automatically; a non-string result is converted with
  `JSON.stringify` first. The cached value may be at most 500 KB. Without a loader, a miss simply
  returns `null`. Because entries expire, the loader can run again for the same key once the TTL
  has passed.
- `options.ttl`: time to live in seconds, minimum 300. The default is no limit: if you leave it out,
  the entry has no TTL, but the cache may still evict the value earlier than any TTL you set.

**Returns:** `String | null`, the cached (or freshly loaded) string, or `null` when nothing is cached
and no loader was given.

**Governance:** 1 unit when the value is already cached; 2 units when the loader has to run.

**Availability:** server scripts.

**Since:** 2016.2

#### Cache.put(options)
<!-- id: N/cache.Cache#put -->

Stores a value under a key. Values that are not strings are converted with `JSON.stringify`.
The cache is not persistent storage, so never treat a `put` as a guarantee the value will be
there later; prefer `get` with a loader, which repopulates the entry whenever it is missing.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.key` | string | Yes | |
| `options.value` | string | Yes | |
| `options.ttl` | number | No | |

- `options.key`: up to 4096 bytes; must not be null. For example, a postal code used as the key
  for the city names stored under it.
- `options.value`: at most 500 KB; non-strings are JSON-stringified.
- `options.ttl`: seconds, minimum 300; by default there is no maximum. The value may be evicted
  before the TTL ends.

**Returns:** `void`

**Governance:** 1 unit.

**Availability:** server scripts.

**Since:** 2016.2

#### Cache.remove(options)
<!-- id: N/cache.Cache#remove -->

Deletes the value stored under a key.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.key` | string | Yes | |

**Returns:** `void`

**Governance:** 1 unit.

**Availability:** server scripts.

**Since:** 2016.2

#### Cache.name
<!-- id: N/cache.Cache#name -->

`string`. The name the cache was created with. Since 2016.2.

#### Cache.scope
<!-- id: N/cache.Cache#scope -->

`cache.Scope`. The scope of this cache, one of the [`cache.Scope`](#cachescope) values. Since 2016.2.

## Enums

### cache.Scope
<!-- id: N/cache.Scope -->

Controls which scripts share a cache. Used by `options.scope` of `getCache` and by `Cache.scope`.
Since 2016.2.

| Value | Who can read and write the cache |
|---|---|
| `PRIVATE` | Only the script that created it (the default for `getCache`) |
| `PROTECTED` | Scripts in the same bundle; for scripts not in any bundle, all unbundled scripts |
| `PUBLIC` | Every server script in the account |

## Examples

A RESTlet that returns a location's display name, caching the lookup for an hour. The loader runs
only on a miss, so repeated calls cost 1 unit each instead of the lookup's cost.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType Restlet
 */
define(['N/cache', 'N/search'], (cache, search) => {
  const locationNames = cache.getCache({
    name: 'warehouse_names',
    scope: cache.Scope.PROTECTED
  });

  const loadLocationName = (context) => {
    const fields = search.lookupFields({
      type: search.Type.LOCATION,
      id: context.key,
      columns: ['name']
    });
    return fields.name;
  };

  const get = (requestParams) => {
    const name = locationNames.get({
      key: String(requestParams.locationId),
      loader: loadLocationName,
      ttl: 3600
    });
    return { locationId: requestParams.locationId, name };
  };

  return { get };
});
```

Clearing an entry after the underlying data changes, so the next `get` reloads it:

```js
/**
 * @NApiVersion 2.1
 * @NScriptType UserEventScript
 */
define(['N/cache'], (cache) => {
  const afterSubmit = (context) => {
    const names = cache.getCache({ name: 'warehouse_names', scope: cache.Scope.PROTECTED });
    names.remove({ key: String(context.newRecord.id) });
  };

  return { afterSubmit };
});
```
