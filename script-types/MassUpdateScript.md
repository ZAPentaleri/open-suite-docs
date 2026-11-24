# MassUpdateScript
<!-- id: script:MassUpdateScript -->

A mass update script plugs custom logic into NetSuite's mass update feature. The standard (general)
mass updates only reach the fields they expose. With a mass update script you can make custom
updates to fields that general mass updates cannot reach, and you can compute each new value,
including complex calculations that look across many records.

## At a glance

| Item | Value |
|---|---|
| `@NScriptType` | `MassUpdateScript` |
| Usage-unit limit per execution | 1,000 |
| Time limit | 300 seconds |
| Runs on | Server |
| How it is started | On demand only, from the mass update's results preview |
| Features that must be enabled | "Server SuiteScript" and "Client SuiteScript" (both) |
| SuiteScript 1.0 counterpart | Mass Update Scripts ([1.0 concepts](../topics/ss1_api.mass_update.md)) |
| Entry points | [`each`](#eachcontext) |

Related pages: [SDF object](../topics/sdf.massupdatescript.md),
[SDF object elements](../gen/catalogs/sdf_massupdatescript_object.md),
[script deployment](../topics/script_deployment.md), [N/record](../modules/N_record.md),
[ScheduledScript](ScheduledScript.md), [MapReduceScript](MapReduceScript.md).

## How it runs

**Starting it.** A user runs the mass update, reviews the preview of matching records, and clicks
Perform Update on the preview results page. Only then does the script run: its `each` entry point is
called once for every record the update applies to.

**What it cannot do.** On-demand is the only submission mode. A mass update script cannot be
prioritized or scheduled, cannot be started from another script, and gives you no programmatic way to
report its status or to start or end it. If you need the work to happen at a particular time, use a
[scheduled script](ScheduledScript.md) or a [map/reduce script](MapReduceScript.md) instead.

**Side effects.** Saving records from a mass update script can fire the
[user event scripts](UserEventScript.md) deployed on those records.

**Setup requirements.**

- Two features must be on: "Server SuiteScript" and "Client SuiteScript". Both are switched on from
  the SuiteCloud subtab of the Enable Features page.
- The deployment's Applies To record type must match the record type being updated.
- The audience on the script deployment must match the audience of the mass update; if they differ,
  the script does not run.
- The user running it needs Edit or Full permission on the record type being updated.

**Managing scripts.** Mass update scripts can be managed with the SuiteCloud Development Framework,
copied between accounts with Copy to Account (its control is at the top right of the mass update
script page), and reviewed with SuiteScript Analysis.

**Typical job.** The source's illustration sets the probability of every selected opportunity to 61
percent; in practice the script earns its keep when each record's new value depends on that record's
own data.

## Entry points

### each(context)
<!-- id: script:MassUpdateScript.each -->

Called once for each record the mass update applies to. NetSuite passes the context automatically. It
tells you which record to work on; loading and saving it is up to your code.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `context` | Object | Yes | |
| `context.id` | number | - | |
| `context.type` | string | - | |

Both context properties exist since 2016.1.

- `context.id`: the internal ID of the record being updated.
- `context.type`: the record type of the record being updated.

**Returns:** `void`

**Errors:** the source does not state any errors specific to this entry point. What happens to the
rest of the run when one `each` call throws is not stated either; catching errors per record keeps
one bad record from affecting the others (general JavaScript guidance).

**Governance:** the execution is limited to 1,000 usage units; the source does not say whether that
budget applies to each `each` call or to the whole run.

## Examples

Assign each selected customer a credit-risk tier based on how much of its balance is overdue relative
to its credit limit. The custom field ID is invented; the deployment's Applies To is Customer:

```js
/**
 * @NApiVersion 2.1
 * @NScriptType MassUpdateScript
 */
define(['N/record'], (record) => {
  const tierFor = (overdue, limit) => {
    if (!limit) {
      return overdue > 0 ? 'C' : 'B';
    }
    const ratio = overdue / limit;
    if (ratio >= 0.5) {
      return 'C';
    }
    return ratio > 0 ? 'B' : 'A';
  };

  const each = (context) => {
    const customer = record.load({ type: context.type, id: context.id });
    const overdue = Number(customer.getValue({ fieldId: 'overduebalance' })) || 0;
    const limit = Number(customer.getValue({ fieldId: 'creditlimit' })) || 0;
    record.submitFields({
      type: context.type,
      id: context.id,
      values: { custentity_credit_risk_tier: tierFor(overdue, limit) }
    });
  };

  return { each };
});
```

Push the finish-no-later-than date of selected project tasks back (by a week unless configured) when
a project slips. The number of days comes from a script parameter (invented ID), so one script serves
several deployments, and a failure on one task is logged instead of thrown:

```js
/**
 * @NApiVersion 2.1
 * @NScriptType MassUpdateScript
 */
define(['N/record', 'N/runtime', 'N/log'], (record, runtime, log) => {
  const each = (context) => {
    const shiftDays = Number(runtime.getCurrentScript().getParameter({ name: 'custscript_shift_days' })) || 7;
    try {
      const projectTask = record.load({ type: context.type, id: context.id });
      const finish = projectTask.getValue({ fieldId: 'finishbydate' });
      if (!(finish instanceof Date)) {
        log.debug({ title: `Task ${context.id} skipped`, details: 'No finish-by date' });
        return;
      }
      const moved = new Date(finish.getTime() + shiftDays * 24 * 60 * 60 * 1000);
      projectTask.setValue({ fieldId: 'finishbydate', value: moved });
      projectTask.save();
    } catch (e) {
      log.error({ title: `Task ${context.id} not shifted`, details: `${e.name}: ${e.message}` });
    }
  };

  return { each };
});
```
