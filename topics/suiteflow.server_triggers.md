# SuiteFlow server triggers
<!-- id: topic:suiteflow.server_triggers -->

Part of: [SuiteFlow workflows](suiteflow.md)

Related: [Client triggers](suiteflow.client_triggers.md), [Trigger execution order](suiteflow.trigger_execution.md),
[Workflow initiation](suiteflow.initiation.md), [Action types](suiteflow.actions.md).

Workflow triggers come in two kinds: server triggers, which run in NetSuite as the record is loaded,
saved or scheduled, and [client triggers](suiteflow.client_triggers.md), which run in the browser
while the user edits. Server triggers fall into three classes: record-based, workflow-based and
time-based.

## The server triggers

| Trigger | When it fires |
|---|---|
| Before Record Load | On the server, after the user clicks to create, edit or view a record and before the page is shown |
| Before Record Submit | On the server, after the user clicks Save and before the record is written to the database |
| After Record Submit | On the server, after the record has been written to the database |
| Entry | The first time a record enters a state |
| Exit | The first time a record leaves a state |
| Scheduled | On the workflow scheduler, every 30 minutes |
| All | Initiation only: any record event, viewing included |

Where each one can be used:

| Used for | Triggers allowed |
|---|---|
| Initiating a workflow | All, Before Record Load, Before Record Submit, After Record Submit, Scheduled |
| Actions | Every server trigger except All |
| Transitions | Before Record Load, Before Record Submit, After Record Submit, Entry, Scheduled, or blank |

Rules that tie these together:

- A workflow can only be initiated by a server trigger.
- For any server trigger, a state runs its actions first and then its transitions.
- Client triggers cannot drive transitions.
- A transition with a blank trigger runs as soon as its conditions are met.

> [!SOURCE-ERROR] In the source's trigger table, the Before Record Submit row links to the page for the
> Before User Submit client trigger. The two are different triggers; the description on this page is the
> server-side Before Record Submit.
<!-- source-error: topic:suiteflow.server_triggers | - | before_record_submit_row_links_before_user_submit_page -->

## Choosing a trigger

- Before Record Load: changes to the form.
- Before Record Submit: validation and calculations before the save.
- After Record Submit: sending email and creating dependent records.

For initiation, prefer a specific trigger over All. Initiating on Before Record Load can start several
instances when the same record is opened repeatedly.

## Before Record Load

Typical jobs: adding or removing buttons, showing calculated values, locking the record, changing how
fields are displayed, and checking permissions or other conditions.

- Refreshing the browser runs the Before Record Load actions again.
- You cannot set field values on an existing record on this trigger; use a custom action instead.
- Compared with the Before User Edit client trigger: Before Record Load runs on the server every time
  the record loads, whereas Before User Edit runs in the browser just before the user starts editing.
  Changes that Before User Edit makes are lost unless the record is saved.
- When a workflow is initiated on All or on Before Record Load and the user clicks Cancel, the record
  reloads, which can start the on-update workflow again. Initiate on Before Record Submit or After
  Record Submit to avoid this.

## Before Record Submit

Typical jobs: validating data before it is written, permission checks, and working out field values.

- If the workflow is initiated on Before Record Submit, its Before Record Load actions never run;
  the entry actions run instead.
- A Return User Error action on this trigger sends the user to an error page, and what they typed is
  lost. Validate on the Before User Submit client trigger when you want to keep their input.

## After Record Submit

Typical jobs: sending email, redirecting, creating dependent records, and acting on values that only
exist after the save.

- The record is reloaded, the actions run, and it is saved again if anything changed.
- System notes for those changes show the Administrator role.

### Before Record Submit or After Record Submit?

For actions that change the record:

| Before Record Submit | After Record Submit |
|---|---|
| One save, so better performance | May cause a second save |
| A failure cancels the user's edits | The user's data is already saved |
| Runs before the record's own business logic | The business logic has already run |

When the action needs the record to be consistent, use Before Record Submit.

## Entry and Exit

- Entry fires the first time the record enters the state, together with the first server trigger that
  happens in that state.
- Exit fires the first time the record leaves the state.
- Neither supports the Confirm or Show Message actions.
- An entry action only runs if its own trigger is valid for the server trigger on which the state was
  entered. For example, a Send Email action set to Entry is skipped when the state is entered on
  Before Record Load.
- Likewise, an exit action needs a trigger that is valid for the server trigger on which the record
  leaves. For example, a Lock Record action set to Exit is skipped when the record leaves on After
  Record Submit.
- You choose the server trigger on which a state is entered through the trigger of the transition into
  it.
- The exit of the source state and the entry of the target state happen on the same server trigger,
  so each runs once.

## Scheduled

- The scheduler checks every 30 minutes. Scheduled work can run later than planned, never earlier.
- For scheduled initiation, every result of the chosen saved search gets its own workflow instance.
- The schedule's time zone is that of the user who created it, as of the moment it was saved.
- The workflow's release status must be Released.
- Scheduled actions never run in an exit state.
