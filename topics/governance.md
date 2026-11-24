# SuiteScript Governance and Limits
<!-- id: topic:governance -->

Part of: [SuiteScript Developer Guide](developer_guide.md)

See also: [N/runtime](../modules/N_runtime.md), [SuiteScript Debugger](debugger.md),
[Script monitoring and logs](monitoring.md), [Setting Runtime Options](runtime_options.md),
[Map/reduce governance](map_reduce.governance.md), catalogs
[other_usage_unit_limits](../gen/catalogs/other_usage_unit_limits.md) and
[other_time_limits](../gen/catalogs/other_time_limits.md).

NetSuite meters scripts so that no single script can monopolize shared resources. Several independent
limits apply: usage units, execution time, instruction or statement counts, logging volume, and a few
data-size caps. Going over most of them ends the script.

## Usage units

Governance is counted in **usage units**, tracked both per script type and per API. Each script type
has a budget, and each API call spends a fixed number of units from it. A script that exceeds its
budget is terminated, and the usage limit error is emailed to the script owner.

Check what is left with [`Script.getRemainingUsage()`](../modules/N_runtime.md) (on the object from
`runtime.getCurrentScript()`), and watch consumption in the debugger's execution log while testing.

### Usage-unit limits by script type

| Script type | Usage units | Notes |
|---|---|---|
| [Client script](../script-types/ClientScript.md) | 1,000 | Each client script is metered on its own; scripts on the same page do not share a pool |
| [User event script](../script-types/UserEventScript.md) | 1,000 | |
| [Suitelet](../script-types/Suitelet.md) | 1,000 | |
| [Portlet](../script-types/Portlet.md) | 1,000 | |
| [Mass update script](../script-types/MassUpdateScript.md) | 1,000 | |
| [Workflow action script](../script-types/WorkflowActionScript.md) | 1,000 | Applies per workflow state: all actions in one state share the 1,000 units |
| [Custom tool](../script-types/CustomTool.md) | 1,000 | |
| [Event subscriber](../script-types/EventSubscriber.md) | 1,000 | |
| [RESTlet](../script-types/Restlet.md) | 5,000 | |
| [Scheduled script](../script-types/ScheduledScript.md) | 10,000 | |
| [Bundle installation script](../script-types/BundleInstallationScript.md) | 10,000 | |
| [SDF installation script](../script-types/SDFInstallationScript.md) | 10,000 | |
| [Map/reduce script](../script-types/MapReduceScript.md) | Per invocation: getInputData 10,000; map 1,000; reduce 5,000; summarize 10,000 | Counted per invocation of a stage function, not per deployment (see [Map/reduce governance](map_reduce.governance.md)) |
| Any script running in the debugger | 1,000 | Regardless of the script's normal budget |

### Other usage-unit limits

From [catalog:other_usage_unit_limits](../gen/catalogs/other_usage_unit_limits.md):

| Kind | Usage units |
|---|---|
| Core plug-ins | 10,000, unless the plug-in defines a lower limit |
| Custom plug-ins | 10,000 per plug-in |
| SSP application scripts | 1,000 |

### Cost per operation

What a call costs depends on the kind of record it touches. Custom records are cheapest, standard
non-transaction records cost more, and standard transaction records cost the most. "Standard
non-transaction" covers the activity, communication, customization, entity, File Cabinet, item, list,
marketing, subrecord, support and website record categories.

Some common operations, with figures from the module pages:

| Operation | Transaction | Standard non-transaction | Custom record |
|---|---|---|---|
| [`record.create`, `record.copy`, `record.load`, `record.transform`, `record.submitFields`](../modules/N_record.md) | 10 | 5 | 2 |
| [`record.delete`, `Record.save`](../modules/N_record.md) | 20 | 10 | 4 |

| Operation | Units |
|---|---|
| [`record.attach`, `record.detach`](../modules/N_record.md) | 10 |
| [`email.send`](../modules/N_email.md) | 20 |
| [`email.sendBulk`, `email.sendCampaignEvent`](../modules/N_email.md) | 10 |
| [`https.get`, `https.post`, `https.put`, `https.delete`, `https.request`](../modules/N_https.md) | 10 |

Each module page gives the figure for every call.

### Worked totals

The source gives these totals; the breakdowns follow from the tables above.

| Scenario | Total |
|---|---|
| User event that deletes an invoice and saves a record (transaction) | 40 units |
| Scheduled script that loads a customer, transforms a record and sends an email | 30 units |
| Scheduled script with two transforms and one email | 24 to 40 units, depending on record kind |
| Suitelet that creates a record and makes an HTTP GET | 12 to 20 units, depending on record kind |

> [!SOURCE-ERROR] The source describes the transform in the 30-unit scheduled-script example as being
> on a "non-standard transaction", which matches none of its own record categories. Work out the cost
> from the actual record type being transformed rather than from that wording.
<!-- source-error: topic:governance | - | transform_example_says_non_standard_transaction -->

