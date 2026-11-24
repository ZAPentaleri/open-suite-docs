# BundleInstallationScript
<!-- id: script:BundleInstallationScript -->

A bundle installation script is server-side code that travels inside a SuiteBundle and runs in the
target account when the bundle is installed, updated or uninstalled. Its job is the setup,
configuration and data work the bundle needs in that account: checking that the account is ready,
configuring things after installation, adjusting data around an upgrade, and tidying up before
removal.

## At a glance

| Item | Value |
|---|---|
| `@NScriptType` | `BundleInstallationScript` |
| Usage-unit limit per execution | 10,000 |
| Time limit | 3,600 seconds |
| Runs on | Server, automatically, as the Administrator role |
| Feature needed to attach it to a bundle | `SUITEBUNDLER` |
| Installation scripts per bundle | At most one |
| SuiteScript 1.0 counterpart | Bundle Installation Scripts ([1.0 concepts](../topics/ss1_api.bundle_installation.md)) |
| Entry points | [`beforeInstall`](#beforeinstallcontext), [`afterInstall`](#afterinstallcontext), [`beforeUpdate`](#beforeupdatecontext), [`afterUpdate`](#afterupdatecontext), [`beforeUninstall`](#beforeuninstallcontext) |

Related pages: [SDF object](../topics/sdf.bundleinstallationscript.md),
[SDF object elements](../gen/catalogs/sdf_bundleinstallationscript_object.md),
[SDF script fields](../gen/catalogs/sdf_bundleinstallationscript_script_fields.md),
[SDF deployments](../gen/catalogs/sdf_bundleinstallationscript_deployments.md),
[script deployment](../topics/script_deployment.md),
[SDFInstallationScript](SDFInstallationScript.md) (the SDF SuiteApp counterpart),
[N/runtime](../modules/N_runtime.md), [N/error](../modules/N_error.md).

## How it runs

**When it fires.** The script runs as part of installing, updating or uninstalling a bundle. Across a
bundle's life its entry points run in this order: `beforeInstall`, `afterInstall`, `beforeUpdate`,
`afterUpdate`, `beforeUninstall`. It runs on its own, with no user involved: there is no audience to
set and no event types to choose, unlike record-based scripts. The script must implement at least
one entry point, and keeping all of them in the same script file is recommended.

The source's script-type page lists the run moments as before install, after install, before
update, after update and *after* uninstall, while the only uninstall entry point defined is
`beforeUninstall`.

> [!SOURCE-ERROR] The script-type page describes the uninstall moment as "after uninstall", but the
> uninstall entry point is `beforeUninstall`, which runs before the bundle is removed. Rely on the
> entry point's own description (before uninstall), and verify in a test account if timing matters.
<!-- source-error: script:BundleInstallationScript | - | type_page_says_after_uninstall_entry_point_is_before -->

**A failure stops the operation.** If the script fails, the install, update or uninstall it belongs
to stops. The error shown is an installation error code followed by the message your script
supplied, so throw errors whose message tells an administrator what to fix.

**Version arguments.** The version string an entry point receives matches the version shown on the
Bundle Basics page of the Bundle Builder. The install and uninstall entry points get a single
`version`; the update entry points get `fromVersion` and `toVersion`.

> [!SOURCE-ERROR] The source's reference listing names `fromVersion` and `toVersion` for the install
> entry points as well, while the pages for those entry points use a single `version`. This page
> follows the entry-point pages; if your code depends on it, log the context in a test install to
> confirm.
<!-- source-error: script:BundleInstallationScript | - | reference_names_toversion_fromversion_but_pages_use_version -->

**Scheduled scripts.** Only `afterInstall` and `afterUpdate` may call scheduled scripts.
`beforeInstall`, `beforeUpdate` and `beforeUninstall` cannot.

**Library files.** The script can call functions in other script files, provided those files are
listed as library files on the script record; every file an entry point calls must be listed there.
The entry point functions themselves must be in the main script file. Library files listed on the
script record are added to the bundle automatically. Custom modules are not supported in bundle
installation scripts.

**Building and attaching it.** The overall sequence: write the JavaScript file with its entry
points, add it to the File Cabinet, create the script record, define a deployment, and associate the
script with a bundle.

1. *The file.* Storing it in the SuiteScripts folder is recommended. The file must be marked as
   available for SuiteBundles: add it to a File Cabinet folder, edit its file record, check the
   available-for-SuiteBundles box, optionally check Hide in SuiteBundle, and save. By default the
   file's contents are visible in target accounts; Hide in SuiteBundle hides the file from users
   there.
2. *The script record.* Create it from the new-script page by choosing the bundle installation type.
   It needs a name, the script file, and at least one function on the Scripts subtab.

> [!SOURCE-ERROR] The source gives the navigation path for creating the script record as a Setup >
> Customization > Scripts route, which appears to be wrong. The spec does not give a corrected path;
> find the new-script page in your account's menus.
<!-- source-error: script:BundleInstallationScript | - | path_given_as_setup_customization_scripts -->

3. *The deployment.* Create it with Save and Deploy, with the Deploy Script button after saving, or by
   editing an existing deployment. The deployment must run as Administrator (the execute-as-admin
   setting checked). One script can have many deployments, each with different parameter values, but
   a bundle uses only one deployment, and a bundle can have at most one installation script. The same
   script can, however, be associated with several bundles.
4. *Status.* The deployment must be `Released` for the script to run in target accounts. Use
   `Testing` while you debug.

> [!SOURCE-ERROR] In the source, the link that should lead on to the next step points back to the same
> topic. No behaviour depends on it; continue with the bundle association step below.
<!-- source-error: script:BundleInstallationScript | - | next_link_points_to_same_topic -->

5. *Associating with the bundle.* This needs the `SUITEBUNDLER` feature, a script record and at least
   one deployment. Choose the deployment in the Bundle Builder, from the installation script dropdown
   on the Bundle Basics step. For a new bundle you reach the Bundle Builder through the create-bundle
   page; for an existing one, edit it from the SuiteBundler list of bundles (search and install
   bundles). You do not need to add the script on the Select Objects page: the script file, the script
   record and the library files go into the bundle automatically and appear as bundle components on
   the Bundle Details page. Hiding the script file's contents from target accounts is a File Cabinet
   record option (see step 1).

> [!SOURCE-ERROR] The source places the create-bundle page under Customization > SuiteBuilder, which
> appears to confuse SuiteBuilder with SuiteBundler. Find the create-bundle command in your account's
> menus rather than following that path literally.
<!-- source-error: script:BundleInstallationScript | - | create_bundle_path_says_suitebuilder -->

**Managing scripts.** Bundle installation scripts can be managed with the SuiteCloud Development
Framework, copied between accounts with Copy to Account, and reviewed with SuiteScript Analysis.

## Entry points

The context properties carry no required flag in the spec, so the tables show `-` for them.

### beforeInstall(context)
<!-- id: script:BundleInstallationScript.beforeInstall -->

Runs before the bundle is installed in a target account for the first time. Use it to check that
the account is set up as the bundle expects, and to halt the installation when it is not. The
source's example idea: check for time-off management before installing.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `context` | Object | Yes | |
| `context.version` | string | - | |

- `context.version`: the version of the bundle being installed. Since 2016.1.

**Returns:** `void`

- Cannot call scheduled scripts.
- A failure here stops the installation.

### afterInstall(context)
<!-- id: script:BundleInstallationScript.afterInstall -->

Runs after the bundle has been installed in a target account for the first time; NetSuite passes the
context automatically. Use it to automate setup and configuration, or to fetch data from an external
system.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `context` | Object | Yes | |
| `context.version` | string | - | |

- `context.version`: the version of the bundle being installed. Since 2016.1.

**Returns:** `void`

- May call scheduled scripts.

### beforeUpdate(context)
<!-- id: script:BundleInstallationScript.beforeUpdate -->

Runs before a bundle update in the target account. Use it for data changes that must happen before
the upgrade.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `context` | Object | Yes | |
| `context.fromVersion` | string | - | |
| `context.toVersion` | string | - | |

- `context.fromVersion`: the version currently installed. Since 2016.1.
- `context.toVersion`: the new version. Since 2016.1.

**Returns:** `void`

- Cannot call scheduled scripts.
- Before this entry point runs, objects and files missing from the new version have already been
  deleted. If `beforeUpdate` then fails, the account stays on the old version, but those objects are
  gone. So remove obsolete objects from a bundle only after every account that has it installed has
  upgraded.

### afterUpdate(context)
<!-- id: script:BundleInstallationScript.afterUpdate -->

Runs after a bundle update in the target account. Use it, for example, to fetch data from an
external system.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `context` | Object | Yes | |
| `context.fromVersion` | string | - | |
| `context.toVersion` | string | - | |

- `context.fromVersion`: the version that was installed before the update. Since 2016.1.
- `context.toVersion`: the new version. Since 2016.1.

**Returns:** `void`

- May call scheduled scripts.

### beforeUninstall(context)
<!-- id: script:BundleInstallationScript.beforeUninstall -->

Runs before the bundle is uninstalled from a target account. Use it to reset configuration or remove
data the bundle created.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `context` | Object | Yes | |
| `context.version` | string | - | |

- `context.version`: the version of the bundle being uninstalled. Since 2016.1.

**Returns:** `void`

- Cannot call scheduled scripts.

## Examples

A warehouse bundle that refuses to install unless Multi-Location Inventory is enabled, and after an
update from a 1.x version opens a task reminding the administrator to review the new settings:

```js
/**
 * @NApiVersion 2.1
 * @NScriptType BundleInstallationScript
 */
define(['N/runtime', 'N/error', 'N/record', 'N/log'], (runtime, error, record, log) => {
  const beforeInstall = (context) => {
    if (!runtime.isFeatureInEffect({ feature: 'MULTILOCINVT' })) {
      throw error.create({
        name: 'WMS_PREREQUISITE_MISSING',
        message: `Version ${context.version} needs Multi-Location Inventory. Enable it, then retry the install.`
      });
    }
  };

  const afterUpdate = (context) => {
    const majorBefore = parseInt(String(context.fromVersion).split('.')[0], 10);
    log.audit({ title: 'Bundle updated', details: `${context.fromVersion} -> ${context.toVersion}` });
    if (majorBefore >= 2) {
      return;
    }
    const reminder = record.create({ type: 'task' });
    reminder.setValue({ fieldId: 'title', value: `Review bin settings introduced in ${context.toVersion}` });
    reminder.setValue({ fieldId: 'assigned', value: runtime.getCurrentUser().id });
    reminder.save();
  };

  return { beforeInstall, afterUpdate };
});
```

Cleaning up before removal: delete the bundle's own configuration records so nothing orphaned is left
behind. The custom record type ID is invented:

```js
/**
 * @NApiVersion 2.1
 * @NScriptType BundleInstallationScript
 */
define(['N/search', 'N/record', 'N/log'], (search, record, log) => {
  const beforeUninstall = (context) => {
    let removed = 0;
    search.create({ type: 'customrecord_wms_bin_rule', columns: ['internalid'] })
      .run()
      .each((result) => {
        record.delete({ type: 'customrecord_wms_bin_rule', id: result.id });
        removed += 1;
        return true;
      });
    log.audit({ title: `Uninstalling ${context.version}`, details: `${removed} bin rules removed` });
  };

  return { beforeUninstall };
});
```
