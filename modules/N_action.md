# N/action
<!-- id: N/action -->

`N/action` lets a script run the same record actions a user would trigger by pressing a button on a
record page (for example, approving a time entry). Running an action has the same effect as clicking
that button, and whatever the action changes is saved straight away: there is no separate save
step. A time bill approved through this module, for instance, is stored as approved as soon as the
call completes.

Points to keep in mind:

- Only the record actions the platform lists as supported can be run this way; you cannot invoke
  arbitrary buttons.
- The module is meant for records in view mode. When you work with a record open in edit mode, use
  the record macro APIs instead (see [Record actions and macros](../topics/record_actions_macros.md)).
- Usage cost is said to depend on which action you run and on the record type it runs against;
  see the note below about how that fits with the per-member figures.

> [!SOURCE-ERROR] The source says the module's governance varies by action and record type, yet its
> individual members each carry a fixed figure (0 units for most calls, 50 for the bulk calls). The
> spec does not reconcile the two; treat the per-member figures with caution and measure the actual
> usage of the actions you run.
<!-- source-error: N/action | - | module_governance_varies_vs_member_governance_none -->

- **Availability:** client and server scripts. Promise variants are client-side only.
- **Since:** the source does not state a version for the module itself; its members date from
  2018.2 (bulk execution from 2019.1).
- **Load it as:** `define(['N/action'], (action) => { ... })`
- Related: [N/record](N_record.md), [N/task](N_task.md) (`task.RecordActionTaskStatus`).

## Members