Scripts that generate excessive automated activity may also be metered beyond these unit limits.

### Staying inside the budget

- Branch on the remaining usage, for example continuing only while more than 50 units remain.
- SuiteScript 2.1 scheduled scripts have no yield or recovery point, so long-running scheduled work
  belongs in a map/reduce script.
- Keep Suitelets and user event scripts quick, because users wait on them.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType ScheduledScript
 */
define(['N/runtime', 'N/record', 'N/log'], (runtime, record, log) => {
  const execute = () => {
    const script = runtime.getCurrentScript();
    const raw = script.getParameter({ name: 'custscript_release_hold_ids' }) || '';
    const orderIds = raw.split(',').filter((id) => id.trim() !== '');

    for (const id of orderIds) {
      // submitFields on a transaction costs 10 units; keep a margin of 50.
      if (script.getRemainingUsage() <= 50) {
        log.audit({ title: 'Budget nearly spent', details: `stopped before order ${id}` });
        break;
      }
      record.submitFields({
        type: record.Type.SALES_ORDER,
        id: id.trim(),
        values: { memo: 'Hold released by nightly job' }
      });
    }
  };

  return { execute };
});
```

## Time limits

A script that runs past its time limit fails with `SSS_TIME_LIMIT_EXCEEDED`.

| Script type | Time limit |
|---|---|
| Client, user event, Suitelet, portlet, mass update, workflow action, custom tool, event subscriber, RESTlet | 300 s |
| Scheduled, bundle installation, SDF installation, map/reduce | 3,600 s |
| Any script in the debugger | 300 s |

Map/reduce stages have their own limits:

| Stage | Time limit |
|---|---|
| getInputData | 3,600 s |
| map | 300 s |
| reduce | 900 s |
| summarize | 3,600 s |

Other time limits, from [catalog:other_time_limits](../gen/catalogs/other_time_limits.md):

| Kind | Time limit |
|---|---|
| Custom record action | 300 s |
| Single-page application (SPA) | 300 s |
| SuiteScript Server Pages application | 300 s |
| Custom GL lines (core plug-in) | 30 s |
| Tax Engine plug-in (core) | 60 s |
| Bank statement parsing, financial institution parsing (core) | 1,800 s |
| Financial institution connectivity (core) | 3,600 s |
| Adjusted Consolidated Rates, Bank Connectivity, Dataset Builder, Email Capture, Payment Processing, Platform Extension, Promotions, Revenue Management, Shipping Partners, Workbook Builder plug-ins (core) | 300 s |
| Custom plug-in type, custom plug-in implementation | 3,600 s |

A script can run for a long time without reaching a unit limit, for example when it:

- does many record operations that each stay within the units,
- touches many transactions that hit the same records,
- triggers many user event scripts or workflows, or
- waits on slow database queries.

## Instruction and statement counts

Separately from units and time, the engine counts the work done:

- SuiteScript 1.0 and 2.0 count **instructions**; going over raises `SSS_INSTRUCTION_COUNT_EXCEEDED`.
- SuiteScript 2.1 counts **statements**, a higher-level measure; going over raises
  `SSS_STATEMENT_COUNT_EXCEEDED`.

One statement can expand into many instructions; sorting an array, for instance, runs many engine
instructions. Either way the script is terminated, so make sure every loop has a reachable exit.

## Map/reduce data limits

When a map/reduce stage writes a key/value pair:

- the key may be at most 3,000 characters; a longer key raises `KEY_LENGTH_IS_OVER_3000_BYTES`;
- the value may be at most 10 MB; a larger value raises `VALUE_LENGTH_IS_OVER_10_MB`.

Carry data in values, not in keys.

## Logging limits

| Limit | Value |
|---|---|
| Log calls per company | 100,000 per 60 minutes |
| Retention of system error logs | 60 days |
| Retention of user logs | 30 days |
| Retention on the Script Execution Logs page | 30 days |
| Log capacity per database instance | 5,000,000 entries |

- When a database's log capacity is reached, **all** logs on it are purged. If you need a complete
  history, write it to custom records.
- A script that logs excessively has its log level raised automatically. For example, a script making
  70,000 debug calls within 20 minutes gets a higher level. The script keeps running; calls below the
  new level are dropped. The new level shows in the deployment's **Log Level** field, the owner gets an
  email, and an entry is added to the execution log.
- On busy deployments, log less: raise the log level or write fewer log lines (see
  [runtime options](runtime_options.md)).
- If a log search misses recent entries, use the filters on the Script Execution Logs page instead.

## Search limits

| Limit | Value |
|---|---|
| Results returned by running a search | 1,000 |
| Results from running the result set of a loaded search | 4,000 |
| Bytes in a text column | 4,000 |

With multibyte character sets, the 4,000-byte text column holds fewer than 4,000 characters.
