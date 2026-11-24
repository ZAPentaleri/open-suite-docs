# SuiteFlow action types
<!-- id: topic:suiteflow.actions -->

Part of: [SuiteFlow workflows](suiteflow.md)

Related: [SuiteFlow form and field actions](suiteflow.field_actions.md), [SuiteFlow email action](suiteflow.send_email.md),
[SuiteFlow server triggers](suiteflow.server_triggers.md), [SuiteFlow client triggers](suiteflow.client_triggers.md),
[WorkflowActionScript](../script-types/WorkflowActionScript.md), [N/record](../modules/N_record.md).

An action is the unit of work a workflow performs while a record is in a given state. This page
lists every action type with the triggers it can run on, then goes through the actions that manage
buttons, create or open records, and start other workflows. Actions that talk to the user or change
fields are explained on [SuiteFlow form and field actions](suiteflow.field_actions.md); the email
action has [its own page](suiteflow.send_email.md).

## Action types and their triggers

Abbreviations used below: BRL = Before Record Load, BRS = Before Record Submit, ARS = After Record
Submit, BUE = Before User Edit, BFE = Before Field Edit, AFE = After Field Edit, AFS = After Field
Sourcing, BUS = Before User Submit. "Entry/Exit" means running when the record enters or leaves the
state.

| Action | Triggers it accepts | Trigger chosen by default |
|---|---|---|
| Add Button | Entry/Exit, BRL | BRL |
| Confirm | BFE, BUS | BUS |
| Create Line | Entry/Exit, BRL, BRS, ARS, Scheduled | Entry/Exit |
| Create Record | Entry/Exit, BRL, BRS, ARS, Scheduled | Entry/Exit |
| Go To Page | Entry/Exit, ARS | ARS |
| Go To Record | Entry/Exit, ARS | ARS |
| Initiate Workflow | Entry/Exit, BRL, BRS, ARS, Scheduled | Entry/Exit |
| Lock Record | Entry/Exit, BRL | BRL |
| Remove Button | Entry/Exit, BRL | Entry/Exit (but see the Remove Button section) |
| Return User Error | Entry/Exit, BRL, BRS, BUE, BFE, AFE, AFS, BUS | BRS |
| Send Campaign Email | Entry/Exit, ARS, Scheduled | Entry/Exit |
| Send Email | Entry/Exit, ARS, Scheduled | Entry/Exit |
| Set Field Display Label | Entry/Exit, BRL, BUE, BFE, AFE, AFS | BRL |
| Set Field Display Type | Entry/Exit, BRL, BUE, BFE, AFE, AFS | BRL |
| Set Field Required | Entry/Exit, BRL, BUE, BFE, AFE, AFS | BRL |
| Set Field Value | Entry/Exit, BRL, BRS, ARS, BUE, BFE, AFE, AFS, BUS, Scheduled | Entry/Exit |
| Show Message | BUE, BFE, AFE, AFS, BUS | BUE |
| Subscribe To Record | Entry/Exit, BRL, BRS, ARS, Scheduled | Entry/Exit |
| Transform Record | Entry/Exit, ARS, Scheduled | The source does not state a default |
| Custom (a workflow action script) | Entry/Exit, BRL, BRS, ARS, Scheduled | Entry/Exit |

The source additionally lists an **Ask Oracle** action. It is not available to HIPAA customers on
NetSuite Next; the source gives no triggers or parameters for it.

> [!SOURCE-ERROR] In one of the source's trigger tables the Add Button / After Record Submit cell
> holds the letter "C", which the table's legend never explains, and the action-group table shows
> that combination as unsupported. Rely on the trigger list above (Entry/Exit and Before Record Load).
<!-- source-error: topic:suiteflow.actions | - | add_button_after_record_submit_cell_c_undefined -->

## Action groups

An action group bundles several actions, and it can only run on server-side triggers: Entry/Exit,
Before Record Load, Before Record Submit, After Record Submit and Scheduled. Client triggers cannot be
used inside a group. A sublist action group (one that works through sublist lines) can hold only
these action types: Create Record, Return User Error, Send Email and Set Field Value.

