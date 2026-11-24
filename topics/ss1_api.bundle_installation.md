# SuiteScript 1.0 bundle installation scripts
<!-- id: topic:ss1_api.bundle_installation -->

> **Legacy material.** This page exists so that 1.0 scripts can be read, converted and minimally
> repaired. Do not write new 1.0 code; the 2.1 counterpart is
> [BundleInstallationScript](../script-types/BundleInstallationScript.md).

Part of: [SuiteScript 1.0 API conventions](ss1_api.md)

A bundle installation script runs code at fixed points while a bundle is installed into, updated
in, or removed from a target account.

## Trigger functions and their arguments

The script record names a function for each trigger you use. NetSuite passes version numbers to
them:

| Trigger | `toversion` | `fromversion` |
|---|---|---|
| `beforeInstall` | Yes | No |
| `afterInstall` | Yes | No |
| `beforeUpdate` | Yes | Yes |
| `afterUpdate` | Yes | Yes |
| `beforeUninstall` | No | No |

`toversion` is the bundle version being installed or updated to; `fromversion` is the version
being replaced and only exists for the two update triggers. The only uninstall hook in the trigger
list is the one that runs before uninstalling.

> [!SOURCE-ERROR] The source's overview describes the uninstall hook as running after uninstall,
> while its list of triggers names a before-uninstall hook. This page follows the trigger list;
> confirm the timing in a test account before relying on it.
<!-- source-error: topic:ss1_api.bundle_installation | - | overview_says_after_uninstall_list_says_before_uninstall -->

## Execution rules

**Governance:** 10,000 usage units per execution.

**Runs as:** the Administrator role.

- Scheduled scripts can be queued only from the after-install and after-update triggers.
- If the script fails, the install, update or uninstall it belongs to is aborted.
- The script's deployment status must be Released for it to run in target accounts.

## Converting

In 2.x the same five triggers are entry points of
[BundleInstallationScript](../script-types/BundleInstallationScript.md), and the versions arrive as
`params.toVersion` and `params.fromVersion`. See
[Converting SuiteScript 1.0 scripts](ss21_transition.from_ss1.md).

## Example (1.0, for reading legacy code)

An after-update function that queues a one-off data fix only when an account moves past a given
version. It runs from an after trigger, which is where queueing a scheduled script is allowed.

```js
function afterBundleUpdate(toversion, fromversion) {
  var from = parseFloat(fromversion);
  var to = parseFloat(toversion);
  if (from < 3.0 && to >= 3.0) {
    nlapiScheduleScript('customscript_backfill_case_category', 'customdeploy_backfill_once', null);
  }
}
```