| Member | Kind | Summary |
|---|---|---|
| [`action.get(options)`](#actiongetoptions) | function | Fetch one action for a record type |
| [`action.get.promise(options)`](#actiongetpromiseoptions) | function | Promise form of `get` |
| [`action.find(options)`](#actionfindoptions) | function | List the actions for a record type or record |
| [`action.find.promise(options)`](#actionfindpromiseoptions) | function | Promise form of `find` |
| [`action.execute(options)`](#actionexecuteoptions) | function | Run one action on one record |
| [`action.execute.promise(options)`](#actionexecutepromiseoptions) | function | Promise form of `execute` |
| [`action.executeBulk(options)`](#actionexecutebulkoptions) | function | Run one action on many records asynchronously |
| [`action.getBulkStatus(options)`](#actiongetbulkstatusoptions) | function | Check the progress of a bulk run |
| [`Action`](#action-object) | object | A single action, ready to execute |

## Functions

### action.get(options)
<!-- id: N/action.get -->

Looks up one action by its ID for a given record type and returns it as an executable
[`Action`](#action-object). If you also pass a record ID, you only get the action back when it can
actually be run on that particular record; the returned object is then tied ("qualified") to that
record.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.recordType` | string | Yes | |
| `options.recordId` | string | No | |
| `options.id` | string | Yes | |

- `options.id`: the action's ID.
- `options.recordId`: when given, the action is returned only if it is executable on that record.

**Returns:** `action.Action`, an action object you can run.

**Errors:**

- `RECORD_DOES_NOT_EXIST`: the record could not be found.
- `SSS_INVALID_ACTION_ID`: the action does not exist, or cannot be run on the given record.
- `SSS_INVALID_RECORD_TYPE`: the record type is not valid.
- `SSS_MISSING_REQD_ARGUMENT`: a required argument was left out.

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2018.2

### action.get.promise(options)
<!-- id: N/action.get.promise -->

Asynchronous form of [`action.get(options)`](#actiongetoptions) for client scripts. It takes the
same parameters and raises the same errors, but returns a Promise.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.recordType` | string | Yes | |
| `options.recordId` | string | No | |
| `options.id` | string | Yes | |

**Returns:** `Promise`, settling with the action object.

**Errors:**

- `RECORD_DOES_NOT_EXIST`: the record could not be found.
- `SSS_INVALID_ACTION_ID`: the action could not be found.
- `SSS_INVALID_RECORD_TYPE`: the record type argument is wrong.
- `SSS_MISSING_REQD_ARGUMENT`: a required argument was left out.

**Governance:** 0 units.

**Availability:** client scripts.

**Since:** 2018.2

### action.find(options)
<!-- id: N/action.find -->

Discovers actions. What you get back depends on how much you specify:

- record type only: every action defined for that record type;
- record type plus record ID: only the actions that qualify for that record;
- with an action ID as well: just that one action.

Actions found with a record ID are qualified, meaning they are bound to that record and can be
executed without repeating its ID. When nothing matches, the result is an empty object rather than
an error.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.recordType` | string | Yes | |
| `options.recordId` | string | No | |
| `options.id` | string | No | |

**Returns:** `Object`, a plain object holding [`Action`](#action-object) objects (empty when there
are none). The source does not state how the entries are keyed, so iterate over the values rather
than relying on particular keys.

**Errors:**

- `RECORD_DOES_NOT_EXIST`: the record could not be found.
- `SSS_INVALID_ACTION_ID`: the action does not exist, or cannot be run on the given record.
- `SSS_INVALID_RECORD_TYPE`: the record type is not valid.
- `SSS_MISSING_REQD_ARGUMENT`: `options.recordType` was not supplied.

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2018.2

### action.find.promise(options)
<!-- id: N/action.find.promise -->

Asynchronous form of [`action.find(options)`](#actionfindoptions) for client scripts, with the same
parameters and errors.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.recordType` | string | Yes | |
| `options.recordId` | string | No | |
| `options.id` | string | No | |

**Returns:** `Promise`, settling with the object of actions.

**Errors:**

- `RECORD_DOES_NOT_EXIST`: the record could not be found.
- `SSS_INVALID_ACTION_ID`: the action could not be found.
- `SSS_INVALID_RECORD_TYPE`: the record type argument is wrong.
- `SSS_MISSING_REQD_ARGUMENT`: `options.recordType` was not supplied.

**Governance:** 0 units.

**Availability:** client scripts.

**Since:** 2018.2

### action.execute(options)
<!-- id: N/action.execute -->

Runs one action against one record without first fetching an `Action` object. Changes made by the
action are saved immediately.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.recordType` | string | Yes | |
| `options.id` | string | Yes | |
| `options.params` | Object | Yes | |
| `options.params.recordId` | int | Yes | |

- `options.id`: the action's ID.
- `options.params`: the action's parameters; `recordId` names the record to act on.

**Returns:** `Object` with two parts: the action's own result (the response), and a list of
notifications (errors and warnings) that is normally empty when the action succeeds.

**Errors:**

- `RECORD_DOES_NOT_EXIST`: the record instance named by the record ID does not exist.
- `SSS_INVALID_ACTION_ID`: the action does not exist, or cannot be run on that record.
- `SSS_INVALID_RECORD_TYPE`: the record type is not valid.
- `SSS_MISSING_REQD_ARGUMENT`: a required argument was left out.

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2018.2

### action.execute.promise(options)
<!-- id: N/action.execute.promise -->

Asynchronous form of [`action.execute(options)`](#actionexecuteoptions) for client scripts. It
accepts the same parameters and raises the same errors.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.recordType` | string | Yes | |
| `options.id` | string | Yes | |
| `options.params` | Object | Yes | |
| `options.params.recordId` | string | Yes | |

> [!SOURCE-ERROR] The source types `options.params.recordId` as a string here but as an integer on
> `action.execute`. The spec does not say which is correct; test with the form you plan to pass.
<!-- source-error: N/action.execute.promise | - | recordid_typed_string_vs_int -->

**Returns:** `Promise`, settling with the same result object as `execute`.

**Errors:**

- `RECORD_DOES_NOT_EXIST`: the record could not be found.
- `SSS_INVALID_ACTION_ID`: the action could not be found.
- `SSS_INVALID_RECORD_TYPE`: the record type argument is wrong.
- `SSS_MISSING_REQD_ARGUMENT`: a required argument was left out.

**Governance:** 0 units.

**Availability:** client scripts.

**Since:** 2018.2

### action.executeBulk(options)
<!-- id: N/action.executeBulk -->

Runs one action over many records asynchronously and returns the ID of the bulk task straight away;
pass that ID to [`action.getBulkStatus(options)`](#actiongetbulkstatusoptions) to follow progress.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.recordType` | string | Yes | |
| `options.id` | string | Yes | |
| `options.params` | array | Yes | |
| `options.condition` | string | No | |
| `options.paramCallback` | string | No | |

- `options.id`: the action's ID.
- `options.params`: one parameter object per target record. Every one of these objects must carry
  its own `recordId`.
- `options.condition`: selects the records by condition instead. The only value described is
  `ALL_QUALIFIED_INSTANCES` (every record the action qualifies for), and it works only when the
  action implements a way to find its qualifying instances.
- `options.paramCallback`: an alternative way to build each record's parameters (typed `string`
  in the spec).
- `options.params`, `options.condition` and `options.paramCallback` are mutually exclusive: use
  one of them to say which records to process.

> [!SOURCE-ERROR] The source marks `options.params` as required but also says it cannot be combined
> with `options.condition` or `options.paramCallback`, so a call using a condition would break the
> "required" rule. The spec does not say which statement wins; if you use `condition`, test whether
> the call is accepted without `params`.
<!-- source-error: N/action.executeBulk | - | params_required_but_mutually_exclusive_with_condition -->

> [!SOURCE-ERROR] `options.paramCallback` is typed as a string, but the source describes it in one
> place as a function and in another as the name of a function. The spec does not settle which form
> is accepted; verify before relying on either.
<!-- source-error: N/action.executeBulk | options.paramCallback | typed_string_but_described_as_function_vs_function_name -->

**Returns:** `string`, the ID of the bulk task.

**Errors:**

- `SSS_INVALID_ACTION_ID`: the action does not exist, or cannot be run on the records.
- `SSS_INVALID_RECORD_TYPE`: the record type is not valid.
- `SSS_MISSING_REQD_ARGUMENT`: `options.recordType` was not supplied.

**Governance:** 50 units.

**Availability:** client and server scripts.

**Since:** 2019.1

### action.getBulkStatus(options)
<!-- id: N/action.getBulkStatus -->

Reports how far a bulk execution started by `executeBulk` has got.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.taskId` | string | Yes | |

- `options.taskId`: the string returned by `executeBulk`.

**Returns:** `task.RecordActionTaskStatus`, a status object from [N/task](N_task.md) describing the
bulk run.

**Errors:** the source does not list any for this function.

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2019.1

## Objects

### Action (object)
<!-- id: N/action.Action -->

Represents one record action. You receive it from [`action.get`](#actiongetoptions) or
[`action.find`](#actionfindoptions). An action obtained with a record ID is qualified: it already
knows which record it belongs to. Available in client and server scripts since 2018.2.

Three rules apply to every way of running an `Action` (`execute`, its promise form, `promise`, and
calling the object directly):

- The parameters you can pass depend on how the particular action is implemented.
- A record ID is required unless the action is already qualified (obtained through `get` or `find`
  with a record ID).
- A record ID you pass when executing takes priority over the one stored in the action.

#### new Action(options)
<!-- id: N/action.Action#constructor -->

The spec's "constructor" is not a `new` call: the action object itself is invoked as a function
with an options object, and doing so executes the action, exactly like
[`Action.execute`](#actionexecuteoptions-1). The name `Action` here stands for the real action's
name. For example, an action fetched into a variable `approve` can be run as `approve(options)`.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | No | |
| `options.Object` | Object | No | |
| `options.params` | Object | No | |
| `options.params.recordId` | string | No | |

- `options.Object` and `options.params.recordId` are conditionally required; see the rules under
  [Action](#action-object).

> [!SOURCE-ERROR] Two problems in the source's description. First, its parameter table has a row
> named `options.Object` [sic], which reads like a placeholder for the action-specific parameter
> object rather than a real property. Second, a note on the same page puts the record ID at a
> different path than the table does (the table nests it as `options.params.recordId`). The spec
> keeps both as given; follow the table and verify with your action.
<!-- source-error: N/action.Action#constructor | - | param_row_named_options_Object -->
<!-- source-error: N/action.Action#constructor | - | note_recordid_path_differs_from_param_table -->

**Returns:** `Object`: the action's result (response) plus notifications (errors and warnings),
normally empty on success.

**Errors:**

- `SSS_MISSING_REQD_ARGUMENT`: a required argument was left out.

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2018.2

#### Action.execute(options)
<!-- id: N/action.Action#execute -->

Runs this action. The arguments you can pass are defined by the action's own implementation.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | No | |
| `options.Object` | Object | No | |
| `options.params` | Object | No | |
| `options.params.recordId` | int | No | |

- `options.Object` and `options.params.recordId` are conditionally required.
- `options.params.recordId` is required unless the action is already qualified because it came
  from `get` or `find` called with a record ID. If you pass it anyway, it overrides the record ID
  stored in the action.

> [!SOURCE-ERROR] Three problems in the source here. It types `recordId` as an integer for this
> method but as a string in the promise and constructor forms; it names a parameter row
> `options.Object` [sic], which looks like a placeholder rather than a real property; and a note
> places the record ID at a different path than the parameter table. The spec does not settle any
> of these; follow the table and verify with the action and type you use.
<!-- source-error: N/action.Action#execute | - | recordid_typed_int_but_string_in_variants -->
<!-- source-error: N/action.Action#execute | - | note_recordid_path_differs_from_param_table -->
<!-- source-error: N/action.Action#execute | - | param_row_named_options_Object -->

**Returns:** `Object` with the action's result (response) and a notifications list (errors and
warnings) that is usually empty when the action succeeds.

**Errors:**

- `SSS_MISSING_REQD_ARGUMENT`: a required argument was left out.

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2018.2

#### Action.execute.promise(options)
<!-- id: N/action.Action#execute.promise -->

Asynchronous form of [`Action.execute`](#actionexecuteoptions-1) for client scripts. The same rules
apply: parameters depend on the action, `recordId` is needed unless the action is qualified, and a
`recordId` you pass wins over the stored one.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | No | |
| `options.Object` | Object | No | |
| `options.params` | Object | No | |
| `options.params.recordId` | string | No | |

- `options.Object` and `options.params.recordId` are conditionally required.

> [!SOURCE-ERROR] The source's parameter table has a row named `options.Object` [sic], which looks
> like a placeholder for the action-specific parameters rather than a real property name. Pass the
> action's own parameters under `options.params`, and verify with your action.
<!-- source-error: N/action.Action#execute.promise | - | param_row_named_options_Object -->

**Returns:** `Promise`, settling with the result object (action result plus notifications, usually
empty on success).

**Errors:**

- `SSS_MISSING_REQD_ARGUMENT`: a required argument was left out.

**Governance:** 0 units.

**Availability:** client scripts.

**Since:** 2018.2

#### Action.promise(options)
<!-- id: N/action.Action#promise -->

The promise form of calling the action object directly (see
[`new Action(options)`](#new-actionoptions)): it runs the action and returns a Promise. It follows
the same rules as the other ways of running an action.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | No | |
| `options.Object` | Object | No | |
| `options.params` | Object | No | |
| `options.params.recordId` | string | No | |

- `options.Object` and `options.params.recordId` are conditionally required.
- Parameters depend on the action's implementation; `recordId` is needed unless the action is
  qualified, and a passed `recordId` overrides the stored one.

> [!SOURCE-ERROR] As on the other execution methods, the source's table has a row named
> `options.Object` [sic], which looks like a placeholder, and a note gives the record ID a different
> path than the table. Follow the table (`options.params.recordId`) and verify with your action.
<!-- source-error: N/action.Action#promise | - | param_row_named_options_Object -->
<!-- source-error: N/action.Action#promise | - | note_recordid_path_differs_from_param_table -->

**Returns:** `Promise`, settling with the action result plus notifications (usually empty on
success).

**Errors:**

- `SSS_MISSING_REQD_ARGUMENT`: a required argument was left out.

**Governance:** 0 units.

**Availability:** client scripts.

**Since:** 2018.2

#### Action.executeBulk(options)
<!-- id: N/action.Action#executeBulk -->

Starts an asynchronous run of this action over many records and returns the bulk task ID, which
you can pass to [`action.getBulkStatus`](#actiongetbulkstatusoptions).

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.params` | array | Yes | |
| `options.condition` | string | No | |
| `options.paramCallback` | string | No | |

- `options.params`, `options.condition` and `options.paramCallback` are mutually exclusive: pick
  one way to describe the target records.
- `options.condition`: the only value described is `ALL_QUALIFIED_INSTANCES`. It works only if the
  action implements a way to find the records it qualifies for.
- `options.paramCallback`: the name of a function that receives a record ID and returns the
  parameter object for that record.
- A record ID is needed for each target unless the action is already qualified via `get` or `find`.

> [!SOURCE-ERROR] The source marks `options.params` as required but also makes it mutually
> exclusive with `options.condition` and `options.paramCallback`. The spec does not resolve this;
> if you select records by condition, test whether the call is accepted without `params`.
<!-- source-error: N/action.Action#executeBulk | - | params_required_but_mutually_exclusive_with_condition -->

**Returns:** `string`, the bulk task ID.

**Errors:**

- `SSS_INVALID_ACTION_ID`: the action does not exist or cannot be run on the record.
- `SSS_INVALID_RECORD_TYPE`: the record type is not valid.
- `SSS_MISSING_REQD_ARGUMENT`: a required argument was left out.

**Governance:** 50 units.

**Availability:** client and server scripts.

**Since:** 2019.1

#### Action.id
<!-- id: N/action.Action#id -->

`string`. The action's ID. Since 2018.2.

#### Action.recordType
<!-- id: N/action.Action#recordType -->

`string`. The record type the action belongs to; values come from
[`record.Type`](N_record.md). Since 2018.2.

#### Action.label
<!-- id: N/action.Action#label -->

`string`. The action's label. Since 2018.2.

#### Action.description
<!-- id: N/action.Action#description -->

`string`. A description of the action. Since 2018.2.

#### Action.parameters
<!-- id: N/action.Action#parameters -->

`Object`. The action's parameters. The source does not describe the shape of this object.
Since 2018.2.

## Examples

A scheduled script that approves a batch of time entries. Each approval is saved immediately, so
there is no `record.save` call. The action ID `approve` is illustrative; look up the real ID with
`action.find` for your record type.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType ScheduledScript
 */
define(['N/action', 'N/record', 'N/log'], (action, record, log) => {
  const approveEntries = (timeBillIds) => {
    timeBillIds.forEach((timeBillId) => {
      try {
        const outcome = action.execute({
          recordType: record.Type.TIME_BILL,
          id: 'approve',
          params: { recordId: timeBillId }
        });
        if (outcome.notifications && outcome.notifications.length) {
          log.audit({ title: `Time bill ${timeBillId}`, details: JSON.stringify(outcome.notifications) });
        }
      } catch (e) {
        log.error({ title: `Could not approve ${timeBillId}`, details: e.name });
      }
    });
  };

  const execute = () => {
    approveEntries([4410, 4411, 4415]);
  };

  return { execute };
});
```

A client script that asks which actions qualify for the open purchase order and runs one of them
asynchronously when it is on offer. Because the actions were found with a record ID, they are
qualified and need no `recordId` when executed.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType ClientScript
 */
define(['N/action', 'N/record', 'N/currentRecord'], (action, record, currentRecord) => {
  const pageInit = () => {};

  const runIfAvailable = (actionId) => {
    const po = currentRecord.get();
    return action.find.promise({ recordType: record.Type.PURCHASE_ORDER, recordId: String(po.id) })
      .then((available) => {
        const match = Object.values(available).find((candidate) => candidate.id === actionId);
        if (!match) {
          return `Action ${actionId} is not available on this order.`;
        }
        return match.execute.promise().then(() => `${match.label} done.`);
      });
  };

  return { pageInit, runIfAvailable };
});
```

Starting a bulk run over a week's worth of time entries and checking on it later:

```js
/**
 * @NApiVersion 2.1
 * @NScriptType ScheduledScript
 */
define(['N/action', 'N/record', 'N/log'], (action, record, log) => {
  const execute = () => {
    const approve = action.get({ recordType: record.Type.TIME_BILL, id: 'approve' });
    const taskId = approve.executeBulk({
      params: [{ recordId: 9021 }, { recordId: 9022 }, { recordId: 9030 }]
    });
    const status = action.getBulkStatus({ taskId });
    log.debug({ title: 'Bulk approval queued', details: `${taskId}: ${status.status}` });
  };

  return { execute };
});
```
