# Record actions and macros
<!-- id: topic:record_actions_macros -->

Part of: [SuiteScript 2.1 overview](ss21_overview.md)

See also: [N/action](../modules/N_action.md), [N/record](../modules/N_record.md),
[Transaction records](transactions.md).

Many record pages carry buttons that run business logic: approve a time entry, start an inventory
count, preview tax. Record actions and macros let a script trigger that logic in code, which makes them
a good fit for automating routine UI chores. Running one has the same result as clicking the matching
button in the UI, and since one call stands in for a series of field updates, it usually means fewer
API calls and lower governance usage.

## Actions versus macros

| | Actions | Macros |
|---|---|---|
| API | [N/action](../modules/N_action.md) | [`Record.executeMacro()`](../modules/N_record.md) and the [`record.Macro`](../modules/N_record.md) object |
| Applies to | a record being viewed (not open for editing) | a record while it is being edited |
| When the change is stored | immediately | when the record is saved |
| Role and permission checks | none are performed | the source does not state whether they are performed |
| Scale | can run in bulk on many records of one type | one record at a time |
| Governance | depends on the action type | none charged (0 usage units) |

Other points:

- If the conditions an action needs are not met (for example the record is not in a suitable state),
  the action simply does not run.
- Actions can also be run through REST web services.
- Previewing tax with a calculate-tax macro is a typical macro use.

## Record types with actions

Actions exist on these record types: `bonus`, `charge`, `inventorycount`, `invoice`, `invoicegroup`,
`revenuearrangement`, `supplychangeorder`, `supplyplandefinition`, `timebill` (time entry),
`vendorpayment` and `timesheet`.

Some examples:

| Record | Action | What it does |
|---|---|---|
| `inventorycount` | start count | starts the count |
| `timebill` | approve | approves the time entry |
| `vendorpayment` | confirm | confirms the payment |
| `invoicegroup` | group invoices | groups invoices |
| `supplyplandefinition` | launch | launches the plan |
| `revenuearrangement` | allocate | runs the allocation |

The source's list of charge actions also contains entries that are really budget macros.

> [!SOURCE-ERROR] The vendor's action list files budget macros under the charge record's actions.
> Do not treat those entries as charge actions; check what a charge record actually offers with
> `action.find()` in your account.
<!-- source-error: topic:record_actions_macros | - | budget_macros_listed_as_charge_actions -->

The source also places a recalculate-override entry under a revenue event, where it does not appear to
belong.

> [!SOURCE-ERROR] A recalculate-override entry is listed under the revenue event, apparently in the
> wrong place. Do not rely on it being a revenue event action; check the actions available on the
> record at run time.
<!-- source-error: topic:record_actions_macros | - | recalculate_override_listed_under_revenue_event -->

For the revenue event, the source gives an action ID that repeats the invoice group's
remove-invoices-from-group ID.

> [!SOURCE-ERROR] The revenue event's action ID is shown as the same ID used to remove invoices from an
> invoice group, which looks copied. The source does not give the correct ID; look it up with
> `action.find()` before using it.
<!-- source-error: topic:record_actions_macros | - | revenue_event_action_id_repeats_removeinvoicesfromgroup -->

The invoice action that unmarks an invoice for grouping appears in this list but is not listed
anywhere else in the source.

## Record types with macros

| Record | Macro | What it does |
|---|---|---|
| `billingratecard` | `modifyPriceByPercent` | recalculates prices by a percentage |
| `chargerule` | `copyResources` | copies resources from tasks |
| `wbs` (work breakdown structure) | `getAmountFieldValue`, `getAmountsFields`, `getUnmatchedActuals`, `setAmountFieldValue` | the source does not describe these beyond their names |
| `salesorder` | `autoAssignLocations` | assigns locations automatically |
| transactions | `calculateTax`, `getSummaryTaxTotals` | `calculateTax` previews tax; the source does not describe `getSummaryTaxTotals` beyond its name (see [Transaction records](transactions.md)) |
| `timesheet` | `checkTimeLimits`, `copyFromWeek` | checks time limits; copies entries from a week |

## Example

Approve a batch of time entries passed in through a script parameter. Actions skip role and
permission checks, so a real script should itself decide which entries it may approve.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType ScheduledScript
 */
define(['N/action', 'N/runtime', 'N/log'], (action, runtime, log) => {
  const execute = () => {
    const raw = runtime.getCurrentScript().getParameter({ name: 'custscript_timebills_to_approve' });
    const ids = String(raw || '').split(',').filter(Boolean);

    ids.forEach((id) => {
      try {
        const outcome = action.execute({ recordType: 'timebill', id: 'approve', params: { recordId: Number(id) } });
        log.debug({ title: `Time entry ${id}`, details: JSON.stringify(outcome.notifications) });
      } catch (e) {
        log.error({ title: `Time entry ${id} not approved`, details: e.name });
      }
    });
  };

  return { execute };
});
```
