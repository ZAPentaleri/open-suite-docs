# SuiteFlow form and field actions
<!-- id: topic:suiteflow.field_actions -->

Part of: [SuiteFlow workflows](suiteflow.md)

Related: [SuiteFlow action types](suiteflow.actions.md), [SuiteFlow client triggers](suiteflow.client_triggers.md),
[SuiteFlow server triggers](suiteflow.server_triggers.md).

This page covers the workflow actions that talk to the user, that change how a field is shown, or
that change what a field holds, plus two actions that reach beyond the current record (Subscribe To
Record and Send Campaign Email). Which triggers each action supports is listed on
[SuiteFlow action types](suiteflow.actions.md).

## Messages to the user

### Show Message

Pops up a message with an OK button. It is informational only: after dismissing it the user can go
on editing, because the action does not block further changes.

### Confirm

Asks the user to confirm before going on. It runs on two triggers, Before Field Edit and Before User
Submit, and the confirmation text is limited to 999 characters. Cancelling has a different effect on
each trigger:

| Trigger | Effect of Cancel |
|---|---|
| Before Field Edit | The field change is rolled back to the previous value |
| Before User Submit | The record is not saved |

### Return User Error

Reports an error to the user. The presentation depends on where the action runs:

| Trigger | What the user sees |
|---|---|
| Before Record Load | An error page with a Go Back link |
| Client triggers | A popup with an OK button |
| Before Record Submit | An error page with a Go Back link |

- Going back from the error page throws away any values the user entered but had not saved. For
  that reason, run validations on client triggers where possible, so the user sees a popup and keeps
  their input.
- On a time entry record, Return User Error is not available on the Before Record Load trigger.

## How fields are displayed

- **Set Field Display Label** renames fields on the record body and fields in sublists alike.
- **Set Field Display Type** puts a field into one of four display types: hidden, inline, disabled or
  normal.
- **Set Field Required** is not enforced on checkbox fields, so marking a checkbox as required has no
  effect.

## Setting field values

**Set Field Value** can take its value from four kinds of source: a static value, a date, another
field, or a formula. The formula language follows the trigger type: on client triggers the formula
is JavaScript/SuiteScript, on server triggers it is SQL.

- The record has to be in edit mode for the action to set a value.
- On Before Record Load the action cannot set values on a record that already exists; use a custom
  action for that case.
- A value set on Before Record Load does not replace a field value that is already stored or that
  comes from sourcing. Set it on a client trigger instead if it must win.
- Setting a multi-select field replaces its current selection instead of adding to it, and choosing
  multi-select values this way only works on new records.
- The action cannot set the external ID.

## Following referenced records

**Subscribe To Record** lets the workflow's conditions watch for changes on a record that the
current record refers to. The source pairs it with the Create Record action; it does not explain
further how the two are combined. On a time entry record, Subscribe To Record is not available on
the Before Record Load trigger.

## Campaign email

**Send Campaign Email** sends a marketing campaign email from the workflow. It needs the
`"Marketing Automation"` feature to be enabled (see [catalog:features](../gen/catalogs/features.md)).
Its parameters are a record, a field, the campaign event to send, and where to store the result. To
follow up on how recipients respond, use it together with a Subscribe To Record action.
