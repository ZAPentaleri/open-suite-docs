# SuiteFlow workflows
<!-- id: topic:suiteflow -->

Sub-topics:

- [SuiteFlow server triggers](suiteflow.server_triggers.md)
- [SuiteFlow client triggers](suiteflow.client_triggers.md)
- [SuiteFlow trigger execution order](suiteflow.trigger_execution.md)
- [SuiteFlow workflow initiation](suiteflow.initiation.md)
- [SuiteFlow action types](suiteflow.actions.md)
- [SuiteFlow form and field actions](suiteflow.field_actions.md)
- [SuiteFlow email action](suiteflow.send_email.md)

See also: [N/workflow](../modules/N_workflow.md), [WorkflowActionScript](../script-types/WorkflowActionScript.md),
[N/runtime](../modules/N_runtime.md).

SuiteFlow is NetSuite's point-and-click workflow engine. Scripts meet it in two ways: a script can
start or advance a workflow through [N/workflow](../modules/N_workflow.md), and a
[workflow action script](../script-types/WorkflowActionScript.md) can supply a custom action that a
workflow runs. Understanding how workflows behave matters when both touch the same record.

## Building blocks

A workflow is made of:

| Element | Role |
|---|---|
| Base record | The record type the workflow runs on |
| States | Stages the record moves through |
| Triggers | Record or time events that make actions and transitions run |
| Actions | What happens inside a state |
| Transitions | Moves from one state to another |
| Conditions | Tests that gate initiation, actions and transitions |
| Custom fields | Values stored for the workflow, a state, or the definition |
| Revisions | A version number of the workflow definition |

### Base record

- The base record is a standard record type or a custom record type.
- It cannot be changed once the workflow is saved.
- For customer, item and transaction base records you also pick a sub-type (for customers: lead,
  prospect or customer).
- To reach the workflow list for a record type, a user needs full permission on that base record.

### States

- Every workflow has at least one state.
- The first state, marked with the start-state property, is where a new instance begins.
- A state with no outgoing transitions is an end state; a workflow may have several.
- Inside a state, actions run in the order they are listed, and transitions are evaluated after the
  actions.

### Conditions

Conditions are written either with the condition builder or as a formula, and they can be attached to
workflow initiation, to actions and to transitions.

### Custom fields

| Kind | Scope |
|---|---|
| Workflow field | One value per workflow instance |
| State field | One value per state |
| Workflow definition field | Shared by all workflows; reached from another record through a custom field of type workflow |

- Workflow and state fields cannot be used in actions or conditions that run on client triggers.
- With **Store Value** cleared, a field keeps its value only for the current instance.
- Typical uses: remembering a record that the workflow created so it can subscribe to it, and keeping
  the value returned by a custom action.

## Filtering by event type and context

Initiation, actions and transitions can each be limited by event type and by execution context. The
contexts field offers the same execution contexts that SuiteScript uses (see
[N/runtime](../modules/N_runtime.md)). Restrict contexts to the ones you need; it improves
performance.

## Record mode: dynamic, mostly

Workflows nearly always work on the record in dynamic mode. Consequences:

- Each action sees the sourcing and calculations triggered by earlier actions.
- Field-setting actions should follow the order a user would fill the form in, for example the company
  before the contact. Setting a dependent field before its parent fails with an invalid reference key
  error.
- Actions and transitions on the before-record-load trigger do not run in dynamic mode.
- In standard (non-dynamic) mode, the effects are applied in a fixed order when the record is saved.

## Revisions

Every saved change bumps the workflow's revision number:

- Workflows created before 2017.2 start at revision 0.
- A new workflow is at revision 2 once its first state exists.
- Each further change adds 1.

What counts as a change: editing the definition; adding, changing or moving a state; changing a
transition, an action or an action group, a custom field or a condition; changing the script ID;
installing or updating the workflow from a bundle; and activating or inactivating it.

You can see the revision on the workflow's History subtab, in the Revision column of the workflows
list, and through a workflow definition search.