## Line and record actions

### Create Line

Appends a line to a transaction's Items sublist, or to the Expenses sublist of an expense report.
You choose whether the line is inserted after the last existing line or ahead of the first one.

### Create Record

You give it the type of record to create, an optional workflow field to store the result in, and the
field values for the new record.

- All fields that the new record requires must receive a value; if one is missing, the action fails.
- Storing the result (the new record's ID) is what lets a later Subscribe To Record action watch the
  new record.
- On time entry records, this action cannot run on Before Record Load.

### Transform Record

Its parameters are the target record type, a field to store the result in, an option to redirect to
the transformed record without saving it, and field values for the new record. It does the same job
as [`record.transform`](../modules/N_record.md) in a script.

- The workflow keeps the same base record type after the transform runs.
- On time entry records it is not available on Before Record Submit or After Record Submit.
- For item fulfillments that span subsidiaries, the source recommends doing the transform in
  SuiteScript, where the `inventorylocation` parameter is available, rather than with this action.
- The source does not state which trigger the action selects by default.

## Navigation actions

### Go To Page

Sends the user to a NetSuite page, a list view, or a report. On time entry records it is not
available on Before Record Submit or After Record Submit.

### Go To Record

Its parameters are the record type, whether the record should open in edit mode, a join field that
points to the record to open, and field values.

- The field values are passed on the URL as parameters shaped like `record.{field}={value}`; the
  effect is the same as a before-load script setting those values on the form.
- On Before Record Load it cannot change fields of a record that already exists. Change the values
  with a custom action first and then redirect.
- On time entry records it is not available on Before Record Submit or After Record Submit.

## Button actions

### Add Button

Places a button on the record. Typical reasons are to let the user fire a transition, to navigate
somewhere, or to drive mass processing.

Its parameters are the button **label**, **Save Record First**, and **Check Condition Before
Execution**. With Save Record First set, the record's Save button also offers a "Save & <label>"
choice.

- For a button that must appear every time the record is opened, run the action on Before Record
  Load, not on Entry.
- A button added on Before Record Load can still show up after a transition has already moved the
  record out of the situation it was meant for. Add a condition to the action to hide it.
- While a record is being created, workflow buttons are disabled on the new-record form.
- After a save performed through such a button, the system notes record Administrator as the user.
- Double clicks and rapid sequences of button clicks are processed one after another rather than in
  parallel.

### Remove Button

Removes a button, standard or custom, identified by its button ID (standard button IDs are listed in
[catalog:buttons](../gen/catalogs/buttons.md)). It cannot remove a button that an Add Button action
put there; to get rid of that one, inactivate the Add Button action. The recommended trigger for
removing buttons is Before Record Load.

> [!SOURCE-ERROR] The source's page for this action names Before Record Load as the default trigger,
> while its quick-reference table names Entry. The spec keeps Entry/Exit as the default. Do not rely on
> either default: set the trigger explicitly when you add the action.
<!-- source-error: topic:suiteflow.actions | - | default_trigger_brl_here_entry_in_quick_reference -->

## Workflow control actions

### Initiate Workflow

Starts a child workflow. Its parameters are the workflow to start and field values for the child
workflow's own fields.

- The child workflow must be active and must have the same base record type; it runs against the same
  record instance as the parent.
- The parent can pause until the child finishes by using a Wait For Workflow transition.
- On time entry records it is not available on Before Record Load.

### Lock Record

Takes the Edit button away for as long as the record stays in the state.

- A script that loads a locked record receives an error.
- List pages still offer an Edit link for the record, so the lock is not watertight.
- On time entry records it is not available on Before Record Load.

## Custom actions

A custom action is one you write yourself as a
[workflow action script](../script-types/WorkflowActionScript.md).

- The script must be deployed for the workflow's base record type; once it is, it is offered as a
  custom action when you add a new action.
- A deployment that applies to all records makes the action available to every workflow.
- Its parameters are a field to store the script's result in, plus one field for each script
  parameter the script defines.
- If the user who started the workflow is outside the script deployment's audience, the action is
  skipped, and the skip is recorded in the workflow execution log.
