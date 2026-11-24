# N/config
<!-- id: N/config -->

`N/config` opens the account's configuration pages (company information, the various preference
pages, enabled features, accounting and tax period management, and so on) as record objects, so a
script can read or change settings with the usual `getValue` / `setValue` / `save` calls. You do
not need to load `N/record` to use it; the object you get back is a
[`record.Record`](N_record.md) all the same.

- **Availability:** server scripts only.
- **Load it as:** `define(['N/config'], (config) => { ... })`
- Preference field IDs: [preferences catalog](../gen/catalogs/preferences.md).
- Related: [N/record](N_record.md), [Record modes](../topics/record_modes.md).

## Members

| Member | Kind | Summary |
|---|---|---|
| [`config.load(options)`](#configloadoptions) | function | Open a configuration page as a record |
| [`config.Type`](#configtype) | enum | Which configuration page to open |

## Functions

### config.load(options)
<!-- id: N/config.load -->

Loads the configuration page you name and returns it as a record object. The field IDs you can
read and write on it are the preference IDs listed in the
[preferences catalog](../gen/catalogs/preferences.md).

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.type` | enum | Yes | |
| `options.isDynamic` | boolean | No | |

- `options.type`: a value from [`config.Type`](#configtype). Since 2015.2.
- `options.isDynamic`: pass `true` to get the record in dynamic mode. The source states no default
  value. See [Record modes](../topics/record_modes.md) for how the modes differ. Since 2015.2.

**Returns:** `record.Record` representing the chosen configuration page.

**Errors:**

- `INVALID_RCRD_TYPE`: `options.type` is missing or is not a valid configuration type.

**Governance:** 10 units.

**Availability:** server scripts.

**Since:** 2015.2

## Enums

### config.Type
<!-- id: N/config.Type -->

The configuration pages that [`config.load`](#configloadoptions) can open. Since 2015.2.

| Value | Configuration page |
|---|---|
| `ACCOUNTING_PERIODS` | The page for managing accounting periods |
| `ACCOUNTING_PREFERENCES` | Accounting preferences |
| `COMPANY_INFORMATION` | Company information |
| `COMPANY_PREFERENCES` | General (company-wide) preferences |
| `FEATURES` | The page where features are enabled |
| `MANUFACTURING_PREFERENCES` | Manufacturing preferences |
| `TAX_PERIODS` | The page for managing tax periods |
| `USER_PREFERENCES` | The user's own Set Preferences page |
| `TIME_POST` | The source does not state which page this opens |
| `TIME_VOID` | The source does not state which page this opens |

The source lists `TIME_POST` and `TIME_VOID` by name only; it does not say what they open or which
fields they carry. Load one in a sandbox and inspect it before depending on it.

> [!SOURCE-ERROR] In the source, the entry for `FEATURES` points to a further reference that turns out
> to be empty, so the list of feature IDs you can read from that page is not given. Check the field
> IDs on the loaded record rather than relying on a documented list.
<!-- source-error: N/config.Type | - | features_value_has_empty_cross_reference -->

## Examples

A scheduled script that reads the account's date and number formats once, so it can format values
for a flat-file export consistently with what users see.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType ScheduledScript
 */
define(['N/config', 'N/log'], (config, log) => {
  const execute = () => {
    const generalPrefs = config.load({ type: config.Type.COMPANY_PREFERENCES });
    const formats = {
      date: generalPrefs.getValue({ fieldId: 'DATEFORMAT' }),
      number: generalPrefs.getValue({ fieldId: 'NUMBERFORMAT' }),
      negative: generalPrefs.getValue({ fieldId: 'NEGATIVE_NUMBER_FORMAT' })
    };
    log.audit({ title: 'Export formats', details: JSON.stringify(formats) });
  };

  return { execute };
});
```

Tightening a manufacturing preference from an administrative Suitelet:

```js
/**
 * @NApiVersion 2.1
 * @NScriptType Suitelet
 */
define(['N/config'], (config) => {
  const onRequest = (context) => {
    if (context.request.method !== 'POST') {
      context.response.write({ output: 'Send a POST to apply the setting.' });
      return;
    }
    const mfgPrefs = config.load({ type: config.Type.MANUFACTURING_PREFERENCES });
    mfgPrefs.setValue({ fieldId: 'ALLOWASSEMBLYPURCHASE', value: false });
    mfgPrefs.save();
    context.response.write({ output: 'Assembly purchasing is now disabled.' });
  };

  return { onRequest };
});
```
