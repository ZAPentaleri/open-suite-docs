# SuiteScript 2.1 custom modules
<!-- id: topic:custom_modules -->

Part of: [SuiteScript 2.1 overview](ss21_overview.md)

Sub-topics:

- [Custom module tutorial](custom_modules.tutorial.md)

See also: [`define`](../globals.md), [`require`](../globals.md),
[`require` configuration](../globals.md), [SuiteScript 2.1 JSDoc validation](jsdoc_validation.md).

A custom module is an AMD module of your own, kept in the File Cabinet and loaded by scripts the same
way they load `N/*` modules. Use one for reusable libraries, for logic shared across a SuiteApp, or to
bring in a third-party library.

## Writing a module

- Create the module with the global [`define`](../globals.md). Its callback returns an object, usually
  made of functions, and that object is what loading scripts receive.
- The functions a module exposes can have any names; they are not tied to script-type entry point
  names.
- The `@NApiVersion` and `@NModuleScope` JSDoc tags are optional in a custom module, though adding
  `@NApiVersion` is recommended.
- `@NModuleScope` controls which scripts, and which accounts, may load the module (scope values are on
  the [JSDoc validation](jsdoc_validation.md) page). A bundled module meant for third parties must be
  public.
- A module never runs on its own; it needs a deployed entry-point script that loads it.
- Custom modules are not supported in [bundle installation scripts](../script-types/BundleInstallationScript.md).
- Upload modules to the SuiteScripts folder of the File Cabinet. A file inherits its folder's
  restrictions, and a module inside a restricted folder can behave erratically.
- Scripts cannot reach the DOM directly. To use jQuery on a Suitelet page, attach it through the form's
  client script, set with [`Form.clientScriptFileId`](../modules/N_ui_serverWidget.md) or
  [`Form.clientScriptModulePath`](../modules/N_ui_serverWidget.md).

**Version mixing risk:** when an entry script declared as 2.0 loads a module written for 2.1, the
module may contain syntax that 2.0 does not support.

## Referring to a module

A loading script names the module by its file name, without the `.js` extension (leaving the
extension off is the recommended form). Three path styles exist:

| Style | Starts with | Example |
|---|---|---|
| Absolute | `/` | `/SuiteScripts/myApp/util` |
| Relative | `./` or `../` | `./util` |
| Bundle virtual path | `/.bundle/<bundle id>/` | `/.bundle/<bundle id>/util` |

- Relative paths keep working when the whole folder is moved.
- Relative paths cannot be used in global contexts. To let `require` resolve a relative path, list
  `require` itself as a dependency of `define` and call that local copy.
- A virtual bundle path follows the bundle ID when the bundle is replaced. The source account's bundle
  ID cannot be used in it. To use a module from another SuiteApp, write its bundle path with that
  SuiteApp's bundle ID.

## define versus require

| Use `define` for | Use `require` for |
|---|---|
| Entry-point scripts | Imports by absolute path |
| New modules and what they export | The debugger |
| Relative imports | Test frameworks |
| | Progressive (on-demand) loading |

`define` loads all its dependencies up front; `require` loads them lazily, when the call runs. Moving a
dependency into the `define` list therefore gives up progressive loading.

## Third-party libraries

A script that uses a third-party library needs the `@NAmdConfig` tag, pointing at a configuration file
through a relative path.

- **General route**: define the library as a custom module, give it a global identifier, and add it as
  a dependency of the script.
- **Non-AMD library**: upload the library, write a JSON configuration file with `shim` and `paths`
  entries, reference that file with `@NAmdConfig`, then list the library as a dependency.

### The configuration file

The configuration is described on the [`require` configuration](../globals.md) entry of the globals
page. In short: it is optional and meant for advanced users; `@NAmdConfig` is the preferred way to
apply it (calling `require.config` with a context object still works for existing scripts but is not
preferred, and that context value is not a file path). The file lives in the File Cabinet, is written in
JSON syntax rather than JavaScript, and follows the amdjs CommonConfig format. Its settings take effect
before any dependency loads. Each client script can have its own configuration, or many scripts can
share one. The properties:

| Property | Type | Use |
|---|---|---|
| `baseUrl` | string | Root folder that shortens relative paths |
| `config` | Object | Attaches attributes such as metadata |
| `map` | Object | Module aliases, several names for one module, or modules with the same name (such as versions) |
| `packages` | Object[] | Uses a CommonJS package as a custom module |
| `paths` | Object | Named path aliases, mock modules for testing, custom names for SuiteScript modules |
| `shim` | Object | Loads a non-AMD JavaScript library |

The source does not describe the inner shape of each property beyond this.

### Named modules

Instead of a path, a module can be required by a name. Names come from a `define` call that passes a
module ID, from the `paths` section of an AMD configuration JSON file, or through the `@NAmdConfig`
tag. Names spare you from writing paths, help when wiring third-party libraries, and avoid name
conflicts. A typical case is a math library stored under SuiteScripts and given an alias in `paths`.

The source walks through these patterns: a utility module, importing it, a public SuiteApp module, an
AMD library, a non-AMD library loaded through a shim, an incrementing counter module, pinning a
specific jQuery version, and a math library shim.

## Troubleshooting

| Symptom | Likely cause / what to try |
|---|---|
| Module not found | `require` was called with a relative path but without the module context; require it through `define` |
| Module does not exist | The path does not resolve; try the absolute path |
| No permission to load the module | The module's `@NModuleScope` does not allow the caller; check the scope |
| Erratic behaviour | The module sits in a restricted folder |

## Example

A library module, saved as `/SuiteScripts/purchasing/lib/shipWindow.js`:

```js
/**
 * @NApiVersion 2.1
 * @NModuleScope SameAccount
 */
define([], () => {
  const MS_PER_DAY = 24 * 60 * 60 * 1000;

  const describeShipWindow = (shipDate, today) => {
    if (!shipDate) {
      return 'No ship date yet';
    }
    const days = Math.round((shipDate.getTime() - today.getTime()) / MS_PER_DAY);
    return days >= 0 ? `Ships in ${days} day(s)` : `Ship date passed ${-days} day(s) ago`;
  };

  return { describeShipWindow };
});
```

A user event script in `/SuiteScripts/purchasing/` that loads it by relative path (no `.js`) and fills
an empty memo on purchase orders:

```js
/**
 * @NApiVersion 2.1
 * @NScriptType UserEventScript
 */
define(['N/log', './lib/shipWindow'], (log, shipWindow) => {
  const beforeSubmit = (context) => {
    const po = context.newRecord;
    if (po.getValue({ fieldId: 'memo' })) {
      return;
    }
    const note = shipWindow.describeShipWindow(po.getValue({ fieldId: 'shipdate' }), new Date());
    po.setValue({ fieldId: 'memo', value: note });
    log.debug({ title: `PO ${po.getValue({ fieldId: 'tranid' })}`, details: note });
  };

  return { beforeSubmit };
});
```

Loading the same library on demand instead: list `require` as a dependency so the relative path
resolves, and pull the module in only on the branch that needs it.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType UserEventScript
 */
define(['require', 'N/log'], (require, log) => {
  const afterSubmit = (context) => {
    const po = context.newRecord;
    if (po.getValue({ fieldId: 'memo' })) {
      return;
    }
    require(['./lib/shipWindow'], (shipWindow) => {
      const note = shipWindow.describeShipWindow(po.getValue({ fieldId: 'shipdate' }), new Date());
      log.audit({ title: 'Ship window', details: note });
    });
  };

  return { afterSubmit };
});
```
