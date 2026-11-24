# SuiteFlow workflow initiation
<!-- id: topic:suiteflow.initiation -->

Part of: [SuiteFlow workflows](suiteflow.md)

Related: [SuiteFlow server triggers](suiteflow.server_triggers.md),
[SuiteFlow trigger execution order](suiteflow.trigger_execution.md),
[SuiteFlow action types](suiteflow.actions.md), [N/workflow](../modules/N_workflow.md),
[UserEventScript](../script-types/UserEventScript.md).

A workflow instance can be started in four ways:

- **event-based**: something happens to a record (it is created, edited, viewed, approved, ...);
- **scheduled**: the workflow runs over the results of a saved search;
- **from SuiteScript**;
- **from another workflow**, through the Initiate Workflow action.

Every initiation produces a new, separate instance, so the same workflow can be running in parallel on
different records. A workflow cannot be initiated until it has at least one state.

## Event-based initiation

### Settings

- **Initiation**: On Create, On Update, or both. On Update also fires when a record is merely viewed.
- **Trigger**: All, Before Record Load, Before Record Submit or After Record Submit.
- **Filters** that narrow when the workflow starts: event types, execution contexts, a condition,
  and a saved search.

If the initiation saved search is on the same record type as the workflow, it must include a criteria
filter.

Which trigger to pick:

| Trigger | Good for |
|---|---|
| Before Record Load | Changing the form or other parts of the user interface |
| Before Record Submit | Checking or adjusting the record before it is written |
| After Record Submit | Work that should only happen once the record has been committed |

If a user opens the same record several times in quick succession, a workflow that initiates on Before
Record Load can end up with several instances.

### Event types

The event types filter is used in three places: initiation, actions and transitions. The event types
it offers depend on the server trigger, the base record type and which features are enabled. The
field is not offered for actions on client triggers, nor for the Scheduled trigger.

Combining the View event type with On Create initiation never starts anything, because a view is
never a create.

The table shows, for each event type, the triggers on which it can initiate a workflow and whether it
counts as a create or an update:

| Event type | Triggers | Counts as |
|---|---|---|
| Approve | Before Record Submit, After Record Submit | Update |
| Cancel | Before Record Submit, After Record Submit | Update |
| Copy | Before Record Load | Create |
| Create | Before Record Load, Before Record Submit, After Record Submit | Create |
| Delete | Before Record Submit | Update |
| Direct List Edit | Before Record Submit, After Record Submit | Update |
| Drop Ship | After Record Submit | Create |
| Edit | Before Record Load, Before Record Submit, After Record Submit | Update |
| Edit Forecast | Before Record Submit | Update |
| Email | Before Record Load | Update |
| Mark Complete | Before Record Submit, After Record Submit | Update |
| Order Items | After Record Submit | Create |
| Pack | Before Record Submit, After Record Submit | Update |
| Pay Bills | After Record Submit | Create |
| Print | Before Record Load | Update |
| Reassign | Before Record Submit | Update |
| Reject | Before Record Submit, After Record Submit | Update |
| Ship | Before Record Submit, After Record Submit | Update |
| Special Order | After Record Submit | Create |
| View | Before Record Load | Update |

Several event types only occur on particular records:

| Event type | Where it occurs |
|---|---|
| Approve, Reject | Record types that go through approval |
| Mark Complete | Tasks and phone calls |
| Reassign | Cases, when a case is grabbed |
| Pay Bills | Vendor payments made on the Pay Bills page |
| Drop Ship | Purchase orders that carry drop-ship items |
| Special Order | Purchase orders that carry special-order items |

> [!SOURCE-ERROR] The source's scope for the Order Items event type is a copy of the Drop Ship wording
> (purchase orders with drop-ship items), so the real scope of Order Items is unknown here. Test on
> which records it fires before depending on it.
<!-- source-error: topic:suiteflow.initiation | - | order_items_limit_copied_from_drop_ship -->

> [!SOURCE-ERROR] The source says Pack and Ship apply to item records, yet the example it gives is an
> item fulfillment. Check which record types actually raise these events in your account.
<!-- source-error: topic:suiteflow.initiation | - | pack_ship_scoped_to_item_records_but_example_is_fulfillment -->

When a record is changed through Direct List Edit, the workflow sees only the fields that were
changed, not the whole record.

### Execution contexts

The contexts filter restricts initiation to record events that come from a given source. Contexts
that can, for instance, start a workflow On Create with After Record Submit include CSV import, a user
event script and a custom mass update. The user event script context only covers server-side user
events.

> [!SOURCE-ERROR] The source's custom mass update example initiates On Create only, which does not fit
> a mass update that edits existing records. Read it as a demonstration of the context filter and
> decide yourself whether you need On Update.
<!-- source-error: topic:suiteflow.initiation | - | mass_update_example_initiates_on_create_only -->

### Detecting changes with old values

A workflow can look at the values the record had before the current save. These "old record" values
are captured just before the first Before Record Submit processing and can be used on the Before
Record Submit and After Record Submit triggers.

In a formula, an old value is referenced with the placeholder `{old.field}`, where `field` stands for
the field ID you want; the source's illustration reads the previous priority's ID this way. Comparing
the old value with the current one is the standard way to tell whether a field was changed in this
save. Be aware that referencing the old record makes the record slower to load.

## Scheduled initiation

- Available frequencies: once, every 30 minutes, daily, weekly, monthly and yearly.
- The workflow runs over the results of a saved search and starts one instance per result. Every
  result counts, not just those on the first page of a summary.
- If that search is on the workflow's own record type, it must include a criteria filter.
- Scheduled initiation cannot have a condition; express the criteria in the search instead.
- Scheduled workflows run as administrator, and system notes show the user as System.
- No execution order is guaranteed between different scheduled workflows.
- The workflow's release status must be Released for scheduled initiation to happen.
- Using a saved search that a SuiteApp provides requires the SuiteApp Marketplace permission.

## Initiation from SuiteScript

The workflow has to exist in the Workflow Manager before a script can start it. The script then needs
three pieces of information:

- the workflow's internal ID or script ID;
- the script ID of the record type;
- the internal ID of the record.

In SuiteScript 2.x these are the `workflowId`, `recordType` and `recordId` options of
[`workflow.initiate`](../modules/N_workflow.md).

> [!SOURCE-ERROR] The source claims that starting a workflow from a script requires a workflow action
> script. That does not match the rest of the material, where workflow action scripts implement custom
> actions and `workflow.initiate` starts a workflow directly. Treat the requirement as doubtful and
> confirm it in your account.
<!-- source-error: topic:suiteflow.initiation | - | says_initiation_needs_workflow_action_script -->

### Example

Purchase orders created by an inbound integration should enter an approval workflow only once they are
saved. Instead of letting the workflow initiate itself on Before Record Load, an after-submit user
event starts it for newly created orders.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType UserEventScript
 */
define(['N/workflow', 'N/record', 'N/log'], (workflow, record, log) => {
  const afterSubmit = (context) => {
    if (context.type !== context.UserEventType.CREATE) {
      return;
    }

    const poId = context.newRecord.id;
    const instanceId = workflow.initiate({
      recordType: record.Type.PURCHASE_ORDER,
      recordId: poId,
      workflowId: 'customworkflow_po_dual_approval'
    });
    log.audit({ title: 'Approval started', details: `PO ${poId}, instance ${instanceId}` });
  };

  return { afterSubmit };
});
```
