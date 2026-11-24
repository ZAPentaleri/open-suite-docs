# SDFInstallationScript
<!-- id: script:SDFInstallationScript -->

An SDF installation script runs while a SuiteApp project built with the SuiteCloud Development
Framework is being deployed, and does the account-side work that deployment needs. Typical jobs:
setting up and checking the target account, confirming that the required permissions and features
are in place before anything is installed, blocking an installation into an account that has not
been prepared, halting the installation when a requirement is missing, migrating data that already
exists, and creating custom record fields according to which features are enabled.

## At a glance

| Item | Value |
|---|---|
| `@NScriptType` | `SDFInstallationScript` |
| Usage-unit limit per execution | 10,000 |
| Time limit | 3,600 seconds |
| Runs on | Server, automatically when the SuiteApp project is deployed |
| When exactly | At the point the project's deploy XML specifies |
| Entry points | [`run`](#runcontext) |

Related pages: [SDF object](../topics/sdf.sdfinstallationscript.md),
[SDF object elements](../gen/catalogs/sdf_sdfinstallationscript_object.md),
[SDF script fields](../gen/catalogs/sdf_sdfinstallationscript_script_fields.md),
[SDF deployments](../gen/catalogs/sdf_sdfinstallationscript_deployments.md),
[deploy file](../topics/sdf.deploy_file.md), [SDF](../topics/sdf.md),
[BundleInstallationScript](BundleInstallationScript.md), [N/runtime](../modules/N_runtime.md).

## How it runs

**Relation to bundle installation scripts.** It fills the same role as a
[bundle installation script](BundleInstallationScript.md), but for SuiteApps deployed with SDF rather
than bundles distributed through SuiteBundler.

**Triggering.** It runs by itself whenever the SuiteApp project is deployed. Where in the deployment
it runs is decided by the position of its entry in the project's deploy XML (see
[deploy file](../topics/sdf.deploy_file.md)). The SDF object page adds that this script type is
supported in SuiteApp projects only, not in account customization projects.

**Records it needs.** A script record and a script deployment record. You can create them as XML in
the SuiteCloud project, with SuiteScript, or in the NetSuite account UI. The deployment can carry
values for the script's custom parameters; the object's XML elements and fields are listed on the
[SDF object](../topics/sdf.sdfinstallationscript.md) page and its catalogs.

**Monitoring.** Follow a run in the SuiteCloud IDE deploy log, or in the execution log on the script
record or on its deployment.

**Managing scripts.** SDF installation scripts can be managed with the SuiteCloud Development
Framework and copied to another account with Copy to Account.

## Entry points

### run(context)
<!-- id: script:SDFInstallationScript.run -->

The single entry point, run at the point the deploy XML specifies. Its context tells you which
version is installed already and which version is arriving. The script can read its custom script
parameters with [`Script.getParameter`](../modules/N_runtime.md).

| Parameter | Type | Required | Default |
|---|---|---|---|
| `context` | Object | Yes | |
| `context.fromVersion` | string | - | |
| `context.toVersion` | string | - | |

Both context properties exist since 2015.2; the source gives no required flag for either.

- `context.fromVersion`: the version currently installed in the account, or `null` on a fresh
  installation. That is how you tell a first install from an upgrade.
- `context.toVersion`: the version being installed.

> [!SOURCE-ERROR] `context.fromVersion` is typed as a string, yet it is also said to be `null` for a
> new installation. Trust the `null` case: test for `null` explicitly before treating the value as a
> version string.
<!-- source-error: script:SDFInstallationScript.run | fromVersion | fromversion_typed_string_but_null_for_new_install -->

**Returns:** `void`

**Errors:** the source names no error codes for this entry point. The
[SDF object](../topics/sdf.sdfinstallationscript.md) page states that an error thrown by the script
makes the SuiteApp deployment fail, and the error is logged in the script log and in the console.
Throwing is therefore how you stop an installation when a requirement is not met.

**Governance:** the whole execution may use at most 10,000 usage units.

## Examples

Refuse to deploy into an account without the Multi-Location Inventory feature (feature ID
`MULTILOCINVT`), and on an upgrade from a 1.x version copy a legacy value into its replacement field.
The custom record and field IDs are invented:

```js
/**
 * @NApiVersion 2.1
 * @NScriptType SDFInstallationScript
 */
define(['N/runtime', 'N/error', 'N/search', 'N/record', 'N/log'], (runtime, error, search, record, log) => {
  const migrateZones = () => {
    let moved = 0;
    search.create({
      type: 'customrecord_pick_zone',
      columns: ['internalid', 'custrecord_zone_code_legacy']
    }).run().each((result) => {
      record.submitFields({
        type: 'customrecord_pick_zone',
        id: result.id,
        values: { custrecord_zone_code: result.getValue({ name: 'custrecord_zone_code_legacy' }) }
      });
      moved += 1;
      return true;
    });
    return moved;
  };

  const run = (context) => {
    if (!runtime.isFeatureInEffect({ feature: 'MULTILOCINVT' })) {
      // A thrown error fails the SuiteApp deployment.
      throw error.create({
        name: 'PICKZONES_NEEDS_LOCATIONS',
        message: 'Enable Multi-Location Inventory before deploying this SuiteApp.'
      });
    }

    if (context.fromVersion === null) {
      log.audit({ title: 'Fresh install', details: `Installing ${context.toVersion}` });
      return;
    }

    if (String(context.fromVersion).startsWith('1.')) {
      const count = migrateZones();
      log.audit({ title: 'Upgrade migration', details: `${count} zones copied for ${context.toVersion}` });
    }
  };

  return { run };
});
```

Read a script parameter set on the deployment to decide whether optional sample data should be
skipped (the parameter ID is invented):

```js
/**
 * @NApiVersion 2.1
 * @NScriptType SDFInstallationScript
 */
define(['N/runtime', 'N/log'], (runtime, log) => {
  const run = (context) => {
    const skipSamples = runtime.getCurrentScript().getParameter({ name: 'custscript_skip_sample_data' });
    log.audit({
      title: `Deploying ${context.toVersion}`,
      details: skipSamples ? 'Sample data skipped by parameter' : 'Sample data will be loaded'
    });
  };

  return { run };
});
```
