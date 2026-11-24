# N/runtime
<!-- id: N/runtime -->

`N/runtime` lets a script inspect the situation it is running in. You can ask which script and
deployment is executing, how many usage units remain, what its parameters hold, who the current user
is (role, permissions, preferences), how the script was triggered, which account, release and
environment it runs in, and whether a given feature is switched on. Server scripts also get a small
per-session key/value store.

- **Availability:** client and server scripts, with exceptions: `Session.get` / `Session.set`,
  `Script.deploymentId` and `Script.logLevel` are for server scripts only.
- `Script.percentComplete` is the value shown as the percent complete of a scheduled script on its
  status page.
- An example of a release value this module reports is `2023.2`.
- **Load it as:** `define(['N/runtime'], (runtime) => { ... })`
- Related: [N/log](N_log.md), [N/config](N_config.md),
  [features catalog](../gen/catalogs/features.md), [permissions catalog](../gen/catalogs/permissions.md),
  [preferences catalog](../gen/catalogs/preferences.md).

## Members

| Member | Kind | Summary |
|---|---|---|
| [`runtime.getCurrentScript()`](#runtimegetcurrentscript) | function | The running script and its deployment |
| [`runtime.getCurrentUser()`](#runtimegetcurrentuser) | function | The user the script runs for |
| [`runtime.getCurrentSession()`](#runtimegetcurrentsession) | function | The current user session |
| [`runtime.isFeatureInEffect(options)`](#runtimeisfeatureineffectoptions) | function | Is a feature enabled in this account? |
| [`runtime.isNextActive()`](#runtimeisnextactive) | function | Is NetSuite Next active for the running user? |
| [`Script`](#script-object) | object | Script and deployment settings, parameters, remaining usage |
| [`User`](#user-object) | object | User properties, permissions, preferences |
| [`Session`](#session-object) | object | Your own key/value data for the session |
| [`runtime.ContextType`](#runtimecontexttype) | enum | Execution contexts |
| [`runtime.EnvType`](#runtimeenvtype) | enum | Account environment types |
| [`runtime.Permission`](#runtimepermission) | enum | Permission levels |
| `runtime.accountId`, `country`, `envType`, `executionContext`, `processorCount`, `queueCount`, `version` | module properties | See [Module properties](#module-properties) |

## Functions

### runtime.getCurrentScript()
<!-- id: N/runtime.getCurrentScript -->

Gives you a [`Script`](#script-object) object describing the script that is running and its
deployment: identifiers, log level, parameter values and remaining usage. When what you need concerns
the session or the user rather than the script, call `getCurrentSession` or `getCurrentUser` instead.

No parameters.

**Returns:** `runtime.Script`

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2015.2

### runtime.getCurrentUser()
<!-- id: N/runtime.getCurrentUser -->

Gives you a [`User`](#user-object) object carrying the properties and preferences of the user the
script is executing for.

No parameters.

**Returns:** `runtime.User`

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2015.2

### runtime.getCurrentSession()
<!-- id: N/runtime.getCurrentSession -->

Gives you the [`Session`](#session-object) object of the current user session, where you can keep
your own values. For properties of the script or the user, use `getCurrentScript` or
`getCurrentUser` instead.

No parameters.

**Returns:** `runtime.Session`

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2015.2

In client scripts, do not count on session data staying put: it can change after the browser
restarts or after the session times out.

### runtime.isFeatureInEffect(options)
<!-- id: N/runtime.isFeatureInEffect -->

Answers whether a feature is enabled in the account. It covers the features that appear on the
account's Enable Features page.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.feature` | string | Yes | |

- `options.feature`: a feature ID from the [features catalog](../gen/catalogs/features.md). Since
  2015.2.

**Returns:** `boolean`, `true` when the feature is enabled in the account.

**Errors:**

- `SSS_MISSING_REQD_ARGUMENT`: `options.feature` was not supplied.
- `WRONG_PARAMETER_TYPE`: `options.feature` is not a string.

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2015.2

### runtime.isNextActive()
<!-- id: N/runtime.isNextActive -->

Reports whether NetSuite Next is active for the user who is running the script. In scheduled and
map/reduce scripts, the answer follows whichever user the script runs as; a script running as the
system user always gets `false`.

No parameters.

**Returns:** `boolean`, `true` when NetSuite Next is active for the running user.

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2026.1

## Objects

### Script (object)
<!-- id: N/runtime.Script -->

The runtime settings of the executing script, obtained from
[`runtime.getCurrentScript()`](#runtimegetcurrentscript). Client and server scripts; since 2015.2.

#### Script.getParameter(options)
<!-- id: N/runtime.Script#getParameter -->

Reads the value of one of the script's parameters.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.name` | string | Yes | |

- `options.name`: the name (ID) of the script parameter. Since 2015.2.

**Returns:** `number | Date | string | boolean | null | undefined`: the parameter's value, or `null`
or `undefined` when it has none.

**Errors:**

- `SSS_MISSING_REQD_ARGUMENT`: `options.name` was not supplied.
- `WRONG_PARAMETER_TYPE`: `options.name` has the wrong type.

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2015.2

#### Script.getRemainingUsage()
<!-- id: N/runtime.Script#getRemainingUsage -->

Tells you how many governance units the executing script still has available.

No parameters.

**Returns:** `number`, the units left.

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2015.2

#### Script.id
<!-- id: N/runtime.Script#id -->

`string`, read-only. The script ID of the executing script. Since 2015.2.

#### Script.deploymentId
<!-- id: N/runtime.Script#deploymentId -->

`string`, read-only. The ID of the deployment that is executing. Server scripts only. Since 2015.2.

#### Script.apiVersion
<!-- id: N/runtime.Script#apiVersion -->

`string`, read-only. The SuiteScript version the script runs under. Since 2015.2.

#### Script.logLevel
<!-- id: N/runtime.Script#logLevel -->

`string`, read-only. The deployment's log level: one of `DEBUG`, `AUDIT`, `ERROR` or `EMERGENCY` (see
[N/log](N_log.md) for which entries each level records). Not supported in client scripts; server
scripts only. Since 2015.2.

#### Script.percentComplete
<!-- id: N/runtime.Script#percentComplete -->

`number`, settable. How far a scheduled script has progressed, as a percentage. The value you set is
what the [scheduled script status page](../topics/scheduled_script.status_page.md) shows as percent
complete. It is available only in scheduled scripts; touching it from any other script type raises
`SSS_OPERATION_UNAVAILABLE`. Since 2015.2.

#### Script.bundleIds
<!-- id: N/runtime.Script#bundleIds -->

`string[]`, read-only. The IDs of the bundles that contain the script. The array may hold several IDs,
which is relevant to bundle installation scripts. Deprecated bundles are not included, and the IDs are
in ascending order, so the last element is not necessarily the bundle whose installation triggered
the script. Since 2015.2.

### User (object)
<!-- id: N/runtime.User -->

The properties and preferences of the user the script executes for, obtained from
[`runtime.getCurrentUser()`](#runtimegetcurrentuser). Client and server scripts; since 2015.2.

#### User.getPermission(options)
<!-- id: N/runtime.User#getPermission -->

Returns the user's level for one permission, as a [`runtime.Permission`](#runtimepermission) value.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.name` | string | Yes | |

- `options.name`: a permission ID from the [permissions catalog](../gen/catalogs/permissions.md).
  Since 2015.2.

**Returns:** `string`, the permission level.

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2015.2

#### User.getPreference(options)
<!-- id: N/runtime.User#getPreference -->

Returns the value of one user preference. The preferences it can read are the general ones (seen in
the UI under Setup > Company > General Preferences) and the accounting ones (Setup > Accounting >
Accounting Preferences). It only reads. To change a preference, load the `COMPANY_PREFERENCES` or
`ACCOUNTING_PREFERENCES` configuration with [`config.load`](N_config.md) and change the value with
[`Record.setValue`](N_record.md).

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.name` | string | Yes | |

- `options.name`: a preference ID from the [preferences catalog](../gen/catalogs/preferences.md).
  Since 2015.2.

**Returns:** `string`, the preference value.

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2015.2

> [!SOURCE-ERROR] The source's description of this method includes a note about the `FULL`
> permission level, which belongs to `User.getPermission` and says nothing about preferences.
<!-- source-error: N/runtime.User#getPreference | - | note_about_permission_full_belongs_to_getpermission -->

#### User.id
<!-- id: N/runtime.User#id -->

`number`, read-only. Internal ID of the current user. Since 2019.2.

#### User.name
<!-- id: N/runtime.User#name -->

`string`, read-only. Name of the current user. Since 2015.2.

#### User.email
<!-- id: N/runtime.User#email -->

`string`, read-only. Email address of the current user; it is only available when the user's employee
record has an email address. It can also be used to get the email of a web store shopper who is
recognized but not logged in. Since 2015.2.

#### User.contact
<!-- id: N/runtime.User#contact -->

`number`, read-only. Internal ID of the logged-in contact. It is `0` when no entity is logged in or
when the logged-in entity is not a contact. Since 2019.1.

#### User.role
<!-- id: N/runtime.User#role -->

`number`, read-only. Internal ID of the user's role. Since 2015.2.

#### User.roleId
<!-- id: N/runtime.User#roleId -->

`string`, read-only. The custom script ID of the user's role. Use it rather than
[`User.role`](#userrole) for a custom role that ships in a bundle: the internal ID of such a role can
change once the bundle is installed. Since 2015.2.

#### User.roleCenter
<!-- id: N/runtime.User#roleCenter -->

`string`, read-only. The center type of the user's role. A center is the role-based set of tabbed
pages a user works in. Since 2015.2.

#### User.department
<!-- id: N/runtime.User#department -->

`number`, read-only. Internal ID of the user's department. Since 2015.2.

#### User.location
<!-- id: N/runtime.User#location -->

`number`, read-only. Internal ID of the user's location. Since 2015.2.

#### User.subsidiary
<!-- id: N/runtime.User#subsidiary -->

`number`, read-only. Internal ID of the user's subsidiary. Since 2015.2.

### Session (object)
<!-- id: N/runtime.Session -->

A place for values you define yourself that should last for the current user session, useful for
tracking user-related state between requests: for instance the scope a user picked, a budget figure,
or a list of business problems they raised. Write with `set` and read with `get`. Obtained from
[`runtime.getCurrentSession()`](#runtimegetcurrentsession). Server scripts; since 2015.2.

#### Session.get(options)
<!-- id: N/runtime.Session#get -->

Returns the value stored under a key, or `null` when the key holds nothing. On the server, values are
stored with [`Session.set`](#sessionsetoptions).

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.name` | string | Yes | |

- `options.name`: the session key. Since 2015.2.

**Returns:** `string | null`

**Errors:**

- `SSS_MISSING_REQD_ARGUMENT`: `options.name` was not supplied.

**Governance:** 0 units.

**Availability:** listed for client and server scripts (see the note below).

**Since:** 2015.2

> [!SOURCE-ERROR] The source lists this method for both client and server scripts, while it presents
> the `Session` object and session get/set as server-only. It does not settle which is right; treat
> `Session.get` as server-only unless you have tested it in a client script.
<!-- source-error: N/runtime.Session#get | - | session_object_server_only_vs_get_both_sides -->

#### Session.set(options)
<!-- id: N/runtime.Session#set -->

Stores a key/value pair in the user's session. Read it back with [`Session.get`](#sessiongetoptions).

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.name` | string | Yes | |
| `options.value` | string | Yes | |

- `options.name`: the session key. Since 2015.2.
- `options.value`: the value to store, as a string. Since 2015.2.

**Returns:** `void`

**Errors:**

- `SSS_MISSING_REQD_ARGUMENT`: `options.name` or `options.value` was not supplied.

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2015.2

## Enums

### runtime.ContextType
<!-- id: N/runtime.ContextType -->

How a script was triggered; the set of values [`runtime.executionContext`](#runtimeexecutioncontext)
can take. Watch out: for several members the key and the string value differ, because the string
drops the underscores (the key `MAP_REDUCE` holds `'MAPREDUCE'`). Compare against the enum member,
not a typed-in string. Since 2015.2.

| Key | String value |
|---|---|
| `ACTION` | `ACTION` |
| `ADVANCEDREVREC` | `ADVANCEDREVREC` |
| `BANKCONNECTIVITY` | `BANKCONNECTIVITY` |
| `BANKSTATEMENTPARSER` | `BANKSTATEMENTPARSER` |
| `BUNDLE_INSTALLATION` | `BUNDLEINSTALLATION` |
| `CLIENT` | `CLIENT` |
| `CONSOLRATEADJUSTOR` | `CONSOLRATEADJUSTOR` |
| `CSV_IMPORT` | `CSVIMPORT` |
| `CUSTOMGLLINES` | `CUSTOMGLLINES` |
| `CUSTOM_MASSUPDATE` | `CUSTOMMASSUPDATE` |
| `DATASETBUILDER` | `DATASETBUILDER` |
| `DEBUGGER` | `DEBUGGER` |
| `EMAIL_CAPTURE` | `EMAILCAPTURE` |
| `FICONNECTIVITY` | `FICONNECTIVITY` |
| `FIPARSER` | `FIPARSER` |
| `MAP_REDUCE` | `MAPREDUCE` |
| `NONE` | `NONE` |
| `OCRPLUGIN` | `OCRPLUGIN` |
| `PAYMENTGATEWAY` | `PAYMENTGATEWAY` |
| `PAYMENTPOSTBACK` | `PAYMENTPOSTBACK` |
| `PLATFORMEXTENSION` | `PLATFORMEXTENSION` |
| `PORTLET` | `PORTLET` |
| `PROMOTIONS` | `PROMOTIONS` |
| `RECORDACTION` | `RECORDACTION` |
| `RESTLET` | `RESTLET` |
| `REST_WEBSERVICES` | `RESTWEBSERVICES` |
| `SCHEDULED` | `SCHEDULED` |
| `SDF_INSTALLATION` | `SDFINSTALLATION` |
| `SHIPPING_PARTNERS` | `SHIPPINGPARTNERS` |
| `SUITELET` | `SUITELET` |
| `TAX_CALCULATION` | `TAXCALCULATION` |
| `USEREVENT` | `USEREVENT` |
| `USER_INTERFACE` | `USERINTERFACE` |
| `WEBAPPLICATION` | `WEBAPPLICATION` |
| `WEBSERVICES` | `WEBSERVICES` |
| `WEBSTORE` | `WEBSTORE` |
| `WORKBOOKBUILDER` | `WORKBOOKBUILDER` |
| `WORKFLOW` | `WORKFLOW` |

### runtime.EnvType
<!-- id: N/runtime.EnvType -->

Kinds of account environment; the values [`runtime.envType`](#runtimeenvtype-1) can take. The spec
does not give string values separately from the names. Since 2015.2.

| Value |
|---|
| `BETA` |
| `INTERNAL` |
| `PRODUCTION` |
| `SANDBOX` |

### runtime.Permission
<!-- id: N/runtime.Permission -->

Permission levels, as returned by [`User.getPermission`](#usergetpermissionoptions), listed here in
the order the spec gives them, from most access to none. Since 2015.2.

| Value |
|---|
| `FULL` |
| `EDIT` |
| `CREATE` |
| `VIEW` |
| `NONE` |

## Module properties

### runtime.accountId
<!-- id: N/runtime.accountId -->

`string`, read-only. The account ID of the current user's account. Since 2015.2.

### runtime.country
<!-- id: N/runtime.country -->

`string`, read-only. The country of the current company. Since 2020.2.

### runtime.envType
<!-- id: N/runtime.envType -->

`string`, read-only. The type of environment the script runs in, as one of the
[`runtime.EnvType`](#runtimeenvtype) values. Since 2015.2.

### runtime.executionContext
<!-- id: N/runtime.executionContext -->

`string`, read-only. What triggered the current script, as a [`runtime.ContextType`](#runtimecontexttype)
value. Typical use: branch or return early depending on the trigger, for instance skipping work for
saves that come in through REST web services. Deployments can also be restricted to chosen execution
contexts without code; see [Script deployment](../topics/script_deployment.md). Since 2015.2.

### runtime.processorCount
<!-- id: N/runtime.processorCount -->

`number`, read-only. How many processors the account has, for example when a SuiteApp needs the
total. For scheduled script deployments that still use queues, read
[`runtime.queueCount`](#runtimequeuecount) instead; the two numbers can differ. Changing the queue
settings of deployments does not affect this value. Since 2018.1.

### runtime.queueCount
<!-- id: N/runtime.queueCount -->

`number`, read-only. How many scheduled script queues the account has. For map/reduce deployments,
read [`runtime.processorCount`](#runtimeprocessorcount) instead. Deployments dropping their queues
does not affect this value. Since 2015.2.

### runtime.version
<!-- id: N/runtime.version -->

`string`, read-only. The NetSuite release the account is on, for example `2023.2`. One use is
checking the release before a bundle is installed. Since 2015.2.

## Examples

**Leaving integration traffic alone in a user event.** Saves coming from a CSV import or REST web
services pass through untouched; interactive saves get a memo naming the user and role.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType UserEventScript
 */
define(['N/runtime', 'N/log'], (runtime, log) => {
  const passThrough = [runtime.ContextType.CSV_IMPORT, runtime.ContextType.REST_WEBSERVICES];

  const beforeSubmit = (context) => {
    if (passThrough.includes(runtime.executionContext)) {
      return;
    }
    const user = runtime.getCurrentUser();
    context.newRecord.setValue({ fieldId: 'memo', value: `Edited by ${user.name} as ${user.roleId}` });
    log.debug({ title: 'Memo stamped', details: `trigger ${runtime.executionContext}` });
  };

  return { beforeSubmit };
});
```

**Reporting progress and stopping before usage runs out.** A scheduled script takes its batch size
from a parameter, keeps the status page current, and stops while it still has a reserve of units.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType ScheduledScript
 */
define(['N/runtime', 'N/log'], (runtime, log) => {
  const execute = () => {
    const script = runtime.getCurrentScript();
    const batchSize = Number(script.getParameter({ name: 'custscript_reprice_batch' })) || 25;

    for (let done = 1; done <= batchSize; done += 1) {
      // ... reprice one item here ...
      script.percentComplete = Math.round((done / batchSize) * 100);
      if (script.getRemainingUsage() < 100) {
        log.audit({ title: 'Stopping early', details: `${done} of ${batchSize} done in ${script.deploymentId}` });
        break;
      }
    }
  };

  return { execute };
});
```

**Gating a Suitelet on permission, feature and environment.** Only users with full or edit access to
sales orders see the page; the currency note appears only when Multiple Currencies is enabled.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType Suitelet
 */
define(['N/runtime'], (runtime) => {
  const allowed = [runtime.Permission.FULL, runtime.Permission.EDIT];

  const onRequest = (context) => {
    const user = runtime.getCurrentUser();
    const level = user.getPermission({ name: 'TRAN_SALESORD' });
    if (!allowed.includes(level)) {
      context.response.write({ output: 'Edit access to sales orders is required.' });
      return;
    }

    const multiCurrency = runtime.isFeatureInEffect({ feature: 'MULTICURRENCY' });
    const dateFormat = user.getPreference({ name: 'DATEFORMAT' });
    const banner = runtime.envType === runtime.EnvType.SANDBOX ? '[SANDBOX] ' : '';

    context.response.write({
      output: `${banner}Account ${runtime.accountId} on ${runtime.version}; multi-currency: ${multiCurrency}; dates: ${dateFormat}`
    });
  };

  return { onRequest };
});
```

**Remembering a choice in the session.** A Suitelet keeps the warehouse a user picked so later
requests can default to it.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType Suitelet
 */
define(['N/runtime'], (runtime) => {
  const onRequest = (context) => {
    const session = runtime.getCurrentSession();
    const picked = context.request.parameters.warehouse;
    if (picked) {
      session.set({ name: 'preferred_warehouse', value: String(picked) });
    }
    const remembered = session.get({ name: 'preferred_warehouse' });
    context.response.write({ output: remembered ? `Using warehouse ${remembered}` : 'No warehouse chosen yet' });
  };

  return { onRequest };
});
```
