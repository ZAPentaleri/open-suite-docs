# N/suiteAppInfo
<!-- id: N/suiteAppInfo -->

`N/suiteAppInfo` reports on what is installed in the account: which bundles and which SDF SuiteApps
are present, and which of them contain particular scripts. Typical uses are a script that enables an
integration only when a companion app is installed, or a diagnostics page showing where scripts came
from. It is supported in every script type.

- **Availability:** client and server scripts; all script types.
- **Load it as:** `define(['N/suiteAppInfo'], (suiteAppInfo) => { ... })`
- **Since:** 2021.1

## Members

| Member | Kind | Summary |
|---|---|---|
| [`suiteAppInfo.isBundleInstalled(options)`](#suiteappinfoisbundleinstalledoptions) | function | Is a bundle installed? |
| [`suiteAppInfo.listInstalledBundles()`](#suiteappinfolistinstalledbundles) | function | Details of every installed bundle |
| [`suiteAppInfo.listBundlesContainingScripts(options)`](#suiteappinfolistbundlescontainingscriptsoptions) | function | Which bundles contain given scripts |
| [`suiteAppInfo.isSuiteAppInstalled(options)`](#suiteappinfoissuiteappinstalledoptions) | function | Is a SuiteApp installed? |
| [`suiteAppInfo.listInstalledSuiteApps()`](#suiteappinfolistinstalledsuiteapps) | function | Details of every installed SuiteApp |
| [`suiteAppInfo.listSuiteAppsContainingScripts(options)`](#suiteappinfolistsuiteappscontainingscriptsoptions) | function | Which SuiteApp contains each given script |

## Functions

### suiteAppInfo.isBundleInstalled(options)
<!-- id: N/suiteAppInfo.isBundleInstalled -->

Checks whether a bundle is installed in the account.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.bundleId` | integer | Yes | |

**Returns:** `boolean`, `true` when the bundle is installed.

**Governance:** 5 units.

**Availability:** client and server scripts.

**Since:** 2021.1

### suiteAppInfo.listInstalledBundles()
<!-- id: N/suiteAppInfo.listInstalledBundles -->

Lists every bundle installed in the account. Takes no parameters.

**Returns:** `Object[]`, one object per bundle with these fields:

| Field | Type |
|---|---|
| `id` | integer |
| `name` | string |
| `version` | string |
| `description` | string |
| `installedFrom` | string |
| `isManaged` | boolean |
| `dateInstalled` | Date |
| `dateLastUpdated` | Date |
| `publisher` | object with an integer `id` and a string `name` |
| `installedBy` | object with an integer `id` and a string `name` |

**Governance:** 10 units.

**Availability:** client and server scripts.

**Since:** 2021.1

### suiteAppInfo.listBundlesContainingScripts(options)
<!-- id: N/suiteAppInfo.listBundlesContainingScripts -->

For each script ID you pass, tells you which bundles contain that script.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.scriptsIds` | string[] | Yes | |

- `options.scriptsIds` [sic]: the script IDs to look up.

> [!SOURCE-ERROR] The parameter table of the source names this option `scriptsIds`, while its syntax
> sample writes `scriptIds`. The spec keeps `scriptsIds` without saying which spelling the API
> accepts; test before relying on either.
<!-- source-error: N/suiteAppInfo.listBundlesContainingScripts | options.scriptsIds | param_table_scriptsIds_vs_syntax_sample_scriptIds -->

**Returns:** `Object` that maps each script ID to an array of integer bundle IDs.

**Governance:** 10 units.

**Availability:** client and server scripts.

**Since:** 2021.1

### suiteAppInfo.isSuiteAppInstalled(options)
<!-- id: N/suiteAppInfo.isSuiteAppInstalled -->

Checks whether an SDF SuiteApp is installed in the account.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.suiteAppId` | string | Yes | |

**Returns:** `boolean`, `true` when the SuiteApp is installed.

**Governance:** 5 units.

**Availability:** client and server scripts.

**Since:** 2021.1

### suiteAppInfo.listInstalledSuiteApps()
<!-- id: N/suiteAppInfo.listInstalledSuiteApps -->

Lists every SuiteApp installed in the account. Takes no parameters.

**Returns:** `Object[]`, one object per SuiteApp with these fields:

| Field | Type |
|---|---|
| `appId` | string |
| `publisherId` | string |
| `name` | string |
| `version` | string |
| `description` | string |
| `dateInstalled` | Date |
| `dateLastUpdated` | Date |
| `installedBy` | object with an integer `id` and a string `name` |

**Governance:** 10 units.

**Availability:** client and server scripts.

**Since:** 2021.1

### suiteAppInfo.listSuiteAppsContainingScripts(options)
<!-- id: N/suiteAppInfo.listSuiteAppsContainingScripts -->

For each script ID you pass, tells you which SuiteApp contains it. Unlike the bundle variant, each
script maps to exactly one value: a SuiteApp ID, or `null` when no SuiteApp contains the script.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.scriptsIds` | string[] | Yes | |

- `options.scriptsIds`: the script IDs to look up (spelled as the spec gives it).

**Returns:** `Object` that maps each script ID to one SuiteApp ID or `null`.

**Governance:** 10 units.

**Availability:** client and server scripts.

**Since:** 2021.1

## Examples

A user event script that only queues a carrier sync when the carrier SuiteApp is installed, so one
script can be deployed to accounts with and without it. The SuiteApp ID comes from a script
parameter.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType UserEventScript
 */
define(['N/suiteAppInfo', 'N/runtime', 'N/log'], (suiteAppInfo, runtime, log) => {
  const afterSubmit = (context) => {
    const appId = runtime.getCurrentScript().getParameter({ name: 'custscript_carrier_app_id' });
    if (!appId || !suiteAppInfo.isSuiteAppInstalled({ suiteAppId: appId })) {
      log.debug({ title: 'Carrier sync skipped', details: `${appId || 'no app id'} not installed` });
      return;
    }
    log.audit({ title: 'Carrier sync', details: `Fulfillment ${context.newRecord.id} queued` });
  };

  return { afterSubmit };
});
```

A Suitelet that lists managed bundles updated in the last 90 days, with who installed them:

```js
/**
 * @NApiVersion 2.1
 * @NScriptType Suitelet
 */
define(['N/suiteAppInfo'], (suiteAppInfo) => {
  const onRequest = (context) => {
    const cutoff = Date.now() - 90 * 24 * 60 * 60 * 1000;
    const recent = suiteAppInfo.listInstalledBundles()
      .filter((b) => b.isManaged && new Date(b.dateLastUpdated).getTime() >= cutoff)
      .map((b) => `${b.id} ${b.name} v${b.version} (installed by ${b.installedBy.name})`);
    context.response.write({ output: recent.length ? recent.join('\n') : 'No recent managed bundles' });
  };

  return { onRequest };
});
```

A Suitelet that reports where a set of scripts came from:

```js
/**
 * @NApiVersion 2.1
 * @NScriptType Suitelet
 */
define(['N/suiteAppInfo'], (suiteAppInfo) => {
  const onRequest = (context) => {
    const scriptsIds = ['customscript_po_router', 'customscript_bin_balancer']; // [sic] option name
    const viaApps = suiteAppInfo.listSuiteAppsContainingScripts({ scriptsIds });
    const viaBundles = suiteAppInfo.listBundlesContainingScripts({ scriptsIds });

    const lines = scriptsIds.map((id) =>
      `${id}: SuiteApp=${viaApps[id] || 'none'}, bundles=${JSON.stringify(viaBundles[id] || [])}`);
    context.response.write({ output: lines.join('\n') });
  };

  return { onRequest };
});
```
